---
vid: '10500590'
title: Cryptography in Dyalog APL and .Net
authors:
- Dan Baronet
byline: Dan Baronet (danb@dyalog.com)
volume: '25'
issue: '1'
page: '54'
received: '2010-03-16'
online: '2011-05-01'
editor: Stephen Taylor
description: Cryptography and how to use it in Dyalog APL and .Net.
abstract: Cryptography and how to use it in Dyalog APL and .Net. This is only an introduction to the subject. The functions presented here are only to play with.
source: content/printed/251/baronet.htm
converter: vecrescue 0.1.0
issue_tab: n1-june-2011
tags:
- programming techniques
- system interfaces
---

Dan Baronet (danb@dyalog.com)
{ .byline }

Cryptography and how to use it in Dyalog APL and .Net. This is only an introduction to the subject. The functions presented here are only to play with.
{ .abstract }

Cryptography deals with Encryption and Decryption. Encryption is the process of converting ordinary data (*plaintext*) into unreadable form (*ciphertext*). Decryption is the reverse. A *cipher* is a pair of algorithms to create the encryption and the decryption. The algorithms are controlled by a *key* or by a pair of *keys*. Ciphers without a key can be easily broken (knowing the cipher) so keys are very important. Ciphers can be used directly for encryption or decryption without additional procedures but it is a good idea to add authentication or integrity checks to make them more secure. Another category of algorithms, used for hashing, do not have a key. All these algorithms are the primitives of cryptography.

## Symmetric-key algorithms

Symmetric-key cryptography refers to encryption methods in which both the sender and receiver share the same key. They come in several flavours, some with better security in one aspect or another than others. There are two main types: block ciphers and stream ciphers.

Block ciphers work in sections (block) and include the Data Encryption Standard (DES) and the Advanced Encryption Standard (AES), designs which have been designated standards by some governments. Despite its deprecation, DES and the more secure triple-DES variant remain quite popular and are used across a wide range of applications. Many other such ciphers have been designed and released, with considerable variation in quality. Many have been cracked.

These block cipher algorithms can directly be used for the encryption and decryption of a data block without additional procedures. However, this is usually not advisable, because identical plaintext blocks encrypt into identical ciphertext blocks, out of which attackers might draw conclusions. To avoid this, these algorithms are used in operation modes, which is somewhat similar to the use of APL primitives in APL operators. Padding schemes can be used on the last block to allow the encryption of plaintexts not matching a multiple of the block size.

Other modes convert a block cipher into a stream cipher. These can continuously encrypt data on a byte or even a bit basis. It is common to all these modes to require (besides the key) an initialization vector (IV). This is a randomly chosen byte sequence in the length of a block.

Symmetric-key systems use the same key for encryption and decryption. A drawback of symmetric ciphers is the key management necessary to use them securely. Each distinct pair of communicating parties must, ideally, share a different key, and perhaps each ciphertext exchanged as well. The number of keys required increases as the square of the number of network members, which very quickly requires complex key management schemes to keep them all straight and secret. The difficulty of securely establishing a secret key between two communicating parties, when a secure channel doesn't already exist between them, also presents a chicken-and-egg problem. So, in practice, most uses have a short life cycle.

## Asymmetric or public-key cryptography

In 1976 was proposed the notion of public-key cryptography in which two different but mathematically related keys are used: a public key and a private key.

In this system, the public key may be freely distributed, while its paired private key must remain secret. The public key is typically used for encryption only, while the private or secret key is used for decryption.

In addition to encryption, public-key cryptography can be used to implement digital signature schemes. A digital signature is reminiscent of an ordinary signature; they both have the characteristic that they are easy for a user to produce, but difficult for anyone else to forge. Digital signatures can also be permanently tied to the content of the message being signed; they cannot then be 'moved' from one document to another, for any attempt will be detectable. In digital signature schemes, there are two algorithms: one for signing, in which a secret key is used to process the message (or a hash of the message, or both), and one for verification, in which the matching public key is used with the message to check the validity of the signature. RSA and DSA are two of the most popular digital signature schemes.

