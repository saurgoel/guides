
# =============================================
# pdfminer
###### pdf2txt.py
pdf2txt.py extracts text contents from a PDF file. It extracts all the text that are to be rendered programmatically, i.e. text represented as ASCII or Unicode strings. It cannot recognize text drawn as images that would require optical character recognition. It also extracts the corresponding locations, font names, font sizes, writing direction (horizontal or vertical) for each text portion. You need to provide a password for protected PDF documents when its access is restricted. You cannot extract any text from a PDF document which does not have extraction permission.
which you can use to extract text and images. The command supports many options and is very flexible. Some popular options are shown below. See the usage information for complete details.
```
pdf2txt.py [options] filename.pdf
Options:
    -o output file name
    -p comma-separated list of page numbers to extract
    -t output format (text/html/xml/tag[for Tagged PDFs])
    -O dirname (triggers extraction of images from PDF into directory)
    -P password
```
That's right, you can even use the command to convert a PDF to HTML or XML! For example, say you want the HTML version of the first and third pages of your PDF, including images.
```
pdf2txt.py -O myoutput -o myoutput/myfile.html -t html -p 1,3 myfile.pdf
```
Note that the package cannot recognize text drawn as images because that would require optical character recognition. It does extract the corresponding locations, font names, font sizes, etc., for each bit of text. Often this is good enough--you can extract the text and use typical Python patterns for text processing to get the text or data into a usable form.

pdf2txt.py -p 1,2 samples/resume_1.pdf

###### dumppdf.py
dumppdf.py dumps the internal contents of a PDF file in pseudo-XML format. This program is primarily for debugging purposes, but it's also possible to extract some meaningful contents (e.g. images).
```
dumppdf.py [options] filename.pdf
Options:
    -a dump all objects
    -p comma-separated list of page numbers to extract
    -i object id
    -E dirname (extract embedded files from the PDF into directory)
    -T dump the table of contents (bookmark outlines)
    -p password
```

###### TOC Information
PDFs generally contain TOC information. This is the table of contents that usually appear on the left hand side in a pdf reader and they can be used to easily navigate the pdf files. Generally this appears on big pdf files like a book.

A layout analyzer returns a LTPage object for each page in the PDF document. This object contains child objects within the page, forming a tree structure. Figure 2 shows the relationship between these objects.

###### Layout Analysis (using PDFPageAggregator)
A layout analyzer returns a LTPage object for each page in the PDF document. This object contains child objects within the page, forming a tree structure. Figure 2 shows the relationship between these objects.
```
from pdfminer.layout import LAParams
from pdfminer.converter import PDFPageAggregator

# Set parameters for analysis.
laparams = LAParams()
# Create a PDF page aggregator object.
device = PDFPageAggregator(rsrcmgr, laparams=laparams)
interpreter = PDFPageInterpreter(rsrcmgr, device)
for page in PDFPage.create_pages(document):
    interpreter.process_page(page)
    # receive the LTPage object for the page.
    layout = device.get_result()
```

###### LTTextBox
Represents a group of text chunks that can be contained in a rectangular area. Note that this box is created by geometric analysis and does not necessarily represents a logical boundary of the text. It contains a list of LTTextLine objects. get_text() method returns the text content.

###### LTTextLine
Contains a list of LTChar objects that represent a single text line. The characters are aligned either horizontaly or vertically, depending on the text's writing mode. get_text() method returns the text content.

###### LTPage
Represents an entire page. May contain child objects like LTTextBox, LTFigure, LTImage, LTRect, LTCurve and LTLine.

###### LTAnno
Represent an actual letter in the text as a Unicode string. Note that, while a LTChar object has actual boundaries, LTAnno objects does not, as these are "virtual" characters, inserted by a layout analyzer according to the relationship between two characters (e.g. a space).

###### LTFigure
Represents an area used by PDF Form objects. PDF Forms can be used to present figures or pictures by embedding yet another PDF document within a page. Note that LTFigure objects can appear recursively.

