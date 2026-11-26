---
vid: '10500040'
title: ISO-9000 certified APL development
subtitle: How to achieve Quality Assurance in a small company / or How to do the boring bits and get on with the fun stuff
authors:
- Chris Hogan
byline: Chris Hogan (chris.hogan@4xtra.com)
volume: '24'
issue: '1'
page: '41'
description: Article submitted to vector.org.uk
abstract: 'That light-hearted sub-title is to point out from the start what this paper isn’t about: it’s not about making you change the way you develop APL software.'
source: content/printed/241/isocert.htm
converter: vecrescue 0.1.0
issue_tab: n1-december-2008
tags:
- development practice
---

How to achieve Quality Assurance in a small company
{ .subtitle }

*or* How to do the boring bits and get on with the fun stuff
{ .subtitle }

Chris Hogan (chris.hogan@4xtra.com)
{ .byline }

That light-hearted sub-title is to point out from the start what this paper isn’t about: it’s not about making you change the way you develop APL software.
{ .abstract }

## What is it that makes APL so good for software development?

Well, that is a sufficiently large and contentious topic for a raft of papers. I’ll limit my remarks to a few obvious points. I’m not trying for a rigorous set of definitions, just a simple list we can all agree with:

- APL’s functional richness gives compact code and high productivity:
- Which in turn leads to small teams as there is no point chopping work up if a few people can do it, not when larger teams lead to longer communication lines or adding extra developers results in information overload by rushing the development.
- The immediacy of function results makes it easy to incorporate non-technical business experts into the team to interpret and guide the developers. Not pair programming as advocated by XP perhaps, but certainly what we might call pair development is a common feature of APL projects. This simplifies communications at a vital stage of project evolution.
- The fact that defined functions and operators are a normal part of the invocation structure enables the developer to build rapidly from small units. When combined with the rapidity of results this means prototyping and incremental delivery are a natural way forward.
- Looking at that list we can see why APLers were pioneering modern Agile methods long before they were even called ‘RAD’. My point is that whatever our Quality Management System (QMS) does it cannot interfere with the advantages of developing in APL.

## So what is ISO 9000?

ISO 9000 started life with the British Ministry of Defence: during a war they wanted to say to any factory in the land "stop what you’re doing and make tanks/guns/bombs" and be reasonably assured that said tanks would run, the guns would fire and the bombs wouldn’t blow up while still in the factory.

In 1979 this military standard was published by the British Standards Institute as BS 5750, with the intention of applying it to peacetime manufacturing, to improve quality control in industries such as aerospace, where variations in quality can be disastrous.

BS 5750 became a European standard, EN 29000, then in 1987, went global as ISO 9000 – ISO is the International Organization for Standardization in English and the Organisation Internationale de Normalisationin in French: neither gives us “ISO”, it’s from the Greek for equal, so a link with APL already…

The way ISO 9000 worked was to require the factory, or software company, or call centre, to document, not what they made or did, but how the manufacturing / coding / telephone answering procedures were managed and to prove by record-keeping that the procedures were being followed.

This first version of ISO 9000 was essentially BS5750 with a different name. It had three main variants, numbered 9001, 9002 and 9003, depending on whether you originated new products, maintained products or just handled products with no concern for how they were made.

I use the word *products*, but the 9003 version proved popular with service companies, because you can define, say, a complaints procedure even though you don’t actually manufacture a physical product. Call centres use this to show they are handling customers in a consistent way, even if it is badly.

The emphasis was still on conformance with procedures, so a new version was published in 1994 concentrating on quality assurance via prevention, instead of just checking final outputs, but even this didn’t change the main product of ISO 9000: shed-loads of work instructions, records and an army of bureaucrats. Probably most frequent criticism of ISO 9000 was the amount of money, time and paperwork required to achieve certification. You’re probably thinking “Not good news for APLers so far”, but please bear with me,.

The current version, which appeared in 2000, combines the three variants into one, also called 9001, so an organization claiming to be ISO 9000 registered probably means ISO 9001:2000 as ISO 9000:2000 is actually the reference guide. The 2000 version has two main differences which should interest us.

Firstly, it makes ‘continuous improvement’ of the company’s performance the central purpose of a QMS. Think of it as being more proactive, rather than just reactive to reviews of the final output. From the APL point of view, we can treat all business processes as an evolving prototype.

Secondly, the standard became ‘parameterised’: instead of different standards according for the different activities companies perform, ISO 9001 is intended to include the lot, but if you can show you don’t need bits, you can leave them out and if you show you can combine bits, then combine them you can – as long as the auditor is able to find out where you’ve put them.

