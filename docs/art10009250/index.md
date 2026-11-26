---
vid: '10009250'
title: Using Delegates in an Interpreted Language
authors:
- Richard Smith
byline: by Richard Smith
volume: '22'
issue: '1'
page: '49'
source: trad/v221/Delegates.htm
converter: vecrescue 0.1.0
issue_tab: n1-november-2005
tags:
- implementations
- system interfaces
---

*by Richard Smith*
{ .byline }

## Introduction

It is essential that an interpreter can handle events. For example, one should be able to react to a button being pressed – in Rowan:

```
button←#new Button[];
button:Text←"Hello, world";
button:Click←#delegate $EventHandler {sender message: ⍇sender:Text };
```

... the event handler must be called when the button is clicked – we expect to see `Hello World` in the session.

In .Net, events are represented by *delegates*, which are effectively typed function pointers. For example, to react to a button being clicked in C#:

```
void Start(){
 button.Click = new EventHandler(myButtonClick);
}

void myButtonClick(object sender, EventArgs e){
 // ... handle the event, do something cool
}
```

The type *System.EventHandler* is a delegate, and to attach a handler to an event you must create an instance of the correct delegate type. Unfortunately for an interpreter, the *new* in the example above requires the address of a method which has the appropriate signature – which must exist at compile time! This article shows how to create a delegate at runtime, which is necessary for an interpreter to use them.

## How it Works

It isn’t possible to have a general event handler in the .Net world, whereas it is possible in ordinary Windows to have a general message-handling function which does broadly the same job. The method a delegate refers to must have the correct signature, and since there is no way to know beforehand what those signatures may be we need a way to create new methods at runtime.