###### LTImage
Represents an image object. Embedded images can be in JPEG or other formats, but currently PDFMiner does not pay much attention to graphical objects.

###### LTLine
Represents a single straight line. Could be used for separating text or figures.

###### LTRect
Represents a rectangle. Could be used for framing another pictures or figures.

###### LTCurve
Represents a generic Bezier curve.


# =============================================
## pdftables - a library for extracting tables from PDF files
pdftables uses pdfminer to get information on the locations of text elements in a PDF document. pdfminer was chosen as a base because it provides information on the full range of page elements in PDF files, including graphical elements such as lines. Although the algorithms currently used do not use these elements they are planned for future work. As a purely Python library, pdfminer is very portable. The downside of pdfminer is that it is slow, perhaps an order of magnitude slower than alternative C based libraries.
You need poppler and Cairo. On a Ubuntu and friends you can go:

sudo apt-get -y install python-poppler python-cairo
Then we can install the pip-able requirements from the requirements.txt file:

pip install -r requirements.txt

# =============================================
# pdfPlumber
Plumb a PDF for detailed information about each text character, rectangle, and line. Plus: Easily extract data from tables trapped in PDFs.

Works best on machine-generated, rather than scanned, PDFs. Built on pdfminer and pdfminer.six.****

# =============================================
# PDFx
Extract references (pdf, url, doi, arxiv) and metadata from a PDF. Optionally download all referenced PDFs and check for broken links.
Extract references and metadata from a given PDF
Detects pdf, url, arxiv and doi references
Fast, parallel download of all referenced PDFs
Find broken hyperlinks (using the -c flag) (more)
Output as text or JSON (using the -j flag)
Extract the PDF text (using the --text flag)
Use as command-line tool or Python package
Compatible with Python 2 and 3
Works with local and online pdfs

# =============================================
# pyPDF2
PyPDF2 is a pure-python PDF library capable of splitting, merging together, cropping, and transforming the pages of PDF files. It can also add custom data, viewing options, and passwords to PDF files. It can retrieve text and metadata from PDFs as well as merge entire files together.
```
def get_pdf_content(pdf_path, page_nums=[0]):
	content = ''
	p = file(pdf_path, "rb")
	pdf = pyPdf2.PdfFileReader(p)
	for page_num in page_nums:
		content += pdf.getPage(page_num).extractText()
	return content
```
Merge (layer)
For an example of the latter case, if you have a one-page PDF containing a watermark, you can layer it onto each page of another PDF.

Say you've created a PDF with transparent watermark text (using Photoshop, Gimp, or LaTeX). If this PDF is named wmark.pdf, the following python code will stamp each page of the target PDF with the watermark.

from PyPDF2 import PdfFileWriter, PdfFileReader
output = PdfFileWriter()

ipdf = PdfFileReader(open('sample2e.pdf', 'rb'))
wpdf = PdfFileReader(open('wmark.pdf', 'rb'))
watermark = wpdf.getPage(0)

for i in xrange(ipdf.getNumPages()):
    page = ipdf.getPage(i)
    page.mergePage(watermark)
    output.addPage(page)

with open('newfile.pdf', 'wb') as f:
   output.write(f)
If your watermark PDF is not transparent it will hide the underlying text. In that case, make sure the content of the watermark displays on the header or margin so that when it is merged, no text is masked. For example, you can use this technique to stamp a logo or letterhead on each page of the target PDF.

Merge (append)
This snippet takes all the PDFs in a subdirectory named mypdfs and puts them in name-sorted order into a new PDF called output.pdf

from PyPDF2 import PdfFileMerger, PdfFileReader
import os

merger = PdfFileMerger()
files = [x for x in os.listdir('mypdfs') if x.endswith('.pdf')]
for fname in sorted(files):
    merger.append(PdfFileReader(open(os.path.join('mypdfs', fname), 'rb')))