I said that everything has got simpler, but ISO 9000 has spawned a slew of specialised standards, some of which don’t even use the ‘9000’: ISO 10007 is for configuration management, which can be included in any ISO 9000 QMS for software development; 14000 is for environmental management; and to add to the confusion, health systems managers often refer to EN 29000 where there is a specific medical standard. I’m not going to go into the rest of the range, but I should mention TickIT, which is an ISO 9000 scheme to “suit information technology companies, especially software development”. This paper is not about TickIT, which is a lot more prescriptive and better suited to large companies with time to waste sufficient personnel to implement it. A software company can have a certified ISO 9000 QMS for its products and services without having to use TickIT.

The earlier versions of the standard were very declarative: “the company shall establish a process to…”, with little guidance on how, and even less idea of why, you were to do it. The new wording is a bit clearer and more process-driven. I would argue that developing software is a process, but equally the QMS itself can be seen as a collection of objects which change state according to a set of rules applied to them. Nothing in the standard dictates a procedural rather than object oriented approach, it is up to the developers to describe what they do and stick to it, not to change they way they work to fit the QMS. Nor is ISO 9000 hostile to the APL prototyping approach, in fact continuous improvement implies incremental delivery and refers to the QMS itself as well as the product or service quality. It is a symmetrical equivalent to the incremental development of an APL project.

## Certification

An ISO certificate is not a once-off, but has to be renewed to prove to the outside world you are sticking to the procedures you’ve defined. How can a non-IT professional comment on your work? Well:

He or she doesn’t, they just testify to all and sundry that the processes you said you would implement have been implemented, are being following and (just maybe) are having an effect.

If the auditor can follow it, then so can your clients and it should prove reassuring to them that you can prove the actions you’ve taken on their behalf to fix their problems or develop their code.

## Contents of ISO 9001

ISO 9001:2000 is only about 30 pages long, but like anything important it’s how you use it that counts. Having said you can leave out what you don’t need, a QMS must have the following:

Quality Policy
:   A formal statement by management, usually about one page long, to commit to the QMS

Quality manual
:   The guidebook to all other documents, so the auditor can see what you’ve left out or combined

Control of Documents
:   How you hand out instructions and details of the QMS to your employees

Control of Records
:   How you record what you did

Control of Nonconforming Product or Service
:   What do you do when it goes wrong

Corrective Action
:   How you fix the problem

Preventive Action
:   How you stop it happening again

Internal Audits
:   Are you following your own standards?

## Audits

A problem for the typical APL development team is that there are two types of auditing: external by a certification body and internal by company staff who are not part of the team, but how do you audit when you are only a one-man band? Or even a ten-man band, if you don’t have a dedicated QA department? Like so many things today, you use the Internet.

In HMW’s case, the Professional Contractors Group (PCG) offers a scheme where the QMS is kept locally, but backed up via Subversion, commonly used as a code repository by multi-site development teams. The Internal Auditor is a member of the PCG staff who can scan the records you’ve published and keep you on track. The external auditor can also see these records, though they often prefer it if you visit them for a brief chat. As we’ll see HMW have gone beyond Subversion to produce an interactive QMS directly available to the auditors.

## The PCG scheme

There is one unique feature of the PCG scheme, which is not a part of the general ISO 9000, but is mandatory for any QMS which is certified to PCG ISO 9000. This is the Code of Ethics, which is in effect a manifesto of how the company works. It has to be sent to every client and referred to in any contract. HMW also posts our copy of the code of ethics on our web site.

The PCG scheme supplies standard documents, such as the “Control of Documents” and “Control of Records” and templates for the Quality Policy and Manual. The scheme defines just two main work flows to cover the rest of the compulsory sections: finding work and doing work. A bit obvious isn’t it?

Finding work
:   The idea is to show that you are trying to find work you are competent to do. This just keeps all your records in one place where the auditor can find them, so you:
:   List your clients, agencies, web sites, wherever you source your potential work from. Show that the code of ethics was included in the materials sent. Perform a simple risk assessment – -does the company have the skills and resources to do the required work? Keep copies of any quotations sent.

Doing work
:   Doesn’t really add a lot more which is specific to this stage:
:   Keep copies of all contracts and other policies agreed, for example health and safety, any confidentiality agreements, etc. Any instructions give about the work you are to perform, basically log what you do, if you’re not given formal specifications, a daily work log will do. Much of what a developer does is technical and therefore is outside the remit of the QMS. Invoices show you billed for work you did and any timesheets, but only if the contract requires billing by signed timesheet.

