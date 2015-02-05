latexmk-config
==============

This is a configuration for latexmk, a tool like make for latex documents.
It contains custom dependencies for automatic conversion of many types of graphics, so you only have to add the graphic sources to your document. The conversion to eps (for latex) or pdf (for pdflatex) it done automatically.
Furthermore a handy wrapper makefile is provided to start latexmk.
(pdf)latex is called with the synctex option, so backward (or inverse) search will work.

An example how to use this latexmkrc file can be found in the testing branch.

##Assumptions##
- In general EPS output was prefered when this script was built, as it can be used with both latex and pdflatex (using the automatic eps->pdf conversion of newer versions). However, the PDF output of most programs seems to be in a better shape. Bugs you may encouter are given below.
- All graphics are assumed to live in the subdir 'graphics'. This is required to clean all generated files. The path can be changed in the latexmkrc file.
- In the includegraphics statement a file extension must be used. ".eps" is prefered here, but for pdflatex ".pdf" can be used, too. 


##Custom dependencies
These are the files that can be converted automatically at the moment. The required tools are given.

### Adobe Illustrator (*.ai)
Requirements: pdftops and ps2eps

### Asymtote (*.asy)
Requirements: asymtote

After the eps->pdf conversion for pdflatex the bounding box may be too large. This can be avoided by using the direct asy->pdf conversion.

### Bitmaps (*.jpg, *.png, *.gif)
Requirements: convert (part of imagemagick)

### Circuit Macros (*.cir)
Requirements: m4, dpic and epstool

circuits macros are assumed to be installed in ${HOME}/.kde/share/apps/cirkuit/circuit_macros which is the case if you use cirKuit, but the location can be configured in the latexmkrc file

### Dia (*.dia)
Requirements: dia

### Glossaries
Requirements: makeglossaries (part of LaTeX package glossaries)

###Gnuplot (*.gp)
Requirements: gnuplot

Conversion to eps using the postscript terminal. The terminal and the output filename are set automatically, so you don't need to set it in your *.gp file. Dependencies from external data files are handled if they are named *.dat or *.csv.
Requirements: gnuplot


### SVG (*.svg)
Requirements: inkscape

For svg file there are three conversion rules:

| Rule | Howto use | Remarks |
--- | --- | ---
| svg->eps | \includegraphics{graphics/svg.eps} | Uses inkscape fonts
| svg->eps_tex | \input{graphics/svg.eps_tex} | EPS+LaTex for typesetting, this may have the wrong bounding box, see [Bugreport #380501](https://bugs.launchpad.net/inkscape/+bug/380501) |
| svg->pdf_tex | \input{graphics/svg.pdf_tex} | PDF+LaTex for typesetting |

### XFig (*.fig)
Requirements: fig2dev


##Makefile Targets
Target | Action|
------ | -------
make pdf | Build documents as pdf, all dependencies are generated, eps files are converted to pdf by newer pdflatex versions|
make ps | Build documents as postscript, all dependencies are generated |
make dvi | Build documents as dvi, all dependencies are generated |
make clean | Removes all files generated by latex runs (including bibtex), files generated from custom dependencies, files named *-eps-converted-to.pdf generated by eps to pdf-conversion of pdflatex and internal latexmk files |
make view | Build pdf and show it with evince |







