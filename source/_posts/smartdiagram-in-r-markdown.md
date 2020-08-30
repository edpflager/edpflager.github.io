---
title: SmartDiagram in R Markdown
tags:
  - cookbook
  - howto
  - R Markdown
  - technical
id: '4236'
categories:
  - - Business Intelligence
  - - R
  - - R Markdown Cookbook
comments: false
date: 2018-12-14 19:33:32
---

[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)A useful plugin from the LaTeX environment that works with RMarkdown is [**smartdiagram**](http://texdoc.net/texmf-dist/doc/latex/smartdiagram/smartdiagram.pdf) developed by Claudio Fiandrino. Conceived as a way to create diagrams from a list of items, it will generate output as circular diagrams, flow diagrams, sequence diagrams and several others. Components within the diagrams can be different colors or a single uniform one. Combined these elements provide a visual layer to describe activities within your R Markdown diagrams and make what your are trying to convey more intuitive to the reader. I won't cover using all of the different diagrams in smartdiagrams with R Markdown, but this should be a good introduction to how to get it to work. To get started, in your Markdown document YAML header-includes line add a call to use the smartdiagram package:

header-includes: usepackage{smartdiagram}
<!-- more -->
Smartdiagram is built on the TikZ package and a call to that package is included so its not necessary to explicitly load it. You may however, need to include a tikz.sty file in your R Markdown directory if you don't already have one. The contents are very brief and are reproduced here:

% Copyright 2006 by Till Tantau
%
% This file may be distributed and/or modified
%
% 1. under the LaTeX Project Public License and/or
% 2. under the GNU Public License.
%
% See the file doc/generic/pgf/licenses/LICENSE for more details.

RequirePackage{pgf,pgffor} % calc and xkeyval have been removed!

input{tikz.code.tex}

endinput

#### Sequence Diagrams

With the package call specified, we can define a simple sequence diagram. Our first step is the diagram set to specify characteristics of the sequence. To set a uniform color for the items in the diagram and specify the text color, we can use this:

smartdiagramset{uniform sequence color = true, sequence item text color = white}

Following that define the diagram type (sequence) and the items in the sequence and knit the file to generate the diagram.

smartdiagram\[sequence diagram\]{Birth, Taxes, Death}

[![](http://edpflager.com/wp-content/uploads/2018/12/PDF-sequenceuniform.png)](http://edpflager.com/wp-content/uploads/2018/12/PDF-sequenceuniform.png) Adding color to the diagram is defined in the smartdiagramset command by replacing uniform sequence color with set color list. You then define a set of colors to match the elements in your list. One thing to be aware - if there are more items in the sequence than defined colors, any items that do not have a specific color defined will be displayed with the default color as shown in the graphic. The code below generates the diagram shown (with a nod to South Park):

smartdiagramset{set color list={blue!50, cyan, green},sequence item text color = black}
smartdiagram\[sequence diagram\]{Steal Underwear, ???, Profit, Success, Taxes}

[![](http://edpflager.com/wp-content/uploads/2018/12/PDF-sequencecolor.png)](http://edpflager.com/wp-content/uploads/2018/12/PDF-sequencecolor.png)

#### Circular Diagrams

Circular diagrams are useful for representing items that repeat in a cycle such as the seasons of the year (Spring, Summer, Fall, Winter). For this example though, I will use the software development lifecycle (SDLC). This is a repetitive five step process, where needs are identified, software is designed to meet those needs, code is implemented to process the design, the implemented software is tested, and the test results are evaluated to see if the software meets the original needs. If not all the needs have been met, or new needs are identified, the cycle repeats. With the same prerequisites in place as defined above for your R Markdown document, you can implement this circular diagram with one line of code and smartdiagram will use a predefined set of colors for your output:

smartdiagram\[circular diagram:clockwise\]{Needs, Design, Implement, Test, Evaluate}

Notice that smartdiagram has a limited number of characters horizontally for items, so it hyphenates output text into multiple lines. With an odd number of elements for your diagram, smartdiagram does not put the first element at the top of the chart, but rather offsets it somewhat.  You can also define how the diagram is laid out - clockwise or counterclockwise. If you don't specify a rotation, smartdiagram will lay it out counterclockwise. [![](http://edpflager.com/wp-content/uploads/2018/12/PDF-circularprocess.png)](http://edpflager.com/wp-content/uploads/2018/12/PDF-circularprocess.png) You can also override the predefined colors by using a smartdiagramset command with your preferred color scheme. Unfortunately, for a uniform color you have to specify each color, as the uniform sequence color command does not work with circular diagrams. Here is an example:

smartdiagramset{set color list={gray, gray, gray, gray},sequence item text color = black}

[![](http://edpflager.com/wp-content/uploads/2018/12/PDF-circularuniform.png)](http://edpflager.com/wp-content/uploads/2018/12/PDF-circularuniform.png)

#### Flow diagrams

Similar to a circular diagram is a flow diagram which can be represented vertically (default) or horizontally. If no direction is specified in the command to generate the diagram it will default to vertical. For consistency, I prefer to include the direction for both. As with other smartdiagram examples, the colors will default to a predefined pallet unless specifically overwritten. Here are two examples:

smartdiagram\[flow diagram:horizontal\]{Beginning, Middle, End}

[![](http://edpflager.com/wp-content/uploads/2018/12/PDF-flowdiagram.png)](http://edpflager.com/wp-content/uploads/2018/12/PDF-flowdiagram.png)

smartdiagramset{set color list={green!50, blue, orange},sequence item text color = black}
smartdiagram\[flow diagram:vertical\]{Beginning, Middle, End}

[![](http://edpflager.com/wp-content/uploads/2018/12/PDF-flowvertical.png)](http://edpflager.com/wp-content/uploads/2018/12/PDF-flowvertical.png) So why use a flow diagram in place of a circular diagram? In the smartdiagram implementation in native LaTeX, the flow diagram has the ability to include other items that feed into the flow at specific parts of the flow. This involves the loading of the smartdiagram library{additions}, which I have not been able to get to work in R Markdown, as of yet.

#### Other diagrams

Smartdiagram includes a number of other diagram options that I have not found a use for, but below are a few examples. Be sure to check out the [smartdiagram](http://texdoc.net/texmf-dist/doc/latex/smartdiagram/smartdiagram.pdf) documentation for more. Bubble Diagrams   [![](http://edpflager.com/wp-content/uploads/2018/12/PDF-bubblediagram.png)](http://edpflager.com/wp-content/uploads/2018/12/PDF-bubblediagram.png) Priority Descriptive diagram [![](http://edpflager.com/wp-content/uploads/2018/12/PDF-prioritydiagram.png)](http://edpflager.com/wp-content/uploads/2018/12/PDF-prioritydiagram.png) Below is the R Markdown code I used to create this post:

\---
title: "SmartDiagrams"
output:
   pdf\_document
header-includes: usepackage{smartdiagram}
---


\`\`\`{r setup, include=FALSE}
# usepackage{TikZ}
knitr::opts\_chunk$set(echo = TRUE)
\`\`\`


Sequence Diagrams

smartdiagramset{set color list={blue!50, cyan, green},sequence item text color = black}
smartdiagram\[sequence diagram\]{Steal Underwear, ???, Profit, Success, Taxes}

smartdiagramset{uniform sequence color = true, sequence item text color = white}
smartdiagram\[sequence diagram\]{Birth, Taxes, Death}

\*\*\*
Circular diagrams

smartdiagramset{set color list={green!50, blue, orange, gray},sequence item text color = black}
smartdiagram\[circular diagram:clockwise\]{Needs, Design, Implement, Test, Evaluate}

smartdiagramset{set color list={gray, gray, gray, gray},sequence item text color = black}
smartdiagram\[circular diagram:counterclockwise\]{Spring, Summer, Fall, Winter}

\*\*\* 
Sequence diagrams

smartdiagramset{border color=none, uniform color list=teal!60 for 99 items}
smartdiagram\[flow diagram:horizontal\]{Spring, Summer, Fall, Winter}


smartdiagram\[flow diagram:horizontal\]{Beginning, Middle, End}


smartdiagramset{set color list={green!50, blue, orange, gray},sequence item text color = black}
smartdiagram\[flow diagram:vertical\]{Beginning, Middle, End}


\*\*\*
Bubble Diagram
smartdiagramset{set color list={blue!50, cyan, green, blue!50, cyan, green,blue!50, cyan, green},sequence item text color = black}
smartdiagram\[bubble diagram\]{A,B,C,D,E,F,G,H,I,J }


\*\*\*
Priority Diagram
smartdiagramset{
set color list={blue!50!cyan,green!60!lime,orange!50!red},
priority arrow width=2cm,
priority arrow height advance=2.25cm
}
smartdiagram\[priority descriptive diagram\]{Food, Shelter, Security }