Some items, such as the confidentiality agreement, can be stored under either work flow. There is, of course a bit more about QMS reviews, training (increasing the skills of staff is as important to the idea of continuous improvement as better quality in products and service) and so on, but as far as direct work for a client goes, that’s it. You should have all this information to hand anyway or do you really work with no idea of what the customer wants?

All ISO 9000 asks is that you record what it was you were asked to do and show that you did it, or at least worked on it Customer feedback is the only proof you need. If they are happy then ISO doesn’t care how you made them happy, just that you record their state of happiness.

## The scheme extended

The standard PCG scheme assumes the company is a service provider and therefore leaves out some aspects of the standard. This isn’t a problem for developers working closely with users or to a set of requirements, that is those providing a development service, but it does preclude developing software as a distinct product.

Fear not! Procedures for these steps have been added into the PCG scheme by several companies including HMW. There are only really two missing elements: a specific design and development cycle; and a process to publish and maintain separate versions of the products.

For design the PCG requirements documents are easily expanded. ISO 9000 does not dictate what the technical content of any of these documents has to be. If you like patterns, then your procedure might be to state which pattern you are using and that it is verified by someone in your company. The auditor just has to see that one was specified and that someone bothered to verify it. You don’t fail for using the wrong pattern, that’s your choice as an expert. You can fail only if you boast to the world you are using patterns and then either you don’t use them or can’t prove to your customer and auditor that you did.

## An ISO 9000 Quality System for APL developers

HMW Computing has developed a Wiki based system using TWiki, which follows the PCG scheme structure, with optional extensions for product development, but is directly available via the world wide web, raher than Subversion. I’m not intending this section as a sales pitch, but I do have to explain some features of our QMS Wiki to show how APL fits with ISO 9000 – but if anybody wishes to talk to me later…

## What do we have to do?

Not as much as you might think, as long as we do it consistently. I’m not the sort of person to add unnecessary bureaucracy to anyone’s burden, especially my own. Our solution is based on the premise that if a development team has documented its work properly, then most of the effort to comply with ISO 9000 has already been done. I stress that this is a structure for a team to record everything about any type of project. So for a particular project, much of this will be unpopulated. Only a few items are mandatory to satisfy ISO 9000.

You don’t have to document payroll procedures for example. Nothing stops you from doing so, but this is where companies often get bogged down, putting things into the QMS which only have peripheral impact on the work they do for the end customer. In XP terms YAGNI – “You Ain’t Gonna Need It”. Just stick to what’s necessary to get the APL project done.

Another mistake made by many companies is that they attempt to get the QMS right first time. An auditor at a working party meeting of the PCG the other day stated that this was a very bad idea: get your QMS working and go for the audit. If nothing else it leaves room for some “continuous improvement”!

## The Wiki facilities

The Wiki has pages for our client entities, which can proceed through the different states of being a prospect, a client, and sadly sometimes an ex-client. Various topics may be associated with them at different stages. Also we keep all our suppliers in the system. The clients’ staff get their own pages, so we can mange contacts with them and their responsibilities within projects.

The fact that all the project-related events are held in the Wiki made a team-wide calendar an obvious extension to the QMS. We have pages for meeting minutes and site visits, with a debrief form for that vital feedback, so we can tell if they are satisfied, well, at least as far as the QMS is concerned. All events, together with scheduled development actions and purely private events can be displayed on the calendar. We have the usual options for repeating events, appointments and public holidays. In short, the Wiki has all the features of a Customer Relationship Management System, without going over the top.

At an early stage the entity is only potentially a Client. Recording at this level is simple: a Wiki Page to record our understanding of the requirements; proof that we sent the code of ethics and (we hope) some acknowledgement that they read it (most clients don’t bother); a form for risk assessment (mainly can we do the job and do we want to); and container pages to which are attached any Quotations, CVs or Confidentiality agreements exchanged.

Quotations can be as brief as an email saying something like: “It’ll cost you £5,000” to do the work you describe in the attached Word document”, which you send to the quotations page (there is a email to Wiki gateway)- you have to be able to prove you did quote and that the customer agreed for you to go ahead on that basis. If on the other hand, you use a method to estimate the effort, then say so in the Quality Manual and prove you use it every time you issue a quotation to your customers

The prospect becomes a client when we start some work for them, so it doesn’t add much more, except give us places to hold more documents, such as any Health and Safety policies or other general instructions, to initiate projects and a set of searches to aggregate information for the project level Wiki pages, which is really were the main activities take place.

