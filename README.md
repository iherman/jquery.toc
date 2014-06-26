# Table of Content (TOC) script

Dynamic Table of Content (TOC) generation based on jQuery. 

## Usage

Include

	<!-- Reference to jQuery; this is just an example, adapt it to the local location of the package --> 
	<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.11.0/jquery.min.js"></script>
	<script src="your_path_to/jquery.headers.toc.js"></script>

or

	<!-- Reference to jQuery; this is just an example, adapt it to the local location of the package --> 
	<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.11.0/jquery.min.js"></script>
	<script src="your_path_to_toc/jquery.sections.toc.js"></script>

Depending on whether you want to use the versions based on the [traditional HTML header elements](#header) or the [HTML5 ``<section>`` elements](#section). The following style files:

	<link rel="stylesheet" type="text/css" href="your_path_to_toc/css/jquery.toc.css"/>
	<link rel="stylesheet" type="text/css" href="your_path_to_toc/css/mystyle.css"/>

are included in the repository to show how css styling can be used to adapt the presentation of the TOC. Salt 'n pepper on taste.

In the HTML source itself, the TOC placeholder:

	<div id="toc"></div>

will contain, after processing, the TOC. If the [version based on sectioning](#section) is used, one can also use

	<section id="toc"></section>


## General features

The following element is added to the TOC placeholder:

	<h2 id="toctitle">Table of content:</h2>

which is followed by the TOC itself through a `<ul>` element (see below). The headers themselves are based either on the `<h2>...</h6>` elements (`<h1>` is reserved for the document title) when using `jquery.headers.toc.js`, or the `<section>` elements when using `jquery.sections.toc.js` (see [below](#section) for more details). 

(It would be more elegant to use the locale to add the TOC title in local language. T.B.D.)

Each header in the text is prepended with the section number, using a `<span>` with a number-dependent `@id`. E.g., if the text of the 12th `<h2>` is "Title", the following structure will be generated into the final DOM:

	<h2><span id="id_12">12. </span>Title</h2>

The TOC entries themselves are put into an `<ul>`; each entry is an `<li>`. The `<li>` elements may include additional `<ul>` elements for a TOC hierarchy. 

The `<ul>`-s are set to the classes `toc` as well as `toclevel1`,`toclevel3`, ...,`toclevel5`, depending on the TOC level. The section number is also added to the entry enclosed by a ``<spam>``. 

E.g., a TOC entry for the item above looks like:

	<ul class="toc toclevel1">
		<li><span class="tocnumber">12. </span><a href="#id_12">Title</a></li>
	</ul>
	
#### [Interaction with the TOC](id:interaction)

The whole TOC can be hidden by clicking on the title. By default, the title is set to the class `tocvisible`; when made invisible, the class changes to `tocinvisible`. This can be used for styling (e.g., adding a different arrow to the beginning of the text). The default visibility can be changed through a [global control attribute](#globalcontrol).

Similar interactions are possible with any TOC item that also includes a sub-hierarchy. The number preceding the TOC entry can be clicked hide corresponding sub-hierarchy or make it visible again. By default, the sub-hierarchy is visible; this default visibility can be changed through a control attribute (see below for the [header](#headercontrol) and [section](#sectioncontrol) cases, respectively).The number itself is enclosed in a `span` element of classes `tocnumber` plus the `tocvisible` or `tocinvisible`, depending on whether the sub-hierarchy is visible or invisible, respectively. These classes can also be used for styling (e.g., putting a different arrow to the beginning of the text). 

## Header/section control

As mentioned above, there are two version of the script: one using the `<h2>...</h6>` elements the other uses the `<section>` elements.

### [`jquery.headers.toc.js`](id:header)
This version uses the traditional `<h2>...</h6>` elements in document order and in hierarchy depth to establish the sections and the TOC entries. `h1` is not used: this is reserved for the full document title.

#### [Special attributes](id:headercontrol)

* `@data-notoc`: a particular entry can be removed from TOC processing by adding the ``data-notoc`` attribute to the `<h2>...<h6>` element.
* ``@data-tochidden``: by default, if the generated section has sub-sections, the corresponding sub-hierarchy in the generated TOC is visible and can be hidden by [user interaction](#interaction). If the ``data-tochidden`` attribute is used, the default for the sub-hierarchy is to be hidden (and can be made visible by user interaction).

### [`jquery.sections.toc.js`](id:section)

Section is based on the `<section><hx>...</hx>...</section>` type structures. The "depth" of the hierarchy of `<section>` elements is used for sectioning and TOC control; the header itself can be any of `<h1>...<h6>`, they are all treated equally. To make styling easier, the class of each of these header elements are extended to a class reflecting their position in the hierarchy, i.e., `headertoclevel1` to `headertoclevel6`.

#### [Special attributes](id:sectioncontrol)

* `@data-notoc`: a particular section, *and its full sub-hierarchy*, can be removed from TOC processing by adding the `data-notoc` attribute to the `<section>` element.
* `@data-tochidden`: by default, if the section has sub-sections, the corresponding sub-hierarchy in the generated TOC is visible and can be hidden by [user interaction](#interaction). If the ``data-tochidden`` attribute is used on the ``<section>``, the default for the sub-hierarchy is to be hidden (and can be made visible by user interaction).

## [Global Control](id:globalcontrol)

Special attributes on the ``head`` element can be used for a global control:

* `@data-tochidden`: sets the initial visibility of the TOC; by default it is visible (and can be toggled by clicking on it). By adding this attribute the TOC is initially invisible (only the title is displayed).
* `@data-tocmaxlevel=N`: sets the "depth" of the TOC. 
	* in the ["headers" version](#header): the value of 1,2,3,4, or 5 sets the maximum depth used. Setting it to the value of "1" means that only the `h2` element is used; a value of "5" means that all headers will be included until `h6` (the default case). If the value is "0", the TOC generation is switched off altogether. 
	* in the ["sections" version](#section): the value of 1,2,3,4, or 5 sets the maximum depth used. Setting it to the value of "1" means that only the top level `<section>` element is considered; value of "5" means that all `<section>`-s until depth of 5 are used (default). If the value is "0", the TOC generation is switched off altogether. 
