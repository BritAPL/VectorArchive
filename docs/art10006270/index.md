---
vid: '10006270'
title: The Genesis of ASL (2)
authors:
- Tony O’Hagan
volume: '7'
issue: '4'
page: '36'
status: not online
issue_tab: n4-april-1991
---

Tony O’Hagan
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 36](../7/4/VOL.7-NO.4-APRIL-1991.pdf#page=38)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR VolL7 No.4</p>
<p>Deputy Chairman: Alan Sykes, Department of ManagementScience and Statistics, University College of Swansea, Singleton Park, Swansea SA2 8PP. Telephone: 0792-295296</p>
<p>Manager:Jake Ansell, Business Studies, University of Edinburgh, William Robertson Building, 50 George Square, Edinburgh EH89JY. Telephone:031-667-1011</p>
<p>BAA Representative &amp; Newsletter Editor: David Eastwood, MicroAPL Ltd, South Bank Technopark, 90 London Road, London SE1 6LN. Telephone:071-922-8866</p>
<p>BAA Representative: John Searle, 13A MountArarat Road, Richmond, Surrey TW10 6PQ. Telephone:081-948-6737 (home)</p>
<p>The Genesis of ASL (2): October 1988 by Tony O Hagan</p>
<p>First Approaches to the BAA Thevery earliest beginnings of the ASL idea were chronicled in thefirst article in * this series. The next key event was a meeting on 5th October 1988 at IBM South Bank, London. Present were Jake Ansell, David Eastwood, Norman Thomson and myself. This meeting drew up the following list of goals as part of establishing an APLservice to statistics users .</p>
<p>1. Define standard functions for basic statistical operations. Identify good implementations, possibly by soliciting code from the statistics/APL community.</p>
<p>2. Identify standardsfor delivering functions/workspacesto different APL systems.</p>
<p>3. Draft standards for more complex functions, their syntax and means of verification.</p>
<p>4. Create a library for statistical functions, comprising the standard basic functions and others submitted by users.</p>
<p>36</p>
<p>VECTOR Vol.7 No.4</p>
<p>5. Prepare housekeeping procedures/ functions to administerthis library. These should coverreceipt, verification and storage of new functions/workspaces, plusselection, packaging and delivery of requested functions/ workspaces.</p>
<p>6. Document functions in book form, possibly loose-leaf.</p>
<p>Several ways of achieving these goals were discussed, but the favourite wasfor a student to do it as a one-year MSc project. I agreed to look into the possibility. Finally, the meeting considered waysof funding the project. The idea of an MSc student wasattractive because it would be cheap, butstill substantial money would be involved. It was here that involvementof the British APL Association wasfirst suggested. Another possibility was funding by IBM.</p>
<p>Myinvestigations concerning the MSc idea were not very successful, for similar reasons as caused us to drop the thought of a PhD project in our earlier discussions. But David Eastwood made contact with the BAA on our behalf at a BAA committee meeting in November. They expressed aninterest in principle in providing support for our project. In particular, they suggested that the ball could be set rolling by having a conference on APL andStatistics , to establish demandand collect ideas. This very positive responseto our tentative enquiries was extremely encouraging. For thefirst time,it lookedlike the statistics library might be in business!</p>
<p>T have no record of what happenedin the next couple of months.I believe that we were asked to clarify our thinking, but no written proposal seems to have been madethen, nor any meeting held. The BAA committee met again in January 1989, Alan Sykes, who was the BAA Education Officer at the time, wrote me a letter dated 18th January reporting the committee s conclusions. A substantial part of Alan s Education budget for the year wasto be given to the proposed conference. He had already made a booking for it at the University of Wales Conference Centre at Gregynog for that September. Furthermore, a firm proposal for fundingof the projectitself would be welcomed.</p>
<p>Another meeting took place on 24th February, again at IBM South Bank. This was publicised by Jake Ansell through the APL Statistics Users Group, and attracted severalinterested people from industry. In all, the attendance was Dick Bowman, Mike Day, David Eastwood, Peter Lane, A MacGillivray, Robin Morphet, Alan Sykes, Norman Thomson and myself. A great deal was discussed. Dick Bowmansuggested that Sig APL mightalso contribute to the ASL project. Dicklater followed this up withoutsuccess. It seems that the American groupare very wary of supporting aninitiative which might later compete (unfairly is implied) with commercial products.</p>
<p>37</p>
<p>VECTOR Vol.7 No.4</p>
<p>The main achievementof this meeting wasto establish two interim committees. A Conference Committee, chaired by Alan Sykes, would meet to plan the conference in September. A Proposal Committee, with me in the chair, was charged with drawing up detailed proposals for funding of the project proper, for submission to Sig APL and the BAA.</p>
<p>Anotherinteresting suggestion madeat this meeting was that in due course the Royal Statistical Society might be prevailed upon to put somekind ofseal of approval on the functions produced. Although this has not been pursued any further, we still hope to do so when sufficiently battle-hardened software is teady. The meeting ended with a discussion of a nameforthe library, and for the project. S-APL wastentatively agreed.</p>
<p>My records are hazy again here.It is clear that ] prepared a first draft proposal and sent it to members of the Proposal Committee only four dayslater, on 28th February. It seems likely that when the meeting on the 24th ended it metamorphosed into a Proposal Committee meeting, which went on to discuss detailed proposals at somelength. The text of that original draft is no longer with me,either. It went through a series of modifications, including further feedback from the BAA in April, and emerged on 5th June 1989 as a substantial five-page document. The name had changed to ASL = APLStatistics Library (because S- APL was already used by 1 P Sharp), and that name has stuck. Since this documentdefines ASLin basically the form which BAA agreedformally to fund, itis worth setting downheretheessential features.</p>
<p>1. The objectives were set out as to establish:</p>
<p>(a) a set of standards whichwill give coherenceto the assembly of functions, make them easy to use singly and in combination,facilitate porting to a range ofinterpreters and hardware, andhelp with verifying their correctness;</p>
<p>(b) a bottom shelf of functions comprising the most fundamentalstatistical routines (descriptivestatistics, calculations on standard distributions, etc.), plusutility functions (data handling and I/O, graphics, numerical procedures,etc.);</p>
<p>(c) various other shelves of functions for specific statistical applications, such as regression (linear models), categorical data analysis, multivariate statistics, reliability, analysis of variance, sample survey analysis, time series and forecasting, all taking functions from the bottom shelf, and possibly other shelves(e.g. a shelf for generalised linear models may take functions from the regression shelf).</p>
<p>38</p>
<p>VECTOR Vol.7 No.4</p>
<p>2. The benefits were described as follows. One ofthe best-selling categories of softwareis statistics packages... We want ASL to be a flagship for APLin this important market. But it was stressed that ASL wouldnotbe just another statistics package. ... it will not be a closed package, allowingthe user to do only whatthe programmers have thought to include. The full power of APL will always be available, ASL merely providinga sophisticated set of functions, which the user will then combine with each other, the APL primitives and his/her own functions.Forthe lay user, the documentation will provide idioms for operations suchas selection of variables and cases, data transformations and passing data between functions. To such a user ASL will appear as a complete package, as general and user-friendly as conventional packages. To the APL userit will be much more.</p>
<p>3. It was also boldly claimed that APLis the best languagefor doingstatistics. Data analysisis essentially exploratory.It is not a simple matter of applying a single standard analysis to the data, but of trying different kinds of analysis, looking at the data in different ways, and drawing together a range of conclusions. The APL environment is idealfor this activity. The APL language also uniquely supports the processing ofarrays of data neatly and simply.</p>
<p>4, Managementof ASL was to be by a Management Committee, to be elected at the September conference.In addition to a Chairman, Manager(responsible for day-to-day running of the ASL effort), Treasurer and Secretary, it was hopedto have BAA involvement throughits Projects, Technical and EducationOfficers. 5. A programmeof work wasdescribed, culminating with a general release of the first shelves in September 1990, These were to comprise a bottom shelf as in 1(b) above and a regression shelf. Although bothfirst and second generation APLs wereto be supported, APL2 implementation was not the first priority.</p>
<p>6. Funding was requested for a programmer, working for about 2 days per week for a year, for a PC for the programmer, and a smail amount for expenses and consumables, The BAA wasasked to commit moneyon a phasedbasis, paymentat the second phase being subjectto satisfactory progressin thefirst phase(to March 1990).</p>
<p>Since then, there have been minor changes in the formal objectives, the managementand the timescale, which will be described in later episodes of this series ... (watch this space!), but the ideals of (2) and (3) have kept ASL going ever since.</p>
<p>39</p>
<p>VECTOR VolL7 No.4</p>
<p>The ASL Regression Shelf by Alan Sykes</p>
<p>Introduction The ASL project outlined initial shelves to be produced in the first year. One of these was a Regression Shelf. An exploratory workspace was produced and discussed by the ASL committee. That workspace built on the work of Jake Ansell and Alan Sykes over recent years in using APL routines that behaved similarly to those implemented in the Royal Statistical Society's Software Package GLIM . It was decided that the Regression Shelf should contain a set of functions that performed GLIM-typecalculations in a way that allowed them to be incorporated into user functions.</p>
<p>Introduction to Regression Mostusers of Statistics, or for that matter users of APL, understand the basic principle of Simple Linear Regression where a response variable s average value dependson a predictor variable in a linear way. The equation ofthatline is estimated from bi-variate data by the method of least-squares, which in APL means the use of the dyadic version of domino .</p>
<p>In this introduction, some basic terminology is explained and the simple linear regression modelis extendedinto the conceptof a statistical linear model.</p>
<p>There are three majorattributes of statistical linear model. First we have the RESPONSEvariable, often referred to as the y-variable . This is usually random, with distribution assumed to be NORMAL or GAUSSIAN. Secondly, we have one or more PREDICTOR variables, usually regarded as non-random, which predict the average value of the response variable in a linear way.If E(Y) denotes the expected value or mean value of the response variable Y, and U,V,..W are predictor variables then we assumethat E(X) = a+bUscv+....aw ,</p>
<p>where a,b,...d are unknown constants.</p>
<p>So muchfor the mean of Y; what aboutits variance? The simplest assumption to make is that the variance is constant, given by k say. Hence y has a Normal Distribution with mean at+bU+cU+...tdW and variance k. Regression Analysis</p>
<p>40</p>
</details>