Though I said earlier that we could leave out the ISO 9000 section on handling and storage of products if we were just in a consulting project, we do have to allow for the situation where we are lent, or placed in charge of, some property belonging to our client. This might be a piece of software installed on our equipment to access a database, or the loan of a laptop. In all cases we must demonstrate to the auditor we are taking care of our client’s assets. If we go as far as to provide facilities to a client (for example web hosting), or if we are given the responsibility to run some facility on behalf of the client we must once more list what it we are doing and do and fill in a risk assessment form. All of these are in pre-defined Wiki pages, so the burden of completing them is light. They are to record what it is we are up to for the auditor to have a clearer understanding and to ensure we keep an eye on what we agreed to do.

HMW uses Process and Data Definitions to gather user requirements into outline system specifications. They are based on a number of sources and are compatible with IEEE 830 (which I won’t go into here), we’ve successfully used them for years, but this particular layout is not mandatory for ISO 9000. We turn the definitions into Development Schedules and Modules, which act both as a part of the code repository. And as a data item for the Incident and Change Request tracking parts of the QMS.

There is a page to record the project environment, which describes the hardware and operation system the project is to use. The daily notes log is a bit like a project Blog. There are a number of other supporting pages, but I won’t go into them here. The fact that all this information is all kept in a Wiki enhances the project documentation over anything kept on paper. For example, by automatically cross indexing Process and Data Definitions we can see which processes create and access particular data items.

This supports the whole software development lifecycle. It’s all ISO 12207 compatible, which is yet another standard (but don’t get me started on this one), so we are killing two birds with one stone. Of course, other documents could still conform to ISO 12207 if they satisfy the QMS requirements.

ISO 9000 says a lot about checking output for defects, which I think is called testing in software developments. We use our development schedule pages to assign testing activities, but as long as you can prove you’ve tested the software to an auditor, then you can use any layout you wish.

HMW uses Maya and Inca our in-house code and change management tools. They are separate from the QMS, of course, but we use the Wiki to publish change files to clients and as the repository for completed products, which covers the preservation, handling and dispatch requirements of ISO 9000. Other teams could use workspaces or some other mechanism. TWiki handles all file types as attachments, with the added advantage of built in version control on each attachment. I stress that we use the Wiki as a repository of official releases, not as the development platform.

The Wiki has a sub-system to allow recording of hours worked against any project defined in the QMS. The result is searchable time sheets, which are used by our in-house accounts to generate invoices based on the time recorded.

We monitor the QMS performance with a series of searches and the work flows provide sensible points for reviews, with appropriate links to on-line forms which activate when needed. The data model is fully documented, but I’ve not included it for the sake of brevity, as this article is long enough as it is.

## Why would you want ISO 9000?

For some business sectors it is a pre-requisite: defence; engineering; and some NHS work. Many other organizations have ISO 9000 certification for at least their customer services and feel more comfortable with suppliers who can demonstrate the same standard of service. I would think that all customers care about the quality of work that is done for them and ISO 9000 is one way of advertising that you are the ones to provide it.

It shows you have a process in place and so increases customer confidence and satisfaction, improving the likelihood of closing deals or renewing contracts.

Using a standard document can increase your efficiency, obviously by making sure you don’t forget something, but also by stopping you don’t from putting down too much, wasting time and confusing the client. After all aren’t things like “patterns” development templates? Aren’t they just a reminder of what to include and exclude in any process or object?

## Conclusion

Our experience shows that, admittedly once we put in a lot of effort to build the QMS, it doesn’t have to be painful to use keep it going.

APL projects using the type of development approach we take for granted can achieve international standards and not be seen as some bastard child of the “mainstream” techniques.

ISO 9000 can help you to make your company more productive, efficient and therefore more profitable.

<a id="references"></a>

## Further reading

1. PCG ISO 9000 official site http://iso9001.pcg.org.uk/cms/index.php
2. List of ISO 9000 standards  
    http://www.iso.org/iso/iso\_catalogue/catalogue\_ics/catalogue\_ics\_browse.htm?ICS1=3&ICS2=120&ICS3=10
3. Twiki Open Source Enterprise Wiki http://twiki.org
4. HMW’s Code of Ethics  
    http://www.hmwcomputing.co.uk/CodeOfEthics.html
5. IEEE 830 Recommended Practice for Software Requirements  
    http://standards.ieee.org/reading/ieee/std\_public/description/se/830-1998\_desc.html
6. ISO/IEC 12207 Framework for Software Lifecycle Processes  
    http://www.iso.org/iso/catalogue\_detail?csnumber=43447
