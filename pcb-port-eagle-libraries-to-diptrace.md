---
layout: post
title: How to Port Eagle Libraries to DipTrace
mathjax: false
related: false
comments: true
---

__Created: Mar, 2015__

Note: better doing this in a Windows system with both Eagle and DipTrace. See the next section for the reason. 

To my knowledge at the time of writing, there is no direct way of porting Eagle libraries (component schematics and layouts) to DipTrace. The tutorial on [this page](http://cryoarchive.net/tutorials/diptrace-tutorials/converting-eagle-cad-files-to-diptrace/) works for porting Eagle schematics and layouts which contain various components to DipTrace, but not for the library directly. However, the latter just needs an extra step. 

First open the schematic or layout (they should come in pairs) in Eagle, run ULP jobs and the ULP scripts are __provided by DipTrace__ (not Eagle), which are usually in "$DIPTRACE_ROOT/Utils/Eagle_to_DipTrace_SCH.ulp" or "$DIPTRACE_ROOT/Utils/Eagle_to_DipTrace_PCB.ulp". Save the output ASCII files. 

Next, import the schematic to DipTrace by clicking "File"-->"Import"-->"DipTrace ASCII". Right click on the component of interest, and click "Save to library" in the right-click menu. Save the component schematic under one of the existing group (or create a new group). Do the same for layout. In the end, check the pin mapping in the component editor (click "pattern" in the "component properties" window in the component editor). Fix the mapping if necessary. 

## More

According to [this post](http://www.eevblog.com/forum/diptrace/how-to-export-eagle-lib-to-diptrace/), if doing it in Linux, pay attention to the problem: 

* Eagle for GNU/Linux produces script files with "UNIX" end of line.
* Eagle for Win32 produces script files with CRLF line terminators.
* DipTrace reads only files with CRLF line terminators.
* So if one does this in Linux, he should convert the UNIX end of line to CRLF for DipTrace to the libraries correctly. 