Most public-key algorithms involve operations which are much more computationally expensive than the techniques used in most block ciphers, especially with typical key sizes. As a result, public-key cryptosystems are commonly *hybrid* cryptosystems, in which a fast high-quality symmetric-key encryption algorithm is used for the message itself, while the relevant symmetric key is sent with the message, but encrypted using a public-key algorithm. Similarly, hybrid signature schemes are often used, in which a cryptographic hash function (see below) is computed, and only the resulting hash is digitally signed.

## Hash algorithms

Cryptographic hash functions are a third type of cryptographic algorithm. They take a message of any length as input, and output a short, fixed length hash which can be used e.g. as a digital signature. MD4 is a long-used hash function which is now cracked; MD5, a strengthened variant of MD4, is also widely used but cracked in practice. SHA-0 was a flawed algorithm that was withdrawn; SHA-1, now considered weak, is widely deployed and more secure than MD5, the SHA-2 family improves on it, and there’s SHA-3 on the way.

## Combining cryptographic primitives

By themselves, cryptographic primitives are quite limited. For instance, a bare encryption algorithm will provide no authentication mechanism, nor any explicit message integrity checking. Only when combined in security protocols, can more than one security requirement be addressed. For example, to transmit a message that is not only encoded but also protected from tinkering (i.e. it is confidential and integrity-protected), an encoding routine, such as DES, and a hash-routine such as SHA-1 can be used in combination. If the attacker does not know the encryption key, he cannot modify the message so that message hashed values can't be successfully faked.

## Example of asymmetric algorithm: RSA

The RSA algorithm is among the most widely used. It involves three steps: key generation, encryption and decryption.

The following uses the notion of congruence. Two integers a and b are said to be congruent modulo n, if their difference a − b is an integer multiple of n, i.e. they have the same remainder.

## Key generation

RSA is known as a deterministic encryption algorithm, i.e. it has no random component. It involves a *public* key and a *private* key. The public key can be known to everyone and is used for encrypting messages. Messages encrypted with the public key can *only* be decrypted using the private key. The keys for the RSA algorithm are generated the following way:

