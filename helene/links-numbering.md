Current links and numbering structure in PDF and webview

##numbering (proposed structure for anything that has a number)
- use 4 ```<span>``` within containers. 
- have the ability to define how many spans we need while writting rulesets.

```
<section data-type="title">

 <span></span>
 
 <span></span>
 
 <span></span>
 
 <span></span>
 
</section>
```
Numbered elements:

 - unit
 - chapter
 - section/page
 - sub sections (...)
 - figure
 - equation
 - cite
 - table
 - image
 - eob solution
 
##links
###intra-links
current pdf structure: <a class="xref target-element">
 
<b>examples</b>:
 ```<a class="link" href="">link to figure</a>```
 
 ``` <a class="xref target-inlinemediaobject">link to an image</a>```

  link target-element
  ```<a xmlns="http://www.w3.org/1999/xhtml" class="xref target-example" href="#m12207-eip-851" title="Example 1.3. ">Example 1.3</a>```
  
  webview: ```<a href="#Example_01_02_05" class="autogenerated-content">Example</a>```

  ```<a xmlns="http://www.w3.org/1999/xhtml" class="xref target-figure" href="#m12207-rachelpike" title="Figure 1.11. ">Figure 1.11</a>```
  
  webview: ```<a href="#Figure_01_01_007" class="autogenerated-content">Figure</a>```

  ```<a xmlns="http://www.w3.org/1999/xhtml" class="xref target-section" href="#m12204" title="1.4. Thermochemistry and Thermodynamics">Section 1.4</a>```

  ```<a xmlns="http://www.w3.org/1999/xhtml" class="xref target-table" href="#m10532-fs-id1167469604443" title="Table 1.4. ">Table 1.4</a>```
  
  webview: ```<a href="#Table_01_04_01" class="autogenerated-content">Table</a>```

  ```<a xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" class="book" href="#book-attribution" title="Attributions">Attributions</a>```


###### note: examples aren't numbered in webview, so the link to examples isn't numbered either, producing something like "The domain of a function can also be determined by identifying the input values of a function written as an equation. See Example, Example, and Example." vs "The domain of a function can also be determined by identifying the input values of a function written as an equation. See Example 1.17, Example 1.18, and Example 1.19."  in PDF

Desired functionality: cook the numbers in. 

<b>proposed</b> new structure could be:

```<a xmlns="http://www.w3.org/1999/xhtml" class="xref intra-link" href="#m12207-eip-851" title="Example 1.3. ">Example 1.3</a>```


###### note: currently, the XSLT handles naming the link itself for numbering. Do we still need that level of detail for numbering? Can we do it another way? Do we need the 4 spans in those inline links?

For reference, current mixins (CSS generated) targets for numbering structure
 ```
  a.target-figure         
  a.target-subfigure       
  a.target-table            
  a.target-example         
  a.target-exercise     
  a.target-equation,
  a.target-inlineequation,
  a.target-informalequation 
  a.target-section 
  ```


###external link
  ```<a xmlns="http://www.w3.org/1999/xhtml" class="link" href="http://openstaxcollege.org">non-shortlinked external URL</a>```
  
  ```<a xmlns="http://www.w3.org/1999/xhtml" class="link" href="http://www.openstaxcollege.org/l/30JWSTdiag">fun diagram</a>```
   > result in PDF: fun diagram (http://www.openstaxcollege.org/l/30JWSTdiag)
   
    webview: ```<a rel="nofollow" href="http://openstaxcollege.org/l/relationfunction">Determine if a Relation is a Function</a>```
   
  PDF requests separation between text and url for styling purposes
   
   <b>proposed</b>: ```<a xmlns="http://www.w3.org/1999/xhtml" class="link" href="http://www.openstaxcollege.org/l/30JWSTdiag">fun diagram <span>http://www.openstaxcollege.org/l/30JWSTdiag</span></a>```
   This allows for url to be a different color, can be hidden if unecessary.


##PFD TOC linking
###structure: 
```
target-part
  > target-chapter
    > target-section
      > target-section
 ```
all the links in the TOC need the 4 span structure described above

####current links in toc in pdf
```<a xmlns="http://www.w3.org/1999/xhtml" href="#idm333840190976" class="target-part"><span class="cnx-gentext-part cnx-gentext-n">I</span><span class="cnx-gentext-part cnx-gentext-autogenerated">. </span><span class="cnx-gentext-part cnx-gentext-t">The Chemistry of Life</span></a>```

```<a xmlns="http://www.w3.org/1999/xhtml" href="#idm196486460864" class="target-chapter"><span class="cnx-gentext-chapter cnx-gentext-autogenerated">Chapter </span><span class="cnx-gentext-chapter cnx-gentext-n">1</span><span class="cnx-gentext-chapter cnx-gentext-autogenerated">. </span><span class="cnx-gentext-chapter cnx-gentext-t">Astronomical Instruments</span></a>```

```<a xmlns="http://www.w3.org/1999/xhtml" href="#m10528" class="target-section"><span class="cnx-gentext-section cnx-gentext-n">1.1</span><span class="cnx-gentext-section cnx-gentext-autogenerated">. </span><span class="cnx-gentext-section cnx-gentext-t">Telescopes</span></a>```

```<a xmlns="http://www.w3.org/1999/xhtml" class="link external-content" href="http://legacy-textbook-dev.cnx.org/content/m49299/latest/#Figure_01_00_001">m49299</a>```

####chapter outline

all the links in the chapter outline need the 4 span structure described above

 ```<a xmlns="http://www.w3.org/1999/xhtml" href="#m10558" class="target-section"><span class="cnx-gentext-section cnx-gentext-n">1.1</span><span
  class="cnx-gentext-section cnx-gentext-autogenerated">. </span><span class="cnx-gentext-section cnx-gentext-t">The Science of Biology</span></a>```

###index
```<a xmlns="http://www.w3.org/1999/xhtml" class="indexterm" href="#m10861-autoid-cnx2dbk-idm333879749696">Parts of a Neuron</a>```

###solutions (EOB)
```<a xmlns="http://www.w3.org/1999/xhtml" class="solution" id="m10966-fs-idm94637584" href="#m10966-fs-idp98793296">Return to Exercise</a>```

  (webview:
   ```<div class="ui-toggle-wrapper">
    <button class="btn-link ui-toggle" title="Show/Hide Solution"></button>
   </div>```)