# Reverse engineering PDFs

**User:** Anonymous  
**Created:** 2025/10/22 19:56:04  
**Updated:** 2025/10/22 20:06:24  
**Exported:** 2026/7/3 13:23:08  
**Link:** [<https://chatgpt.com/c/68f91981-9bf4-832f-adba-9c785a2f1e5a>](https://chatgpt.com/c/68f91981-9bf4-832f-adba-9c785a2f1e5a)

## Prompt:

10/22/2025, 7:56:01 PM

do you know tools for near-lossless re-creation ("reverse engineering") of PDF files from scanned image files?  
the common method is to create PDF files with the scanned image as page "background" image and transparent text in the foreground, so the PDF looks like the original and the book contents (text) can be searched and copied ("searchable scanned PDF files")...  
but these PDF files are either excessively large (say 100 MB for a book with 200 pages) because PDF only supports JP2 images which require rather large files to preserve "good" quality (also needed for later reproduction of the book by printing),  
or when i increase the compression level to get smaller PDF files (say 10 MB for the same book), then the image quality gets really bad, so the book is no longer usable for printing...  
so ideally, i want a PDF file with no "page background images", but i want to "reverse engineer" the print layout of the book with the original font and the original positions of every character...  
are there tools for this job?

## Response:

10/22/2025, 7:56:08 PM

> **Thinking longer for a better answer**
>
> **Searching tools for recreating PDFs from scans**
>
> [FineReader Help](https://pdf.abbyy.com/media/w52judlj/users_guide_en.pdf)  
> [How to replaces images of text in PDFs with formatted ...](https://superuser.com/questions/178123/how-to-replaces-images-of-text-in-pdfs-with-formatted-text-using-ocr)  
> [Layout Parser](https://layout-parser.github.io/)  
> [PDF optimization - OCRmyPDF documentation - Read the Docs](https://ocrmypdf.readthedocs.io/en/latest/optimizer.html)  
> [Find fonts from images](https://helpx.adobe.com/fonts/using/visual-search-adobe-fonts.html)  
> [How to Handle Difficult Scans with Complex Layouts in ...](https://www.youtube.com/watch?v=hrZZEdA0kbI)  
> [I Tried 6 PDF Extraction Tools—Here's What I Learned](https://www.reddit.com/r/automation/comments/1jp5i3t/i_tried_6_pdf_extraction_toolsheres_what_i_learned/)  
> [Layout-Parser/layout-parser: A Unified Toolkit for Deep ...](https://github.com/Layout-Parser/layout-parser)  
> [WhatTheFont Font Finder - Identify Fonts by Image](https://www.myfonts.com/pages/whatthefont/?srsltid=AfmBOopwIVTjNA79khn_0CFMKD5wBBowJVJscEB93DYPKU45iFITL7wh)  
> [PDF settings](https://help.abbyy.com/en-us/finereader/15/user_guide/pdf_settings)  
> [How can I visually inspect the structure of a PDF to reverse ...](https://stackoverflow.com/questions/3549541/how-can-i-visually-inspect-the-structure-of-a-pdf-to-reverse-engineer-it)  
> [Analyzing Document Layout with LayoutParser](https://medium.com/data-science/analyzing-document-layout-with-layoutparser-ed24d85f1d44)  
> [Visual search \| Adobe Fonts](https://fonts.adobe.com/fonts/vs/upload)  
> [Speeding up the processing of documents with identical ...](https://support.abbyy.com/hc/en-us/articles/4403088768658-Speeding-up-the-processing-of-documents-with-identical-layouts)  
> [Deep Layout Parsing — Layout Parser 0.3.2 documentation](https://layout-parser.readthedocs.io/en/latest/example/deep_layout_parsing/)
>
> **Advising OCR tools and layout reconstruction techniques**
>
> [eloops/hocr2pdf: take scanned image, and hocr output ...](https://github.com/eloops/hocr2pdf)  
> [Creating a searchable PDF with opensource tools ghostscript ...](https://www.morethantechnical.com/blog/2013/11/21/creating-a-searchable-pdf-with-opensource-tools-ghostscript-hocr2pdf-and-tesseract-ocr/)  
> [hocr2pdf - hOCR to PDF converter of the ExactImage toolkit](https://manpages.ubuntu.com/manpages/jammy/man1/hocr2pdf.1.html)  
> [using tesseract hocr output to create a searchable PDF](https://groups.google.com/g/tesseract-ocr/c/tdfaEfDnPPY)  
> [hOCR Files with Tesseract / Determining if a PDF has high ...](https://stackoverflow.com/questions/48778859/hocr-files-with-tesseract-determining-if-a-pdf-has-high-quality-text-layers)  
> [4.9.1 Creating Searchable PDF's](https://oscarmanual.org/oscar_emr_12/developers/4.9-utilities/pdfsearch/)  
> [jbarlow83/ocrmypdf - Docker Image](https://hub.docker.com/r/jbarlow83/ocrmypdf)  
> [Convert hOCR to PDF - x + 3](https://xplus3.net/2009/04/02/convert-hocr-to-pdf/)  
> [Advanced features - OCRmyPDF documentation](https://ocrmypdf.readthedocs.io/en/latest/advanced.html)  
> [ExactImage](https://exactcode.com/opensource/exactimage/)  
> [How I prevent hocr2pdf to use a large font from tesseract ...](https://askubuntu.com/questions/490957/how-i-prevent-hocr2pdf-to-use-a-large-font-from-tesseract-generated-hocr-file)  
> [Investigating OCR and Text PDFs from Digital Collections](https://bibwild.wordpress.com/2023/07/18/investigating-ocr-and-text-pdfs-from-digital-collections/)  
> [Creating searchable PDFs - daiR](https://dair.info/articles/searchable_pdfs.html)  
> [How to extract text with OCR from a PDF on Linux?](https://superuser.com/questions/28426/how-to-extract-text-with-ocr-from-a-pdf-on-linux)  
> [Create OCR-Processed PDFs In 2 Steps](https://www.mindee.com/blog/create-ocrized-pdfs-in-2-steps)  
> [HOCR](https://en.wikipedia.org/wiki/HOCR)
>
> Thought for 46s

Short answer: yes - but with big caveats. Perfectly “reverse-engineering” a printed book at the character/kerning/font level (so the PDF contains *no page images* and yet looks **identical** to the original) is effectively a manual/minimally-automated typesetting job. There *are* tools and realistic workflows that get you very close (searchable text with precise positioning, much smaller files, print-quality output)-but fully automatic recovery of the original font + exact glyph metrics/kerning for every character is usually not possible without manual intervention (font identification + substitution + proofing/adjustment in a layout editor).

Below I’ll give (A) the practical tool set and why each helps, (B) a realistic pipeline you can try (open-source and commercial options), and (C) the tradeoffs + tips to get **near-lossless** printable output and small file sizes.

------------------------------------------------------------------------

# A. Tools that matter (what they do)

- **Tesseract (hOCR)** - OCR engine that can output hOCR (positional HTML) or PDF. hOCR captures words/lines with bounding boxes so you can place text exactly. Good for open workflows. ([Wikipedia](https://en.wikipedia.org/wiki/HOCR?utm_source=chatgpt.com))
- **hocr2pdf / ExactImage** - converts hOCR + images into a searchable PDF; you can control whether to keep page images or only text positioning. Useful for building PDFs that put text (vector) directly at coordinates. ([exactcode.com](https://exactcode.com/opensource/exactimage/?utm_source=chatgpt.com))
- **OCRmyPDF** - battle-tested pipeline wrapper (Tesseract + preprocessing + optimizers). It includes an image-oriented optimizer that can mask text areas and apply different compressions (monochrome text areas compressed with JBIG2/JBIG2-like approaches, images compressed separately), which helps keep file size down while preserving print quality. Also supports working with hOCR internally. ([ocrmypdf.readthedocs.io](https://ocrmypdf.readthedocs.io/en/latest/optimizer.html?utm_source=chatgpt.com))
- **LayoutParser / deep layout tools (Layout-Parser, Kraken, OCRopus, ocropus)** - ML tools to detect columns, headings, running heads, tables, images, etc. This is important if you want to *recreate a typeset layout* rather than just a naive left-to-right flow. LayoutParser can even “draw” OCR’d text back at the original positions (mode II). ([layout-parser.github.io](https://layout-parser.github.io/?utm_source=chatgpt.com))
- **ABBYY FineReader / Adobe Acrobat Pro (commercial)** - best commercial OCR + layout recovery; they can export to Word/InDesign formats (so you can reflow and recompose), have templates for repeated layouts, and give fine control over image quality vs. text. If you need highest accuracy and integrated font matching/export, these are the usual choices. ([help.abbyy.com](https://help.abbyy.com/en-us/finereader/15/user_guide/pdf_settings?utm_source=chatgpt.com))
- **Font identification tools (WhatTheFont, Adobe Fonts visual search, FontSquirrel, Identifont)** - to find the closest matching desktop font from imagery. You’ll almost always need a human to select a close font and decide whether to buy/embed it. ([myfonts.com](https://www.myfonts.com/pages/whatthefont/?srsltid=AfmBOopwIVTjNA79khn_0CFMKD5wBBowJVJscEB93DYPKU45iFITL7wh&utm_source=chatgpt.com))

------------------------------------------------------------------------

# B. Realistic pipelines (from quick → full recompose)

## 1) Fast, small, searchable PDFs (no page images visible; good printing)

This keeps high-quality *text* as selectable text and *keeps only the image areas that must remain* (figures, photos), and uses monochrome compression for text regions.

Steps (open-source):

1.  Preprocess (deskew, denoise) with **ScanTailor-Advanced** / ImageMagick / Leptonica.
2.  OCR with **Tesseract** producing **hOCR**:  
    `tesseract page.tif page -l eng hocr`
3.  Use **hocr2pdf** (ExactImage) or **OCRmyPDF** to render text into the PDF layer and **compress images differently** (text regions -\> JBIG2/CCITT, pictures -\> quality JPEG/PNG). Example with ocrmypdf:  
    `ocrmypdf --deskew --optimize 3 input.pdf output.pdf`  
    (see OCRmyPDF optimizer docs for tuning). ([manpages.ubuntu.com](https://manpages.ubuntu.com/manpages/jammy/man1/hocr2pdf.1.html?utm_source=chatgpt.com))

Why this helps: you avoid embedding full high-res background images for text (the PDF contains vector/text for OCR’d characters) yet you can keep high-quality images for figures so printed output remains good.

## 2) Near-pixel-perfect reconstruction (best-effort automatic)

This tries to place each OCR’d glyph at the same coordinate (text as vector) so pages visually match the scanned image. You’ll still likely need manual corrections.

Pipeline:

1.  Use **LayoutParser** to detect blocks/columns/inline elements and generate a layout map. ([layout-parser.github.io](https://layout-parser.github.io/?utm_source=chatgpt.com))
2.  OCR each region with Tesseract/kraken/ABBYY and export **hOCR** or **ALTO XML** (ALTO is common in digitization projects). ([Wikipedia](https://en.wikipedia.org/wiki/HOCR?utm_source=chatgpt.com))
3.  Use an hOCR→PDF renderer (custom script, hocr2pdf, or OCRmyPDF’s hOCR renderer) that places text spans exactly where the OCR engine reported them. OCRmyPDF has an hOCR renderer that aims for more accurate glyph placement than older tools. ([ocrmypdf.readthedocs.io](https://ocrmypdf.readthedocs.io/en/latest/advanced.html?utm_source=chatgpt.com))
4.  Identify font(s) with an image font matcher (WhatTheFont / Adobe). If you can find a close commercial/OSS font, substitute it and adjust sizes; import into InDesign / Scribus for manual kerning/line-fit adjustments. ([myfonts.com](https://www.myfonts.com/pages/whatthefont/?srsltid=AfmBOopwIVTjNA79khn_0CFMKD5wBBowJVJscEB93DYPKU45iFITL7wh&utm_source=chatgpt.com))

**Reality check:** automatic glyph substitution will rarely match exact metrics/kerning. If you need *press-ready exact match* you must recompose in a layout program (InDesign / Scribus / LaTeX) and do manual typographic fixes after font substitution.

## 3) Full recompose / publishing quality (manual work)

- Export OCR + structure to Word/RTF/IDML (ABBYY or Acrobat can export).
- Rebuild in **Adobe InDesign** or **Scribus** using the closest matching fonts; manually adjust kerning, tracking, line breaks and hyphenation. This is the only reliable way to reproduce print-quality identical output without embedding images. Commercial OCR that exports to IDML/Word helps a lot here. ([Pdf.Abbyy](https://pdf.abbyy.com/media/w52judlj/users_guide_en.pdf?utm_source=chatgpt.com))

------------------------------------------------------------------------

# C. Important tradeoffs & practical tips

1.  **Perfect automation ≠ exact visual match.** OCR + font-match can give you very good visual similarity, but exact glyph metrics/kerning are almost never recovered automatically. Expect manual layout work for “identical” appearance.
2.  **Don't rely on full-page JP2 only.** JPEG2000/JPEG2000 in PDF can be large or poorly supported by some tools; many open tools (OCRmyPDF) do image masking + mixed compression (monochrome text regions compressed with JBIG2/CCITT and photos with JPEG at higher quality) to get small files without visibly degrading print quality. Look to OCRmyPDF’s optimizer and masked compression approach. ([ocrmypdf.readthedocs.io](https://ocrmypdf.readthedocs.io/en/latest/optimizer.html?utm_source=chatgpt.com))
3.  **JBIG2 is your friend for clean monochrome text** (very small for black-and-white glyphs) but be cautious: lossy JBIG2 can introduce substitution artifacts; use lossless JBIG2 or vetted tool options. OCRmyPDF supports safe options. ([ocrmypdf.readthedocs.io](https://ocrmypdf.readthedocs.io/en/latest/optimizer.html?utm_source=chatgpt.com))
4.  **Use hOCR/ALTO as the canonical intermediate.** These formats carry coordinates and make it possible to (a) produce a searchable PDF without the image, or (b) recompose in a layout tool with exact boxes to place text. Tools are available (hocr2pdf, hocr-tools, OCRmyPDF) but expect some fiddling. ([Wikipedia](https://en.wikipedia.org/wiki/HOCR?utm_source=chatgpt.com))
5.  **If you must match fonts exactly:** identify the font (WhatTheFont / Adobe font visual search), license it, then recompose the content in a layout editor using OCR input to automate most text entry - then proof/adjust. ([myfonts.com](https://www.myfonts.com/pages/whatthefont/?srsltid=AfmBOopwIVTjNA79khn_0CFMKD5wBBowJVJscEB93DYPKU45iFITL7wh&utm_source=chatgpt.com))

------------------------------------------------------------------------

# Minimal example commands (practical)

- OCR + searchable PDF with ocrmypdf (open source, good defaults):

<!-- -->

    ocrmypdf --deskew --optimize 3 --redo-ocr input.pdf output.pdf

See the optimizer docs to tune compression thresholds. ([ocrmypdf.readthedocs.io](https://ocrmypdf.readthedocs.io/en/latest/optimizer.html?utm_source=chatgpt.com))

- hOCR via Tesseract:

<!-- -->

    tesseract page.tif page -l eng hocr
    hocr2pdf -i page.tif -o page.pdf page.hocr

(then combine pages). Exact options depend on hocr2pdf version. ([manpages.ubuntu.com](https://manpages.ubuntu.com/manpages/jammy/man1/hocr2pdf.1.html?utm_source=chatgpt.com))

------------------------------------------------------------------------

# Recommended approach (my pragmatic pick)

1.  If you want a mostly automated, **small** searchable PDF that still prints well: use **OCRmyPDF** with careful preprocessing (deskew, despeckle) and optimizer settings. That gets you 90% of what you want without huge file sizes. ([ocrmypdf.readthedocs.io](https://ocrmypdf.readthedocs.io/en/latest/optimizer.html?utm_source=chatgpt.com))
2.  If you need the PDF to contain *no page images* and yet be visually very close: run OCR → export hOCR/ALTO → font-identify → import into InDesign/Scribus and recompose (semi-automatic). Expect manual typographic work. Use LayoutParser to speed up block detection. ([layout-parser.github.io](https://layout-parser.github.io/?utm_source=chatgpt.com))
3.  If you have budget/time constraints and need scale: consider **ABBYY FineReader** (commercial) or enterprise OCR services that export to IDML/Word with higher accuracy and layout templates. ([help.abbyy.com](https://help.abbyy.com/en-us/finereader/15/user_guide/pdf_settings?utm_source=chatgpt.com))

------------------------------------------------------------------------

# Final plain truth

- There are robust tools and pipelines to produce *searchable, printable, and fairly compact* PDFs from scans (OCRmyPDF + masking/compression, hOCR→PDF, Tesseract, LayoutParser). ([ocrmypdf.readthedocs.io](https://ocrmypdf.readthedocs.io/en/latest/optimizer.html?utm_source=chatgpt.com))
- But **fully automatic, lossless “reverse-engineering”** that results in a PDF with *no page images* and **pixel-for-pixel identical** appearance using the original font/kerning is not generally achievable without manual typesetting work (font licensing + manual corrections). You can get close; getting perfect requires human in the loop.

------------------------------------------------------------------------

If you want, I can:

- propose a concrete open-source command pipeline for a sample 200-page book (exact commands for preprocessing, OCR, optimizer flags), or
- sketch a step-by-step workflow targeted at maximum print quality + small file size (with recommended OCRmyPDF / hOCR settings and where to do manual corrections).

Which would you prefer?

------------------------------------------------------------------------

**Sources:**

- [HOCR](https://en.wikipedia.org/wiki/HOCR?utm_source=chatgpt.com)
- [ExactImage](https://exactcode.com/opensource/exactimage/?utm_source=chatgpt.com)
- [PDF optimization - OCRmyPDF documentation - Read the Docs](https://ocrmypdf.readthedocs.io/en/latest/optimizer.html?utm_source=chatgpt.com)
- [Layout Parser](https://layout-parser.github.io/?utm_source=chatgpt.com)
- [PDF settings](https://help.abbyy.com/en-us/finereader/15/user_guide/pdf_settings?utm_source=chatgpt.com)
- [WhatTheFont Font Finder - Identify Fonts by Image](https://www.myfonts.com/pages/whatthefont/?srsltid=AfmBOopwIVTjNA79khn_0CFMKD5wBBowJVJscEB93DYPKU45iFITL7wh&utm_source=chatgpt.com)
- [hocr2pdf - hOCR to PDF converter of the ExactImage toolkit](https://manpages.ubuntu.com/manpages/jammy/man1/hocr2pdf.1.html?utm_source=chatgpt.com)
- [FineReader Help](https://pdf.abbyy.com/media/w52judlj/users_guide_en.pdf?utm_source=chatgpt.com)

------------------------------------------------------------------------

Powered by [ChatGPT Exporter](https://www.chatgptexporter.com)
