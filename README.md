---
layout: default
title: FDD-OCR
---

# FDD-OCR
Fast Food Franchisee Lead Extraction

This project extracts franchisee information from publicly available Fast Food Franchise Disclosure Documents (FDDs) in PDF format using Optical Character Recognition (OCR) and Regular Expressions (regex).

## Installation and Usage

1. Clone the repository to your local machine using `git clone https://github.com/your-username/your-repo-name.git`
2. Install the required dependencies using `pip install -r requirements.txt`
3. Place the PDF files in the `PDF` directory
4. Run the `pdf.ipynb` notebook to convert the PDFs to JPEGs
5. Run the `OCR.ipynb` notebook to extract the text from the JPEGs using OCR
6. Run the `Clean.ipynb` notebook to extract the franchisee information using regex

### Note

* This project uses the following dependencies: `pdf2image`, `pytesseract`, and `Pillow`.
* The project assumes that the PDF files are in the `PDF` directory and that the output files will be written to the same directory.
* The project uses regex patterns to extract the franchisee information from the text extracted by OCR. These patterns may need to be modified or updated to accommodate changes in the format of the FDDs.


```python
# Import the required library
from pdf2image import convert_from_path
     

# Define a function to convert PDFs to JPEGs
def convert_pdf_to_jpeg(pdf_file, output_dir):
    pages = convert_from_path(pdf_file, 500)
    for count, page in enumerate(pages):
        page.save(f'{output_dir}/out{count}.jpg', 'JPEG')
     

# Convert PDFs to JPEGs
convert_pdf_to_jpeg('PDF/burgerking.pdf', 'PDF/bking')
convert_pdf_to_jpeg('PDF/Popeyes/Popeyes.pdf', 'PDF/Popeyes')
convert_pdf_to_jpeg('FDD/Wendy.pdf', 'PDF/Wendy')
convert_pdf_to_jpeg('FDD/Denny.pdf', 'PDF/Denny')
```