merger.write("output.pdf")
Delete
If you want to delete pages, iterate over the pages in your source PDF and skip over the pages to be deleted as you write your new PDF. For example, if you want to get rid of blank pages in source.pdf:

from PyPDF2 import PdfFileWriter, PdfFileReader
infile = PdfFileReader('source.pdf', 'rb')
output = PdfFileWriter()

for i in xrange(infile.getNumPages()):
    p = infile.getPage(i)
    if p.getContents(): # getContents is None if  page is blank
        output.addPage(p)

with open('newfile.pdf', 'wb') as f:
   output.write(f)
Since blank pages are not added to the output file, the result is that the new output file is a copy of the source but with no blank pages.

Split
You want to split one PDF into separate one-page PDFs. Create a new file for each page and write it to disk as you iterate through the source PDF.

from PyPDF2 import PdfFileWriter, PdfFileReader 
infile = PdfFileReader(open('source.pdf', 'rb'))

for i in xrange(infile.getNumPages()):
    p = infile.getPage(i)
    outfile = PdfFileWriter()
    outfile.addPage(p)
    with open('page-%02d.pdf' % i, 'wb') as f:
        outfile.write(f)
Slices
You can even operate on slices of a source PDF, where each item in the slice is a page.

Say you have two PDFs resulting from scanning odd and even pages of a source document. You can merge them together, interleaving the pages as follows:

#!python
from PyPDF2 import PdfFileWriter, PdfFileReader 
even = PdfFileReader(open('even.pdf', 'rb'))
odd = PdfFileReader(open('odd.pdf', 'rb'))
all = PdfFileWriter()
all.addBlankPage(612, 792)

for x,y in zip(odd.pages, even.pages):
    all.addPage(x)
    all.addPage(y)

while all.getNumPages() % 2:
    all.addBlankPage()

with open('all.pdf', 'wb') as f:
    all.write(f)
The first addBlankPage (line 5) insures that the output PDF begins with a blank page so that the first content page is on the right-hand side. Note that, when you add a blank page, the default page dimensions are set to the previous page. In this case, because there is no previous page, you must set the dimensions explicitly; 8.5 inches is 612 points, 11 inches is 792 points).

Alternatively, you might add a title page as the first page. In any case, putting the first content page on the right-hand side (odd numbered) is useful when your PDFs are laid out for a two-page (book-like) spread.

The last addBlankPage sequence (lines 11-12) insures there is an even number of pages in the final PDF. Like the first addBlankPage, this is an optional step, but could be important depending on how the final PDF will be used. For example, a print shop will appreciate that your PDFs do not end on an odd page.

###### Metadata
You can set your own metadata in a PDF:
from PyPDF2 import PdfFileMerger
def add_metadata(name, data):
    merger = PdfFileMerger()
    with open('%s.pdf' % name, 'rb') as f0:
        merger.append(f0)

    merger.addMetadata(data)

    with open('%s_update.pdf' % name, 'wb') as f1:
        merger.write(f1)

metadata = {u'/hey':u'there', u'/la':'deedah'}
add_metadata('myfile', metadata)
The function add_metadata opens the source PDF file and appends it to a new PdfFileMerger instance, resulting in a copy of the original PDF. Then it adds a special dictionary of key-value pairs into the new PDF metadata dictionary and writes the new PDF out to disk. The dictionary keys and values are unicode strings; also, the key string begins with a forward slash.

Why would you want to add metadata to a PDF? One situation is when you are processing PDFs as part of a larger workflow in which each PDF file must be processed exactly once. In that case, when you open a file for processing, you first check for the custom metadata key; if it is not present, you add the key and continuing processing the file. If it's already present, the file has already been processed, so you skip it and continue with the next file.

