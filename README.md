Howdy!
============

This is a test book on GitHub. From here you can:

1. **[Read the book online](http://philschatz.github.io/epub-anatomy/reader/)** (note the **TOC button** on the top-left and the **Annotation buttons** on the top-right of the page)
2. **[Download the PDF!](http://pdf.oerpub.org/philschatz/epub-anatomy/)** [![PDF Status](http://pdf.oerpub.org/philschatz/epub-anatomy.png)](http://pdf.oerpub.org/philschatz/epub-anatomy/) : updated every time the book is changed
3. **[Edit the Book online!](http://oerpub.github.io/github-bookeditor/#repo/philschatz/epub-anatomy)** : takes a while to load but close the login to just play around
4. [Download the book](https://github.com/philschatz/epub-anatomy/archive/master.zip) by clicking the "[Download ZIP](https://github.com/philschatz/epub-anatomy/archive/master.zip)" button on this page

----

## Danger! Below be Dragons

You've been warned! These are just notes I jotted down when building this repository.

Anatomy and Physiology test data

Converting EPS files to SVG
---------------------------

Install ghostscript (`gs`) and inkscape:

    brew install ghostscript inkscape

Then, for each EPS file:

    echo "$1" # Path to the EPS file (mminus the extension)
    gs -dEPSCrop -dEPSFitPage -sDEVICE=pdfwrite -o temp.pdf "$1.eps" 2> /dev/null > /dev/null
    /Applications/Inkscape.app/Contents/Resources/bin/inkscape --export-plain-svg="$1.svg" -z temp.pdf 2> /dev/null



Full script:

    RESOURCES_DIR=./resources

    mkdir ${RESOURCES_DIR}

    echo "Converting EPS files to SVG"
    find $1 -name "*.eps" -print0 | while read -d $'\0' EPS_PATH
    do
      FILE_PATH=$(dirname "${EPS_PATH}")
      FILE_NAME=$(basename "${EPS_PATH}" .eps)
      #echo "${EPS_PATH}"
      echo "${FILE_PATH}/${FILE_NAME}"
      gs -dEPSCrop -dEPSFitPage -sDEVICE=pdfwrite -o temp.pdf "${EPS_PATH}" 2> /dev/null > /dev/null
      /Applications/Inkscape.app/Contents/Resources/bin/inkscape --export-plain-svg="${FILE_PATH}/${FILE_NAME}.svg" -z temp.pdf 2> /dev/null

      # Copy to resources and prettify (for easy diffing) the SVG
      xmllint --nsclean --pretty 2 "${FILE_PATH}/${FILE_NAME}.svg" > "${RESOURCES_DIR}/${FILE_NAME}.svg"
    done



    # Failed attempts:

    #gs -sDEVICE=pdfwrite -o output.pdf ~/Downloads/art-unzipped/A\&P_Chapter10_Final_Arts/1003_Thick_and_Thin_Filaments.eps
    #/Applications/Inkscape.app/Contents/Resources/bin/inkscape --export-plain-svg=output.svg -z output.pdf

    #gs -dEPSCrop -dEPSFitPage -sDEVICE=pdfwrite -o temp.pdf "$1.eps" 2> /dev/null > /dev/null
    #/Applications/Inkscape.app/Contents/Resources/bin/inkscape --export-plain-svg="$1.svg" -z temp.pdf 2> /dev/null


    #rm "$1.png"
