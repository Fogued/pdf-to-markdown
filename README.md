# PDF to Markdown (Marker + Google Colab)

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Fogued/pdf-to-markdown/blob/main/PDF_to_Markdown.ipynb)

A Google Colab notebook that converts PDF, images, DOCX, PPTX, XLSX, HTML and EPUB to Markdown, HTML or JSON with [Marker](https://github.com/VikParuchuri/marker). It can use NVIDIA's free API as an LLM to improve tables, equations and text.

With Markdown output, the result is downloaded as a ZIP ready to unzip into an [Obsidian](https://obsidian.md) vault: one folder per document, with images in `images/` and equations as `$...$` and `$$...$$`. Each image file is prefixed with the document name (`name_page_0_Figure_1.jpeg`), so `images/` folders from different documents can be merged into one folder without overwriting each other.

## Usage

1. Open the notebook in Google Colab with the *Open in Colab* button above.
2. Turn on the GPU: *Runtime → Change runtime type → T4 GPU*.
3. Run the cells in order:
   1. Check the GPU.
   2. Install Marker (pinned to `1.10.2`).
   3. Choose the options.
   4. Connect to the NVIDIA API (only if you use the LLM).
   5. Upload **one or several** documents.
   6. Convert.
   7. Package and download the ZIP.

In step 5 you choose the mode: select one file to convert just that one, or several to convert them all in a batch, loading the models only once.

## Options

| Option | What it does |
|---|---|
| `USE_LLM` | Uses the NVIDIA model to improve the result. Slower, and needs an API key. |
| `NVIDIA_MODEL` | Multimodal (text + image) model from [build.nvidia.com](https://build.nvidia.com). |
| `OUTPUT_FORMAT` | `markdown` builds the Obsidian package; `html` and `json` are downloaded exactly as Marker creates them. |
| `FORCE_OCR` | Only if the text comes out garbled or the PDF is scanned. |
| `PAGE_RANGE` | Pages to convert, starting at 0 (`0-10`, `0,5-10`). Empty = all. |
| `REDO_INLINE_MATH` | Redoes the equations inside the text (only with `USE_LLM`). |

## NVIDIA API key

Only needed with `USE_LLM = True`. Get one for free at [build.nvidia.com](https://build.nvidia.com) (it starts with `nvapi-`). You can store it as a Colab secret named `NVIDIA_API_KEY` (key icon in the sidebar) or paste it when the notebook asks. The key is never saved in the notebook.

## Slides to Markdown (no LLM)

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Fogued/pdf-to-markdown/blob/main/Slides_to_Markdown.ipynb)

`Slides_to_Markdown.ipynb` is a lighter notebook for lecture slides (for example LaTeX Beamer decks) that already contain text. It reads the PDF's own text layer with PyMuPDF, so it needs no GPU, no API key and no vision LLM. A batch of 3-4 decks of about 20 pages converts in under a minute.

The output of one upload is a single `.md` file plus one `images/` folder:
- YAML front-matter at the top, then every PDF's `## Slide N: Title` sections, in filename order.
- Nested bullets, tables and links, plus LaTeX math (`$...$`).
- Every figure saved in that shared `images/` folder, with a caption placeholder and the labels found on it. Links look like `images/<deck>_slideNN_figK.png`.

The following are removed:
- Repeated headers, footers and logos.
- Incremental "build" slides, which are collapsed into their final step.
- The hidden OCR layer of OCR'd PDFs. It is only used to recover text inside pictures.

Formulas are converted from the math fonts. The small [pix2tex](https://github.com/lukas-blecher/LaTeX-OCR) model is only used for 2-D formulas (fractions, sums), and its result is kept only when it matches the symbols in the PDF.

Upload PDFs or a `.zip`, or set `DRIVE_INPUT_FOLDER` to read them from Google Drive. Set `OUTPUT_NAME` to choose the Markdown filename (for example `lecture1_intro`); leave it empty to use the shared start of the PDF names. Results download as one ZIP: `<name>.md` next to `images/`. Scanned PDFs without a text layer are not supported; use the Marker notebook for those.

## Why Marker 1.10.2

Marker 2.x needs Docker (vLLM) or llama.cpp for its layout models, which are not available in Colab, and fails with `docker binary not found`.