1. Choose two distinct prime numbers p and q.
2. Compute n = p × q. n is used as the modulus for both the public and private keys.
3. Compute the totient[\[1\]](#ref1) of n, T = (p-1)×(q-1).
4. Choose an integer e such that 1&lt;e &lt; T, and e and T share no divisors other than 1 (i.e. e and T are *coprimes*). e is released as the public key exponent.
5. Determine d which satisfies the congruence of de and 1 modulo T. In other words, de − 1 can be evenly divided by the totient T= (p − 1)x(q − 1). d is kept as the private key exponent.

The *public key* consists of the modulus n and the public (or encryption) exponent e. The *private key* consists of the modulus n and the private (or decryption) exponent d which must be kept secret.

## Encryption

Adam transmits his public key (n,e) to Bea and keeps the private key secret. Bea then wishes to send message m[\[2\]](#ref2) to Adam. She computes the ciphertext c corresponding such that c and m<sup>e</sup> are congruent modulo n.

Bea then transmits c to Adam. If Adam's ex Eve were to <em>eaves</em>drop on the transmitted message she could make no sense of it.

## Decryption

Adam can recover m from c by using his private key’s exponent d like this:

We know m and c<sup>d</sup> are congruent mod n because c<sup>d</sup> and m<sup>ed</sup> are congruent modulo n.

Since ed = 1 + kT, then m<sup>ed</sup> is congruent modulo n, to m<sup>1+kT</sup> or m(m<sup>T</sup>)<sup>k</sup> or simply m.

The last congruence directly follows from Euler’s theorem when m is relatively prime to n. It can then be shown that the equation holds for all m.

This shows that we get the original message back as c<sup>d</sup> and m are congruent modulo n.

This is a simplified example. In reality there are all kinds of ways to complicate matters and make the life of a would-be attacker hell. For example, the choice of prime numbers should be large and the plaintext should be padded with random numbers.

## A working example

Here is an example of RSA encryption and decryption in J. The parameters used here are artificially small, but one can also use OpenSSL to generate and examine a real key pair.

We’ll use the verbs

```
tot =:  */ @: <:
cong=: =/@:|
NB. the (2) numbers (right arg) have the same remainder mod n (left arg)
xgcd=: 3 : 0
xy=. y|.~>/y NB. smaller first
 0 1         NB. default result
if. 0<{: t=. |/\xy do.
 'X Y'=. xgcd t
 Y, X-Y*<.%~/xy
end.
)
```

1. Choose two prime numbers, eg: `p =: 61` and `q =: 53`
2. Compute `n =: p*q NB. =3233`
3. Compute the totient `T =: tot p,q NB. (61-1)*(53-1) =3120`
4. Choose e > 1 coprime to 3120, here we pick a value not too small e = 17
5. Compute d such that `T cong 1,d*e` e.g., by computing the modular multiplicative inverse of e modulo T:

```
   d =:  T|{: xgcd e,T    NB. 2753
```

since 46801 = 17×2753 and 1 = 3120|46801 this is the correct answer.

The public key is (n = 3233, e = 17). The encryption statement is:

```
   c =: n|m^e
```

The private key is (n = 3233, d = 2753). The decryption statement is:

```
   m =: n|c^d
```

For example, to encrypt m = 123, we calculate

```
   c=: 3233 | 123x ^ 17    NB. 855
```

To decrypt c = 855, we calculate.

```
   m=: 3233 | 855x ^ 2753  NB. 123
```

In real life situations the primes selected would be much larger, however in our example it would be relatively trivial to factor n, 3233, obtained from the freely available public key back to the primes p and q. Given e, also from the public key, we could then compute d and so acquire the private key. The message, also, would be a much larger integer (think of a character string as a long list of bits representing a large integer).

## .Net

The previous example is fairly easy to program in any language as long as the numbers are kept small. Otherwise special routines must be written to handle large numbers like the ones represented by long strings. Plus there are a number of issues that must be dealt with regarding security. Without getting into too many details let’s say that it is not easy to come up with an acceptable solution. This is where ready-made solutions like .Net come in the picture.

.Net offers a series of classes to handle cryptography. They reside in `System.Security.Cryptography` which is in `System.Core.dll`

Let's first define a few utilities, including a *hash* function:

```
:Namespace CryptoTools

   ∇ r←DNetCrypto ⍝ This is the location of the cryptography libraries
     r←'System.Security.Cryptography,\Program Files' ⍝ change for 64b OS
     r,←'\Reference Assemblies\Microsoft\Framework\v3.5\System.Core.dll'
   ∇
```

```
   ∇ value←hash string;str;⎕USING
    ⍝ Return hash value of the string given as arg
     :If isChar str←,string
         str←⎕UCS str ⍝ turn characters into numbers for the hash fn
         {}⎕DR str    ⍝ kludge for V12 to ensure str is small int
     :EndIf
     ⎕USING←DNetCrypto                          ⍝ also SHA1/384/512, CRC32
     value←(⎕NEW SHA256Managed).ComputeHash⊂str ⍝ MD5CryptoServiceProvider
   ∇

   if←/⍨ ⋄ UTF8←'UTF-8'∘⎕ucs ⋄ isChar←{∨/0 2=10|⎕DR 1/⍵}
   UCSN←{~isChar ⍵:⍵ ⋄ v{⍺}⎕dr v←UTF8 ⍵} ⍝ ⎕DR forces demotion (V12)

:EndNamespace
```

The hash function (here SHA256, but it could be another) can be applied to any string:

```
      CryptoTools.hash  ⊃,/⎕src CryptoTools
165 157 223 218 183 249 78 22…
```

The following class will handle symmetric cryptography:

```
:Class  CodeSymmetric

    :include CryptoTools

    M2MS←{8÷⍨⍵[2]+0,s×⍳(-/2↑⍵)÷1⌈s←¯1↑⍵} ⍝ valid Key sizes [min-max]
    ∇ boa provider;choices;msg;n
      :Access public
      :Implements constructor
⍝ Data Encryption Standard (DES) supports a 64 bit key only
⍝ Rivest Cipher 2 provider supports keys from 40 to 128* bits
⍝ Rijndael (also known as AES) provider supports keys 128/192/256*
⍝ TripleDES provider (also known as 3DES) supports keys of 128/192*
      choices←'DES' 'RC2' 'Rijndael' 'TripleDES'
      msg←'Invalid provider; choose one of',⍕choices
      msg ⎕SIGNAL 99 if(⍴choices)<n←choices⍳⊂provider
      ⎕USING←DNetCrypto
      choices←DESCryptoServiceProvider RC2CryptoServiceProvider
      _algo←⎕NEW n⊃choices,RijndaelManaged TripleDESCryptoServiceProvider

      n←⊃_algo.LegalKeySizes.(MaxSize MinSize SkipSize)
      _vks←M2MS n   ⍝ all valid Key sizes

      IV←'1Az=-@qT' ⍝ Initialisation Vector

      ⎕DF provider  ⍝ conveniently identify instance
    ∇
```

```
    ∇ r←RandomKey ⍝ This generates a random Key
      :Access public
      _algo.GenerateKey
      r←_algo.Key
    ∇

    :property Key ⍝ The key used to encrypt/decrypt data
    :access public
        ∇ r←get
          r←_key
        ∇
        ∇ set val;val;msg;t
          msg←'⍴Key must be ',((1<⍴t)/'one of '),⍕t←_vks
          msg ⎕SIGNAL 11 if~(⍴val←val.NewValue)∊t
          _algo.Key←_key←UCSN val
        ∇
    :endproperty
```

```
   ⍝ Using the default Cipher Block Chaining (CBC) mode, all data blocks
   ⍝ are processed using the value derived from the previous block; the
   ⍝ first data block has no previous data block to use, so it needs an
   ⍝ Initialisation Vector (IV) to feed the first block

    :property IV
    :access public
        ∇ r←get
          r←_iv
        ∇
        ∇ set Value;val;validBS
        ⍝ We must ensure the value fits requirements:
          validBS←M2MS⊃_algo.LegalBlockSizes.(MaxSize MinSize SkipSize)
          :If ~validBS∊⍨⍴val←Value.NewValue ⍝ if not a valid block size
              val←validBS[1++/validBS<⍴val]⍴val ⍝ pick the next one up
          :EndIf
          _algo.IV←_iv←UCSN val
        ∇
    :endproperty
```

```
⍝ Encrypts the specified Data using preset key and preset IV

    ∇ r←EncryptString d;⎕USING;ms;cs;cr
      :Access public
      ⎕USING,⍨←⊂'System'

     ⍝ The key and IV have better been set
      ms←⎕NEW IO.MemoryStream
      cr←_algo.CreateEncryptor ⍬
      cs←⎕NEW CryptoStream(ms cr CryptoStreamMode.Write)
      cs.Write((UCSN d)0,⍴d)
      cs.Close
      ms.Close
      r←ms.ToArray
    ∇
```

```
    ⍝ Decrypts the specified data using preset key and preset IV
    ∇ r←DecryptCipher encryptedData;b;ms;cs;len;⎕USING
      :Access public
      ⎕USING,⍨←'System' 'Dyalog'
      ms←⎕NEW IO.MemoryStream(encryptedData 0,⍴encryptedData)
     cs←⎕NEW CryptoStream(ms(_algo.CreateDecryptor ⍬)CryptoStreamMode.Read)
      b←⎕NEW ByRef,⊂⊂3/⍨⍴encryptedData
      len←cs.Read(b 0,⍴encryptedData)
      cs.Close
      r←⎕UCS len↑b.Value
    ∇
:EndClass
```

Let’s try it:

```
      s1←⎕new CodeSymmetric  'TripleDES'
      s1.Key←16⍴'secret'
      +cs1←s1.EncryptString 'Let''s rendez-vous at midnight'
155 230 195 207 193 180 216 228 2 14 11…
      s1.DecryptCipher cs1
Let's rendez-vous at midnight
```

Any instance of the class will do to decrypt the cipher as long as the Initialisation Vector and the key are the same.

Obviously, that code could be modified to, say, accept the Initialisation Vector and/or the key at instantiation time.

The following class will handle asymmetric cryptography:

```
:Class CodeAsymmetric
⍝ The only provider supported is the RSACryptoServiceProvider.

    :include CryptTools
    ⎕using← DNetCrypto ⋄ ⎕io ⎕ml←1 ⋄ ⎕wx←3
    ∇ boa0 ⍝ The public Key is automatically generated
      :Implements constructor
      :Access public
      _rsa←⎕NEW RSACryptoServiceProvider
      GenerateNewKeys
    ∇

    ∇ boa1 arg ⍝ This is where you make the public Key.
  ⍝ It could be made of INI files, XML, etc.
  ⍝ Here we only accept XML strings.
      :Implements constructor
      :Access public
      _rsa←⎕NEW RSACryptoServiceProvider
      _rsa.FromXmlString⊂arg
    ∇
```

```
  ⍝ Generates a new public/private key pair as strings
    ∇ {(publicKeyXML privateKeyXML)}←GenerateNewKeys
  ⍝ Generate new keys for this instance
      :Access public
      publicKeyXML←_rsa.ToXmlString 0
      privateKeyXML←_rsa.ToXmlString 1
    ∇

    ∇ r←Decrypt cipher
      :Access public
      r←UTF8 _rsa.Decrypt cipher 0
    ∇

    ∇ r←Encrypt d
      :Access public
      r←_rsa.Encrypt((UCSN d)0)
    ∇

:EndClass
```

Now, Adam creates an object with both public and private keys:

```
      adam←⎕new CodeAsymmetric
      (pub  pvt)← adam.GenerateNewKeys
```

Adam forwards the public key to Bea who uses it to send him messages:

```
      bea←⎕new   CodeAsymmetric    pub
      +msg←bea.Encrypt  'meet me at midnight'
89 12 181 136 53 …
```

Adam decrypts the message like this:

```
      adam.Decrypt  msg
meet me at midnight
```

That’s how easy it is. Because asymmetric cryptography is calculation-intensive it is best to limit the material to small strings.

Now, in real life messages tend to be longer and symmetric cryptography works best. What some people do is to encrypt the large text symmetrically with a key which is encrypted asymmetrically. They often also add a signature, for example the hash of the message and/or encrypted key to ensure everything is kosher. Something along the lines of

```
      beamsg←1000⍴'long message... '
      code←⎕new CodeSymmetric 'RC2'
      code.Key←cpw←'secret'
      ⍴cryptedmsg←code.EncryptString beamsg
1008
      ⍴cryptedkey←bea.Encrypt cpw
128
      ⍴H←CryptoTools.hash cryptedkey,cryptedmsg
32
```

She then sends Adam the encrypted message, the encrypted key and the hash. Adam first checks there's been no tampering:

```
      H≡CryptoTools.hash cryptedkey,cryptedmsg
1
```

He then finds the key used to encrypt the message:

```
      adam.Decrypt cryptedkey
secret
```

Then he finally decodes the message:

```
      decode←⎕new CodeSymmetric 'RC2'
      decode.Key←'secret'
      decode.DecryptCipher cryptedmsg
long message... long message... long message...
```

That is the basis of message exchange protocols like SSL which will bundle up all the stuff to transfer, adding their own packaging information.

## Epilogue

The topic of cryptography is fairly complex. There are many issues related to this which are out of the scope of this article.

Feel free to play with this; there are a number of methods to en/decrypt streams (files) and others. Have a look.

## Notes

1. <a id="ref1"></a> The totient (usually denoted φ) of a number is defined as the number of coprimes of that number. For a prime number P it is P-1.
2. <a id="ref2"></a> Here the Message is turned into an (large) integer using a technique known as "padding" which is irrelevant to the description
