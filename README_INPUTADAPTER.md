# OpenLoopAdapters

To be able to execute a generic Openloop processing files from different input sources
need to be adapted to a generic format.

The generic format consists of the following structure:

<blockquote>

HEADER<br>
&nbsp;BRG (Begin Resource Group)<br>
&nbsp;&nbsp;BR (Begin Resource, i.e. Font, Codepage, FORMDEF, Overlay, Pagesegment)<br>
&nbsp;&nbsp;ER (End Resource)
&nbsp;&nbsp;&nbsp;&nbsp;... more resources
&nbsp;ERG (End Resource Group)
<br><br>
DOCUMENT<br>
BDT (Begin Document)<br>
&nbsp; BNG (Begin Named Group = document or mailpack)<br>
&nbsp;&nbsp; TLE (Tag Logical Element, contains Metadata)<br>
&nbsp;&nbsp; BPG (Begin Page)<br>
&nbsp;&nbsp;&nbsp; BPT (Begin Presentation Text)<br>
&nbsp;&nbsp;&nbsp;&nbsp;PTX (Presentation Text Data)<br>
&nbsp;&nbsp;&nbsp; EPT (End Presentation Text)<br>
&nbsp;&nbsp;&nbsp; (IPO Insert Page Overlay)<br>
&nbsp;&nbsp;EPG (End Page)<br>
.... more pages for this document<br>
&nbsp;ENG (End Named Group)<br>
... more Resource Groups<br>
EDT (End Document)

## Basic Operation
In order to process correctly the documents different states are maintained:
- Processorstate - providing the action to be done on the SF (like Drop);
- Documentate - maintaining metadata of the document (like Name, Adress);<br>
  -...

On a number of Structured fields based on the state informatie actions are done for the output file. (like writing TLE with certain content)

The project contains the following adapters:

## IXE
This adapters perfoms following actions:
<ul>
   <li>Drops the NOP fields</li>
   <li>Adds Named group (by scanning the OMR marks on the pages</li>
   <li>Shift text on the pages</li>
   <li>Removes first 10 pages
</ul> 

## Betalen
This adapters perfoms following actions:
<ul>
<li>reads existing TLE (and removes this from the output)</li>
<li>creates new standardised TLEs</li>
<li>de BNG moet geïnterpreteerd worden</li>
<li> adds Named Groups </li>
<li> adds leaflet code</li>
<li> adds for certain streams message pages</li>
</ul> 


## Sparen
This adapter has significant overlap with the betalen adapter and differs on the folling points:
<ul>
<li>TLE processing (based on savingtype)</li>
<li> Peform counting for outputing statistics in the output log.</li>
</ul> 

## Statements (RST)
This adapter processes 8 inch (square) RAE statements documents. Those documents come from four streams:

* RAE572 (8inch) stream: 907 (24h) Envelope: 224818 (10069398) (Buiten)
*  RAE552 (8inch) stream: 815 (24h) Envelope: 225940 (10069576) (Buiten)
*  RAE532 (8inch) stream: 814 (24h) Envelope: 225940 (10069577) (NL)
*  RAE585 (8inch) stream: 902 (24h) Envelope: 224818 (10069573) (NL)

The processor for this adapter executes following actions:

* Removes NOP and IMM fields.
* Reads OMR code from PTX with STRJ font. OMR code is used for locating place to put BNG (5th character)
* Injects BNG ENG pairs where OMR code contains &amp; character as 5th character.
* Moves the PTX AMI's 74 points on odd pages and 17 points on even pages.
* Moves overlays displacement to 1 on all pages.
* Changes x page size to A4 size.
* Cleans reprint codes, replacing them with single space character.
* Cleans KIX code, replacing it with single space character.
* Adds address TLE's.

 
