# LLM Document Classifier

A small command-line Python program that extracts text from a PDF or TXT file and asks a Gemini model to classify its main subject.

## Setup

Use Python 3.10 or newer. Open this folder in VS Code, then create and activate a virtual environment in the integrated PowerShell terminal:

```powershell
py -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

If PowerShell blocks activation, run `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass` in that terminal, then activate again. In VS Code, press `Ctrl+Shift+P`, choose **Python: Select Interpreter**, and select the interpreter inside `.venv`. VS Code may also offer to select it automatically.

Create a Gemini API key in Google AI Studio. Create a `.env` file in the project folder (beside `classifier.py`) and add:

```env
GEMINI_API_KEY=your-gemini-api-key
```

The program loads this file automatically. `.env` is excluded from Git; never commit or share it. After changing `requirements.txt`, install the new dependency inside your virtual environment with `python -m pip install -r requirements.txt`.

## Run

```powershell
python classifier.py "path\to\document.pdf"
python classifier.py "path\to\document.txt"
```

Default categories are `news`, `sports`, `medical`, `finance`, `education`, `technology`, `entertainment`, `legal`, and `other`.

Choose your own labels with `--categories`:

```powershell
python classifier.py article.txt --categories news,sports,medical,science
```

The program always adds `other` if it is not in the supplied list. Set `GEMINI_MODEL` to change the default model, or pass `--model`:

```powershell
python classifier.py article.txt --model gemini-3.8-flash
```

Output is JSON containing the selected category and a brief reason. Scanned PDFs without selectable text need OCR before they can be classified. Documents longer than 80,000 characters are shortened before sending to the model. Document text is sent to the Gemini API, so use files you are permitted to process that way.
