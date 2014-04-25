tiffs2pdfs
==========

Scripts to generate combined PDFs from TIFF images.

## Requirements

- Uses ImageMagick `convert` to create PDFs from TIFF files.

- Uses `pdfmerge` to combine single-image PDFs into a single PDF.

- `pdfmerge` is a front-end for Ghostscript.

## Installation

Copy this directory onto you computer somewhere and add its `bin` directory
to your PATH:

```bash
export PATH=/path/to/tiffs2pdfs/bin:$PATH
```

Alternately, you can copy `tiffs2pdf` and `tiffs2pdfs-functions` to a directory
that's already in your path, like `$HOME/bin`.

## Usage

Pass the script the name of a source directory containing TIFF's to convert:

```bash
$ tiffs2pdf /path/to/my_tiffs
```

`tiffs2pdf` will create a PDF in the directory with the same name as the
source directory base name + `.pdf`: 

```bash
/path/to/my_tiffs/my_tiffs.pdf
```
