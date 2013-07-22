epub-anatomy
============

Anatomy and Physiology test data

Converting EPS files to SVG
---------------------------

Install ghostscript (`gs`) and inkscape:

    brew install ghostscript inkscape

Then, for each EPS file:

    echo "$1" # Path to the EPS file (mminus the extension)
    gs -dEPSCrop -dEPSFitPage -sDEVICE=pdfwrite -o temp.pdf "$1.eps" 2> /dev/null > /dev/null
    /Applications/Inkscape.app/Contents/Resources/bin/inkscape --export-plain-svg="$1.svg" -z temp.pdf 2> /dev/null
