# Word Cloud Generator

Generates a themed, color-coded word cloud from a document by extracting and counting domain-specific keywords and phrases. Supports multiple input file formats: `.odt`, `.docx`, `.pdf`, and plain text (`.txt`, `.md`, etc.).

## Features

- **Multi-format input**: reads OpenDocument Text (`.odt`), Word (`.docx`), PDF, and plain text files automatically based on file extension.
- **Custom keyword grouping**: assigns colors to keywords based on predefined category groups (e.g. ML/AI terms, sports terms, methodology terms).
- **Phrase extraction**: detects both single words and two-word phrases (n-grams) using `scikit-learn`.
- **Frequency-based sizing**: word size in the cloud reflects how often it appears in the source document.
- **Saves output**: renders the word cloud to a PNG image.

## Requirements

- Python 3.8+
- A virtual environment is recommended (see [Installation](#installation))

### Dependencies

| Package | Purpose |
|---|---|
| `wordcloud` | Generates the word cloud image |
| `matplotlib` | Renders and saves the final figure |
| `nltk` | Provides English stop words |
| `scikit-learn` | Extracts keyword/phrase frequencies (n-grams) |
| `odfpy` | Reads `.odt` files |
| `python-docx` | Reads `.docx` files |
| `pypdf` | Reads `.pdf` files |

## Installation

```bash
# 1. Navigate to the project folder
cd /home/keen-alise/Documents/keywords

# 2. Create and activate a virtual environment
python3 -m venv venv
source venv/bin/activate

# 3. Install dependencies
pip install wordcloud matplotlib nltk scikit-learn odfpy python-docx pypdf
```

> **Note:** On some Linux systems, running `pip install` outside a virtual environment will fail with an `externally-managed-environment` error. Using a venv (as above) avoids this entirely.

The first run will also download NLTK's English stopword list automatically if it isn't already present.

## Usage

1. Open `wordcloud_generator.py` and update the `document_path` variable near the top to point to your source file:

   ```python
   document_path = '/home/keen-alise/Documents/keywords/manoj sir.odt'
   ```

   This can be a path to an `.odt`, `.docx`, `.pdf`, or plain text file.

2. Run the script (with the virtual environment activated):

   ```bash
   python3 wordcloud_generator.py
   ```

3. The output image is saved as `keyword_wordcloud.png` in the same directory, and a preview window will pop up.

## Customizing keyword groups

Edit the `color_groups` dictionary in the script to change which keywords/phrases are detected and how they're colored:

```python
color_groups = {
    'ml_ai': ['machine learning', 'prediction', ...],
    'sports': ['football', 'premier league', ...],
    'methodology': ['statistical inference', 'probability', ...],
    'technical_tools': ['python']
}
```

Any keyword not found in these lists falls back to a default charcoal color. If none of the listed keywords are found in the document, the script automatically falls back to showing all extracted phrases instead.

## Troubleshooting

**`ModuleNotFoundError: No module named 'wordcloud'`**
The dependencies aren't installed for the Python interpreter you're using. Make sure your virtual environment is activated (`source venv/bin/activate`) before installing packages and running the script.

**`UnicodeDecodeError` when reading a document**
This means the script is trying to read a binary file (like `.odt`, `.docx`, or `.pdf`) as plain text. Confirm the file has the correct extension and that you're running the multi-format version of the script (check for `extract_text_from_odt` in the file — see below).

**Verify you have the correct script version:**
```bash
grep -n "extract_text_from_odt" wordcloud_generator.py
```
If this returns nothing, you have an outdated version of the script that only supports plain text files.

**Circular import / `ImportError` involving `collections` or `functools`**
This happens if the script file is named the same as a Python standard library module (e.g. `keyword.py`). Rename the script to something else, such as `wordcloud_generator.py`, and delete any `__pycache__` folder in the same directory.

**Scanned/image-only PDFs return no text**
`pypdf` only extracts text that's already selectable in the PDF. Scanned PDFs (images of text) require OCR (e.g. `pytesseract`), which isn't included in this script.

## Project Structure

```
keywords/
├── venv/                        # virtual environment (not tracked)
├── anydocument.odt                # example source document
├── wordcloud_generator.py       # main script
├── keyword_wordcloud.png        # generated output (after running)
└── README.md                    # this file
```
Use this to generate word cloud not LLM's. 
