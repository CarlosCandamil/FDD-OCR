# Fast Food Franchisee Lead Extraction

## Description: 
Extract franchisee information from publicly available Fast Food Franchise Disclosure Documents (FDDs) in PDF format using Optical Character Recognition (OCR) and Regular Expressions (regex) to identify new business development leads.

## Business Case:

**Problem Statement:** Identifying potential business leads in the restaurant franchise industry can be a time-consuming and manual process. Franchise Disclosure Documents (FDDs) contain valuable information about franchisees, but extracting this information from PDFs is a challenging task.

**Solution:** Develop a project that leverages OCR and regex to extract franchisee information from FDDs, providing a efficient and automated way to identify new business development leads.

## Goals:

- Extract franchisee information from FDDs using OCR and regex.
- Develop a system to store and manage the extracted data.
- Identify new business development leads in the restaurant franchise industry.
- Target Audience: Restaurant franchisees.

## Technical Requirements:

- Python programming language.
- OCR library (Tesseract-OCR).
- Regex library (re).
- PDF processing library (PyPDF2).
- Data storage and management system (CSV).


## Project Timeline:

Week 1: Develop OCR and regex scripts to extract franchisee information from FDDs.

Week 2: Develop a system to store and manage the extracted data.

Week 3: Test and refine the project.

## Installation and Usage

1. Clone the repository to your local machine using `git clone https://github.com/your-username/your-repo-name.git`
2. Install the required dependencies using `pip install -r requirements.txt`
3. Place the PDF files in the `PDF` directory
4. Run the `pdf.ipynb` notebook to convert the PDFs to JPEGs
5. Run the `OCR.ipynb` notebook to extract the text from the JPEGs using OCR
6. Run the `Clean.ipynb` notebook to extract the franchisee information using regex


# Python Scripts:

### Run the `pdf.ipynb` notebook to convert the PDFs to JPEGs

```python
# Import the required library
from pdf2image import convert_from_path
```


```python
# Define a function to convert PDFs to JPEGs
def convert_pdf_to_jpeg(pdf_file, output_dir):
    pages = convert_from_path(pdf_file, 500)
    for count, page in enumerate(pages):
        page.save(f'{output_dir}/out{count}.jpg', 'JPEG')
```


```python
# Convert PDFs to JPEGs
convert_pdf_to_jpeg('PDF/burgerking.pdf', 'PDF/bking')
convert_pdf_to_jpeg('PDF/Popeyes/Popeyes.pdf', 'PDF/Popeyes')
convert_pdf_to_jpeg('FDD/Wendy.pdf', 'PDF/Wendy')
convert_pdf_to_jpeg('FDD/Denny.pdf', 'PDF/Denny')
```


### Run the `OCR.ipynb` notebook to extract the text from the JPEGs using OCR

```python
# Import the required libraries
import os
from PIL import Image
import pytesseract
```


```python
# Define a function to extract text from JPEGs using OCR
def extract_text_from_jpeg(folder_path):
    result_text = ""
    for filename in os.listdir(folder_path):
        if filename.endswith(".jpg"):
            file_path = os.path.join(folder_path, filename)
            with Image.open(file_path) as img:
                ocr_text = pytesseract.image_to_string(img)
                result_text += ocr_text
    return result_text
```


```python
# Extract text from JPEGs using OCR
folder_paths = ['PDF/Popeyes', 'PDF/Arbys', 'PDF/Carl', 'PDF/Chick', 'PDF/Denny']
for folder_path in folder_paths:
    result_text = extract_text_from_jpeg(folder_path)
    with open(f'{folder_path}/result.txt', 'w') as f:
        f.write(result_text)
```


### Run the `Clean.ipynb` notebook to extract the franchisee information using regex

```python
# Import the required libraries
import re
import csv
```


```python
# Define a function to extract franchisee information using regex
def extract_franchisee_info(text, pattern):
    matches = re.findall(pattern, text)
    new_list = list(set(matches))
    return new_list
```


```python

# Define a function to save the extracted information to a CSV file
def save_to_csv(file_path, data):
    with open(file_path, 'w', newline='') as file:
        writer = csv.writer(file)
        writer.writerows([[item] for item in data])
```


```python

# Extract franchisee information using regex
folder_paths = ['PDF/bking', 'PDF/Popeyes', 'PDF/Carl', 'PDF/Chick', 'PDF/Denny']
patterns = {
    'PDF/bking': r"(?<=[\n\r])\s*([A-Z][^\n\r\d]*?)\s*(?:Inc|Ltd|Corp|LLC|LP)",
    'PDF/Popeyes': r"(?<=[\n\r])\s*([A-Z][^\n\r\d]*?)\s*(?:Inc|Ltd|Corp|LLC|LP)\b",
    'PDF/Carl': r"\b[A-Za-z\s,.'-]+(?:\s+Inc\.?|\s+Ltd\.?|\s+Corp\.?|\s+LLC\.?|\s+LP\.?)\b",
    'PDF/Chick': r"(?:[A-Za-z\s,.'-]+(?:\s+Inc\.?|\s+Ltd\.?|\s+Corp\.?|\s+LLC\.?|\s+LP\.?))",
    'PDF/Denny': r"(?:[A-Za-z\s,.'-]+(?:\s+Inc\.?|\s+Ltd\.?|\s+Corp\.?|\s+LLC\.?|\s+LP\.?))"
}
for folder_path, pattern in patterns.items():
    with open(f'{folder_path}/result.txt', 'r') as f:
        text = f.read()
    franchisee_info = extract_franchisee_info(text, pattern)
    save_to_csv(f'{folder_path}/franchisee_info.csv', franchisee_info)
```

### Notes

* This project uses the following dependencies: `pdf2image`, `pytesseract`, and `Pillow`.
* The project assumes that the PDF files are in the `PDF` directory and that the output files will be written to the same directory.
* The project uses regex patterns to extract the franchisee information from the text extracted by OCR. These patterns may need to be modified or updated to accommodate changes in the format of the FDDs.
