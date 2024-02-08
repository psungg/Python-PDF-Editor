# Python-PDF-Editor

## PDF File Rotation

1. Create PdfFileReader and PdfFileWriter Object
2. Read through each page of the original PDF File and rotate it regard to the specific degree
3. Add each rotated page to the PdfFileWriter Object
4. Write all data from PdfFileWriter to a new PDF File

```python
import PyPDF4

def PDFRotation(originalFileName, newFileName, rotateAngle):
    pdfReader = PyPDF4.PdfFileReader(originalFileName)
    pdfWriter = PyPDF4.PdfFileWriter()
    
    for page in range(pdfReader.getNumPages()):
        pageObject = pdfReader.getPage(page)
        pageObject.rotateClockwise(rotateAngle)
        
        pdfWriter.addPage(pageObject)
    newFile = open(newFileName, "wb")
    pdfWriter.write(newFile)
    newFile.close()
    print("Done!")

originalFileName = "Sample.pdf"
newFileName = "RotatedSample.pdf"
rotateAngle = 270
PDFRotation(originalFileName, newFileName, rotateAngle)
```

## PDF File Merging

1. Specify PDF files (2 and above) for merging
2. Create PdfFileMerger Object
3. Read through each PDF files and merge all files together
4. Write all data from PdfFileMerger object to a new PDF File

```pyhton
import PyPDF4

def PDFMerger(pdfs, outputFileName):
    pdfMerger = PyPDF4.PdfFileMerger()
    for pdf in pdfs:
        pdfMerger.append(pdf)
    newFile = open(outputFileName, 'wb')
    pdfMerger.write(newFile)
    newFile.close()
    print("Done!")
    
pdfs = ["Sample.pdf","RotatedSample.pdf"]
outputFileName = "CombineSamples.pdf"
PDFMerger(pdfs, outputFileName)
```

## PDF File Splitting

1. Specify splitting position e.g [2,5,10]
2. Initialize the start and end page
3. For each spliting position

    A. Create a PdfFileReader
    
    B. Create a PdfFileWriter
    
    C. Add the start to end pages to the PdfFileWriter Object
    
    D. Write all data from PdfFileMerger object to a new PDF File (for each splitting position)
    
    E. Indicate the new start and end page from the splitting position list

```python
import PyPDF4

def PDFSplitting(originalFileName, splitPosition):
    start = 0
    end = splitPosition[0]
    for i in range(len(splitPosition)+1): #### must + 1 because code start from 0
        pdfReader = PyPDF4.PdfFileReader(originalFileName)
        pdfWriter = PyPDF4.PdfFileWriter()
        
        for page in range(start,end):
            pdfWriter.addPage(pdfReader.getPage(page))
        outputPdf = "SplittedFile"+str(i+1)+".pdf"
    
        newFile = open(outputPdf, 'wb')
        pdfWriter.write(newFile)
        newFile.close()
    
        start = end #### start from last end
    
        try:
            end = splitPosition[i+1] #### check if there any next index left
        except IndexError:
            end = pdfReader.getNumPages()
        print("Done!")

originalFileName = "CombineSamples.pdf"
splitPosition = [2,5] #1st file: 1-2, 2nd file: 3-5, 3rd: 6+
PDFSplitting(originalFileName, splitPosition)
```

## PDF Watermark

1. Specify the original PDF file and Watermark file
2. Create a PdfFile Reader object for original PDF file
3. Create a PdfFileReader object for watermark file and get the page object
4. Create a PdfFileWriter object
5. For each page of the original PDF file
    
    A. Merge the watermark object to each page
    
    B. Add each watermarked page to the PdfFile Writer Object
    
    C. Write all data from PdfFileWriter object to a new PDF File (watermarked)

```python
import PyPDF4

def watermarkMaking(originalFileName, outputFileName, WatermarkFileName):
    pdfReader = PyPDF4.PdfFileReader(originalFileName)
    watermarkReader = PyPDF4.PdfFileReader(watermarkFileName)
    mergePDF = PyPDF4.PdfFileWriter()
    
    watermark = watermarkReader.getPage(0)
    
    for page in range(pdfReader.getNumPages()):
        currentPage = pdfReader.getPage(page)
        currentPage.mergePage(watermark)
        
        mergePDF.addPage(currentPage)
        
    newFile = open(outputFileName, 'wb')
    mergePDF.write(newFile)
    newFile.close()
    print ("Done!")
    

originalFileName = "WatermarkTest.pdf"
outputFileName = "WatermarkPortfolio.pdf"
watermarkFileName = "WatermarkLogo.pdf"
watermarkMaking(originalFileName, outputFileName, watermarkFileName)
```
