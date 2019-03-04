---
layout: default
title: 3D IO
nav_order: 1
parent: IJ-Plugins Toolkit
has_children: true
permalink: /docs/ijp-toolkit/3D-IO
---

##  3D IO

Plugins for reading and writing in 3D formats:

* **MetaImage Reader** - reads from MetaImage format used by <a href="http://itk.org/">ITK
* **MetaImage Writer** - writes a 2D image or a 3D stack to MetaImage format used by <a href="http://itk.org/">ITK</a>.
        Option <strong>Save in single file</strong> indicates whether the image should be saved in a single file
        (extension *.mha) or the header and the image data should be saved in separate files (with extensions *.mhd
        and *.raw respectively.)
* **VTK Reader** - reads format used by <a href="http://vtk.org/">VTK</a>.
* **VTK Writer** - writes a 2D image or a 3D stack in format used by <a href="http://vtk.org/">VTK</a>.
* **Export as STL** - interprets intensity in 2D image as height and writes the height surface in
        <a href="http://en.wikipedia.org/wiki/STL_%28file_format%29">STL format</a>. Data can be saved either in
        <samp>binary</samp> or <samp>ascii</samp> (text) format. Binary format produces much smaller files.
        Option <strong>Save sides</strong> enables generation of the mesh for sides and the bottom.

Plugins install under `Plugins > 3D IO` menu.