Fortunately, the Framework does provide the *System.Reflection.Emit* namespace to emit IL (the high-level assembler C# is compiled to) directly in a temporary assembly in memory. So what we have to do is make a new type which defines a method of the correct signature and creates a delegate from it.

## The Code

Here I walk through the C# code of the *CreateDelegate* method, which builds a new type that exposes a delegate of the requested type. Within the comments of this method are the lines of C# which the emitted IL represent.

```
internal static Delegate CreateDelegate(IEngine engine, Type type, Symbol verb){
 if(!type.IsSubclassOf(typeof(Delegate))) throw new ArgumentException("Type "+type.FullName+" is not a delegate");
 SetUp();
```

Make sure what we have been asked for is a delegate, and set up the temporary assembly if we haven’t already done so.

```
 if((verb.type & ObjectType.Token) != 0){
  verb.type = ObjectType.Token | ObjectType.StringRep;
 } else {
  verb.intValue = TokenType.Verb;
  verb.type = ObjectType.TokenTypeGiven | ObjectType.StringRep;
 }

 DelegateIdentifier di = new DelegateIdentifier(type, verb);
 object o = saved[di];
 if(o is Delegate) return (Delegate)o;
```

Check if we’ve already made a delegate from this verb, and if so return it. *saved* is a *Hashtable* which caches previous delegate requests.

```
 String name = "Type"+(index++);
 TypeBuilder ty = module.DefineType(name);
 FieldBuilder fb = ty.DefineField("e", typeof(IRowanEngine), FieldAttributes.Static | FieldAttributes.Private);
 FieldBuilder delegate_f = ty.DefineField("d", type, FieldAttributes.Static | FieldAttributes.Private);
 FieldBuilder verbSym_f = ty.DefineField("v", typeof(Symbol), FieldAttributes.Static | FieldAttributes.Private);
```

Set up the new type (with a unique name) and some fields it needs. *e* is the Rowan engine it will call back to when the event is fired, *d* is the stored delegate and *v* is the verb which is to be called. I use one-character names to save a few bytes of memory, as these names should never see the light of day.

```
 // Get the Invoke method of the delegate and crib its parameter list
 MethodInfo mi = type.GetMethod("Invoke");
 ParameterInfo[] pia = mi.GetParameters();
 Type[] paramtypes = new Type[pia.Length];
 for(int i = 0; i < pia.Length; i++) paramtypes[i] = pia[i].ParameterType;
```

Take the parameter list of the appropriate Delegate type and copy it. The signature of a Delegate is the same as that of its Invoke method.

```
 // private static {rettype} Handler({params}){
 MethodBuilder Handler_m = ty.DefineMethod("H",  MethodAttributes.Static | MethodAttributes.Private, mi.ReturnType, paramtypes);
 ILGenerator ILgen = Handler_m.GetILGenerator();
```

Create a new method called *H* and prepare to generate the IL for it. *H* will be the method we attach to the delegate, so we set the return type and parameter types to what we just copied from *Invoke*.

```
 // Make symbols out of the objects, and bundle up as 'right arg'
 // ...
```

Emit the *H()* method with a series of calls to *ilGen.Emit()*. This method packages up all its parameters into an array and calls the Rowan function in *v* with it as the right argument. See the complete code at the end of the article for details of which IL codes are emitted; I haven’t included it here because the details of what the handler does are Rowan-specific.

```
 // public static void InitS(IEngine arg0, Symbol arg1)
 MethodBuilder m = ty.DefineMethod("InitS", MethodAttributes.Static | MethodAttributes.Public, typeof(void), new Type[]{typeof(IEngine), typeof(Symbol)});
 ILgen = m.GetILGenerator();
 // Engine = arg0;
 ILgen.Emit(OpCodes.Ldarg_0);
 ILgen.Emit(OpCodes.Stsfld, fb);
 // verbSym = arg1;
 ILgen.Emit(OpCodes.Ldarg_1);
 ILgen.Emit(OpCodes.Stsfld, verbSym_f);
 // delegate = new {delegatetype} (Handler)
 ILgen.Emit(OpCodes.Ldnull);
 ILgen.Emit(OpCodes.Ldftn, Handler_m);
 ILgen.Emit(OpCodes.Newobj, type.GetConstructor(new Type[]{typeof(object), typeof(IntPtr)}));
 ILgen.Emit(OpCodes.Stsfld, delegate_f);
 // return;
 ILgen.Emit(OpCodes.Ret);
 // }
```

Emit an initialisation method *InitS()* which sets the *e*, *d* and *v* fields. This, *d* and *H()* (above) are all declared as static, so we don’t need to create an instance of our new type to use the delegate it exposes.

```
 // public static Delegate GetDelegate(){
 m = ty.DefineMethod("GetDelegate", MethodAttributes.Static | MethodAttributes.Public, typeof(Delegate), new Type[]{});
 ILgen = m.GetILGenerator();

 // return delegate;
 ILgen.Emit(OpCodes.Ldnull);
 ILgen.Emit(OpCodes.Ldfld, delegate_f);
 ILgen.Emit(OpCodes.Ret);
 // }
```

Emit a method *GetDelegate()*, which just returns the delegate in *d*.

```
 Type finalType = ty.CreateType();
```

Create the type we’ve been building. It is now possible to call static methods defined within it, or create an instance of it, but it is no longer possible to add new methods to it.

```
 finalType.InvokeMember("InitS",  BindingFlags.Public | BindingFlags.Static | BindingFlags.InvokeMethod, null, null, new object[]{engine, verb});
 Delegate res = (Delegate)finalType.InvokeMember("GetDelegate", BindingFlags.Public | BindingFlags.Static | BindingFlags.InvokeMethod, null, null, new object[]{});
 saved[di] = res;
 return res;
}
```

Call the *InitS()* method of the new type to initialise the fields, and then call *GetDelegate()* to retrieve the delegate and return it. We also save this delegate in our cache.

And that’s it, basically! A call to *CreateDelegate()* can create a delegate of any type, even custom delegate types from third party controls. For another interpreted language, you’d need to change the type of *v* to your own language’s function type, and modify the emitted method *H()* to call back into your engine instead of the Rowan one.

## Appendix: Complete Code of the Internals Class

This is the complete C# code of the class which supports the production of delegates in Rowan at runtime.

```
internal class Internals {
 private static AssemblyBuilder assembly = null;
 private static ModuleBuilder module = null;
 private static bool set_up = false;
 private static int index = 0;

 private static AssemblyBuilderAccess access = AssemblyBuilderAccess.Run | AssemblyBuilderAccess.Save;

 private static Hashtable saved;

 private static void SetUp(){
  if(set_up) return;
  AssemblyName name = new AssemblyName();
  name.Name = "RowanDynamicAssembly";
  assembly = AppDomain.CurrentDomain.DefineDynamicAssembly(name, access);
  module = assembly.DefineDynamicModule("RowanDynamicModule", "dynmodule.dll");
  set_up = true;
  saved = new Hashtable();
 }

 internal static Delegate CreateDelegate(IEngine engine, Type type, Symbol verb){
  if(!type.IsSubclassOf(typeof(Delegate))) throw new ArgumentException("Type "+type.FullName+" is not a delegate");
  SetUp();

  if((verb.type & ObjectType.Token) != 0){
   verb.type = ObjectType.Token | ObjectType.StringRep;
  } else {
   verb.intValue = TokenType.Verb;
   verb.type = ObjectType.TokenTypeGiven | ObjectType.StringRep;
  }

  DelegateIdentifier di = new DelegateIdentifier(type, verb);
  object o = saved[di];
  if(o is Delegate) return (Delegate)o;

  String name = "Type"+(index++);
  TypeBuilder ty = module.DefineType(name);
  FieldBuilder fb = ty.DefineField("e", typeof(IRowanEngine), FieldAttributes.Static | FieldAttributes.Private);
  FieldBuilder delegate_f = ty.DefineField("d", type, FieldAttributes.Static | FieldAttributes.Private);
  FieldBuilder verbSym_f = ty.DefineField("v", typeof(Symbol), FieldAttributes.Static | FieldAttributes.Private);

  // Get the Invoke method of the delegate and crib its parameter list
  MethodInfo mi = type.GetMethod("Invoke");
  ParameterInfo[] pia = mi.GetParameters();
  Type[] paramtypes = new Type[pia.Length];
  for(int i = 0; i < pia.Length; i++) paramtypes[i] = pia[i].ParameterType;
  // private {rettype} Handler({params}){
  MethodBuilder Handler_m = ty.DefineMethod("H",  MethodAttributes.Static | MethodAttributes.Private, mi.ReturnType, paramtypes);
  ILGenerator ILgen = Handler_m.GetILGenerator();
  // Make symbols out of the objects, and bundle up as 'right arg'
  // Make array of tokens like "f ( p1 p2 p3 )"
  // ra = new RArray();
  LocalBuilder ra = ILgen.DeclareLocal(typeof(RArray));
  ILgen.Emit(OpCodes.Ldloca_S, ra);
  ILgen.Emit(OpCodes.Initobj, typeof(RArray));
  // ra.contents = new Symbol[{no of params}]
  ILgen.Emit(OpCodes.Ldloca_S, ra);
  ILgen.Emit(OpCodes.Ldc_I4_S, pia.Length);
  ILgen.Emit(OpCodes.Newarr, typeof(Symbol));
  ILgen.Emit(OpCodes.Stfld, typeof(RArray).GetField("contents"));
  // {for each parameter}
  for(int i = 0; i < pia.Length; i++){
   // ra.contents[{i}] = Symbol.CreateFromObject({params[i]})
   ILgen.Emit(OpCodes.Ldloca_S, ra);
   ILgen.Emit(OpCodes.Ldfld, typeof(RArray).GetField("contents"));
   ILgen.Emit(OpCodes.Ldc_I4_S, i);
   ILgen.Emit(OpCodes.Ldelema, typeof(Symbol));
   ILgen.Emit(OpCodes.Ldarg_S, i);
   ILgen.EmitCall(OpCodes.Call, typeof(Symbol).GetMethod("CreateFromObject"), null);
   ILgen.Emit(OpCodes.Stobj, typeof(Symbol));
  }
  // ArrayList al = new ArrayList();
  LocalBuilder al = ILgen.DeclareLocal(typeof(ArrayList));
  ILgen.Emit(OpCodes.Newobj, typeof(ArrayList).GetConstructor(new Type[]{}));
  ILgen.Emit(OpCodes.Stloc_S, al);
  // al.Add(verbSym);
  ILgen.Emit(OpCodes.Ldloc_S, al);
  ILgen.Emit(OpCodes.Ldsfld, verbSym_f);
  ILgen.Emit(OpCodes.Box, typeof(Symbol));
  ILgen.EmitCall(OpCodes.Callvirt, typeof(ArrayList).GetMethod("Add"), null);
  ILgen.Emit(OpCodes.Pop);
  // al.Add(ra.getSymbol());
  ILgen.Emit(OpCodes.Ldloc_S, al);
  ILgen.Emit(OpCodes.Ldloca_S, ra);
  ILgen.EmitCall(OpCodes.Call, typeof(RArray).GetMethod("getSymbol"), null);
  ILgen.Emit(OpCodes.Box, typeof(Symbol));
  ILgen.EmitCall(OpCodes.Callvirt, typeof(ArrayList).GetMethod("Add"), null);
  ILgen.Emit(OpCodes.Pop);
  // Argument temp = new Argument(); engine.ArgumentStack.Push(temp)
  ILgen.Emit(OpCodes.Ldsfld, fb);
  ILgen.EmitCall(OpCodes.Callvirt, typeof(IRowanEngine).GetMethod("get_ArgumentStack"), null);
  LocalBuilder temp = ILgen.DeclareLocal(typeof(Argument));
  ILgen.Emit(OpCodes.Ldloca_S, temp);
  ILgen.Emit(OpCodes.Initobj, typeof(Argument));
  ILgen.Emit(OpCodes.Ldloc_S, temp);
  ILgen.EmitCall(OpCodes.Callvirt, typeof(ArgumentStack).GetMethod("Push"), null);
  // engine.PushFunction("DelegateHandler", verbSym.ns);
  ILgen.Emit(OpCodes.Ldsfld, fb);
  ILgen.Emit(OpCodes.Ldstr, "DelegateHandler");
  ILgen.Emit(OpCodes.Ldsfld, verbSym_f);
  ILgen.Emit(OpCodes.Ldfld, typeof(Symbol).GetField("ns"));
  ILgen.EmitCall(OpCodes.Callvirt, typeof(IEngine).GetMethod("PushFunction"), null);
  // try {
  Label try1 = ILgen.BeginExceptionBlock();
  //  engine.ExecuteBlock(al);
  ILgen.Emit(OpCodes.Ldsfld, fb);
  ILgen.Emit(OpCodes.Ldloc_S, al);
  ILgen.EmitCall(OpCodes.Callvirt, typeof(IEngine).GetMethod("ExecuteBlock"), null);
  ILgen.Emit(OpCodes.Leave, try1);
  // } catch(RException rx) {
  ILgen.BeginCatchBlock(typeof(RException));
  //  throw rx.originalError;
  ILgen.Emit(OpCodes.Ldfld, typeof(RException).GetField("originalError"));
  ILgen.Emit(OpCodes.Throw);
  // }
  ILgen.EndExceptionBlock();
  // engine.PopFunction();
  ILgen.Emit(OpCodes.Ldsfld, fb);
  ILgen.EmitCall(OpCodes.Callvirt, typeof(IEngine).GetMethod("PopFunction"), null);
  // engine.ArgumentStack.Pop() ... left on stack
  ILgen.Emit(OpCodes.Ldsfld, fb);
  ILgen.EmitCall(OpCodes.Callvirt, typeof(IRowanEngine).GetMethod("get_ArgumentStack"), null);
  ILgen.EmitCall(OpCodes.Callvirt, typeof(ArgumentStack).GetMethod("Pop"), null);
  if(mi.ReturnType != typeof(void)){
   // (return engine.ArgumentStack.Pop() ... ) .result;
   ILgen.Emit(OpCodes.Stloc_S, temp);
   ILgen.Emit(OpCodes.Ldloca_S, temp);
   ILgen.Emit(OpCodes.Ldfld, typeof(Argument).GetField("result"));
   LocalBuilder res_v = ILgen.DeclareLocal(typeof(Symbol));
   //ILgen.Emit(OpCodes.Stloc_S, res_v);
   //ILgen.Emit(OpCodes.Ldloc_S, res_v);
  } else ILgen.Emit(OpCodes.Pop);

  ILgen.Emit(OpCodes.Ret);
  // }

  // public static void InitS(IEngine arg0, Symbol arg1)
  MethodBuilder m = ty.DefineMethod("InitS", MethodAttributes.Static | MethodAttributes.Public, typeof(void), new Type[]{typeof(IEngine), typeof(Symbol)});
  ILgen = m.GetILGenerator();
  // Engine = arg0;
  ILgen.Emit(OpCodes.Ldarg_0);
  ILgen.Emit(OpCodes.Stsfld, fb);
  // verbSym = arg1;
  ILgen.Emit(OpCodes.Ldarg_1);
  ILgen.Emit(OpCodes.Stsfld, verbSym_f);
  // delegate = new {delegatetype} (Handler)
  ILgen.Emit(OpCodes.Ldnull);
  ILgen.Emit(OpCodes.Ldftn, Handler_m);
  ILgen.Emit(OpCodes.Newobj, type.GetConstructor(new Type[]{typeof(object), typeof(IntPtr)}));
  ILgen.Emit(OpCodes.Stsfld, delegate_f);
  // return;
  ILgen.Emit(OpCodes.Ret);
  // }

  // public static Delegate GetDelegate(){
  m = ty.DefineMethod("GetDelegate", MethodAttributes.Static | MethodAttributes.Public, typeof(Delegate), new Type[]{});
  ILgen = m.GetILGenerator();

  // return delegate;
  ILgen.Emit(OpCodes.Ldnull);
  ILgen.Emit(OpCodes.Ldfld, delegate_f);
  ILgen.Emit(OpCodes.Ret);
  // }

  Type finalType = ty.CreateType();
  finalType.InvokeMember("InitS",  BindingFlags.Public | BindingFlags.Static | BindingFlags.InvokeMethod, null, null, new object[]{engine, verb});
  Delegate res = (Delegate)finalType.InvokeMember("GetDelegate", BindingFlags.Public | BindingFlags.Static | BindingFlags.InvokeMethod, null, null, new object[]{});
  saved[di] = res;
  return res;
 }

 internal static void SaveAssembly(String filename){
  assembly.Save(filename);
 }

 private struct DelegateIdentifier {
  Type Type; VerbInfo Verb;

  internal DelegateIdentifier(Type type, Symbol verb){ Type = type; Verb = (VerbInfo)verb.Value; }
 }
}
```
