---
layout: post
title: Quick Reference of ADS
mathjax: false
related: false
comments: true
---

_Last modified: Feb, 2012_


Just some quick notes on using ADS. 

## Basic procedure of layouting a photomask

* Launch ADS

* Create new project. 'File' -->  'New Project' --> Name: $HOME/ADS/mynewproj; Project Technology Files: ADS Standard; Length unit--micron. (The project name will be mynewproj_prj

* The schematic window is already there by default. Back to project view window, click 'Window' --> 'New Layout' (or click on the button with yellow squiggling lines in a black background) to pull up layout window. Schematic view is not needed in this case (no schematic for a photomask), just close it. 

* Draw a 1um radius circle. Right click --> 'Properties' , and edit parameters such as the center coord and circle radius. Click 'File' --> 'Save Design', save it as "circle1um". Open up another layout view, and do 'Insert' --> 'Component' --> 'Component Library'. Click 'Projects', and find the design just created under the project. The component list will be on the right of the new insert window. Just drag the entry into the new layout view. The circle can be used as a component for another layout. 

## Draw repeated pattern

To draw repeated units/blocks, 'Edit' -> 'Advanced Copy/Paste' -> 'Step and Repeat'. This is very useful to draw a whole array of same components. 

## Layer control

There is a small floating window called 'Layers' besides the layout window. If there is none, click 'View' --> 'Layer View' to pull up the window. To view the physical meaning of layer, click 'Edit' in layer view, adn choose 'Adavance'.

## Flatten

When one creates a new layout based on an existing GDSII file, sometimes certain parts of the design are not selectable due to different hierarchy. That means, everything that is inserted as a **component** can not be selected by **layer**. To circumvent, you can "**flatten**" out the hierachy. To "flatten" a component: select the component --> right click --> 'Component' --> 'Flatten'. 

It may just flatten one hierachy at one time. Repeat the operation for several times for a full flattening.  

## Coordinates

There are two coordinate pairs in the right bottom of the window. 

The one in the left: absolute (x,y) coordinates; the one in the right: relative (x,y) coordinates with respect to the point of last mouse click. 


## Basic simulation

Here is a minimalist tutorial on simulating the transient response of a RLC circuit with ADS.

* New project -> schematic (created with new project by default).

* The first drop-down menu toolbar contains lots of components. To draw a basic RLC circuit, 'Lumped-components' --> pull parts in. Click + Ctrl r will rotate the components. 

* Finish a RLC circuit. 

* Insert wire (also in the toolbar)
 ** Insert probe components: 'Probe compoents' --> 'I_Probe'.  To probe voltage, one only needs to name the wire, and it will show in results (click 'Name' on the toolbar).
 ** Don't forget to insert ground (on the toolbar too). 

* Insert a voltage source: 'Sources-Time Domain' -> select step function voltage source.

* Add simulation engine: 'Simulation-Transient' --> select 'Trans' component and pull it in.

* Run (Click 'Simulate' button on the toolbar).

* There will be a new page for the results. On the 'Palette' plane, select 'Rectangular Plot' and add the variable concerned from left list to right list. Click 'okay'. 

* The buttons with "wavy" patterns are markers for the curve. Click on one of them and place it on the curve to read the values of points of interest. 
