# Getting Started With Kylo Documentation

## Install Sphinx and Pandoc

 $ brew install python

 $ sudo pip install sphinx

 $ sudo pip install sphinx-autobuild

Download Pandoc

The open source application Pandoc is useful for this conversion. Download the appropriate Pandoc package from https://github.com/jgm/pandoc/releases/tag/1.19.1 .
Install Pandoc

Use Homebrew and run this command:

 $ homebrew: brew install pandoc

## Adding Documents
There are two methods for creating new documents.

1. Create a word document and convert to RST
2. Start from RST directly

### Option 1: Convert Docx to RST

1. Author a word document
2. Run Pandoc to convert

 $ pandoc -f docx NameofFile.docx -t rst -o NameofFile.rst

3. List next steps here

### Option 2: Start with RST

1. Create RST document by making a  new file with the "rst" extension
2. Run "make html" to rebuild the HTML locally.
3. View the HTML file in the _build folder to