# =============================================
## **pdfInfo** - Pdf Metadata Tool
```
pdfinfo MVN-2013-00026-WKK/public_notice.pdf
Creator:        FUJITSU fi-4010CU
Producer:       Adobe Acrobat 9.52 Paper Capture Plug-in
CreationDate:   Fri Jan 25 09:45:08 2013
ModDate:        Fri Jan 25 09:46:16 2013
Tagged:         yes
Form:           none
Pages:          3
Encrypted:      no
Page size:      606.1 x 792 pts
Page rot:       0
File size:      199251 bytes
Optimized:      yes
PDF version:    1.6
```
Run pdfInfo on all the files
Let’s run it on all of the files.
```
$ for file in */public_notice.pdf; do pdfinfo $file && echo; done
# Lots of output here
```
What was used to produce these files?
```
$ for file in */public_notice.pdf; do pdfinfo $file|sed -n 's/Creator: *//p' ; done|sort|uniq -c
33 Acrobat PDFMaker 10.1 for Word
48 Acrobat PDFMaker 9.1 for Word
10 FUJITSU fi-4010CU
135 HardCopy
7 HP Digital Sending Device
2 Oracle9iAS Reports Services
6 PScript5.dll Version 5.2.2
4 Writer
```
When were they created?
```
$ for file in */public_notice.pdf; do pdfinfo $file|grep CreationDate: > /dev/null && date -d "$(pdfinfo $file|sed -n 's/CreationDate: *//p')" --rfc-3339 date ; done
2012-07-03
2012-07-06
2012-07-06
2012-07-06
# ...
```
How many pages do they have?
```
$ for file in */public_notice.pdf; do pdfinfo $file|sed -n 's/Pages: *//p' ; done | hist 1
 1    |   1 | 
 2    |  27 | **********
 3    | 198 | ********************************************************************************
 4    |  16 | ******
 5    |   1 | 
 8    |   2 | 
10    |   1 | 
31    |   1 | 
40    |   1 | 
TOTAL | 248 |
```

# =============================================
# pdfToText - For text only
pdftotext normally screws up the layout of PDF files, especially when they have multiple columns, but it’s fine for what I’m doing because I only need to find small chunks of text rather than a whole table or a specific line on multiple pages. As we saw earlier, most of the files contain images, so I need to run OCR. Like pdftotext, OCR programs often mess up the page layout, but I don’t care because I’m using regular expressions to look for small chunks.

# =============================================
# pdfImages - Extract Images
I don’t even care whether the images are in order; I just use pdfimages to pull out the images and then tesseract to OCR each image and add that to the text file. (This is all in the translate script that I linked above.)

# =============================================
# pdfToHtml

# When layout matters
If I care about the layout of the page, pdftotext probably won’t work. Instead, I use pdftohtml or inkscape. I’ve never needed to go deeper, but if I did, I’d use something like PDFMiner.

# =============================================
# InkScape - Convert pdf to svg
Inkscape can convert a PDF page to an SVG file. I have a little script that runs this across all pages within a PDF file.
Once you’ve converted the PDF file to a bunch of SVG files, you can open it with an XML parser just like you could with the pdftohtml output, except this time much more of the layout is preserved, including the groupings of elements on the page.
Here’s a snippet from one project where I used Inkscape to parse PDF files. I created a crazy system for receiving a very messy PDF table over email and converting it into a spreadsheet that is hosted on a website.
http://okfnlabs.org/blog/2013/12/25/parsing-pdfs.html

# =============================================
# pdf2SVG
https://github.com/scraperwiki/pdf2svg

# =============================================
# pdfImages - For Removing Images
People often think that optical character recognition (OCR) is going to be a hard part. It might be, but it doesn’t really change this decision process. If I care about where the images are positioned on the page, I’d probably use Inkscape. If I don’t, I’d probably use pdfimages, as I did here.
If I need OCR, I use pdfimages to remove the images and tesseract to run OCR. If I needed to run OCR and know more about the layout, I might convert the PDFs to SVG with Inkscape and, and then take the images out of the SVG in order to know more precisely where they are in the page’s structure.

# =============================================
# Tesseract - For OCR