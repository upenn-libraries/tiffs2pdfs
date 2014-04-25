#!/usr/bin/env bash
# bail on errors and show debugging stuff
read -r -d '' HELP <<-'EOF'
HELP TEXT HERE

EOF

################################################################################
### ENV STUFF
COMMAND=`basename $0`
# Load basic functions; primarily messaging functions
# Rename file name here to match actual functions
source "`dirname $0`/tiffs2pdfs-functions"

################################################################################
### TEMPFILES
# From:
#   http://stackoverflow.com/questions/430078/shell-script-templates
# create a default tmp file name
tmp=${TMPDIR:-/tmp}/prog.$$
# delete any existing temp files
trap "rm -f $tmp.?; exit 1" 1 2 3 13 15
trap "rm -f $tmp.?; exit" 0
# then do
#   ...real work that creates temp files $tmp.1, $tmp.2, ...

################################################################################
#### USAGE AND ERRORS
# Note that LOGFILE is used by functions for log commands: log_message,
# log_warning, log_error, etc.
#
# replace log file with another value if you want
LOGFILE=LOG_${COMMAND}_`tstamp`.log

usage() {
   echo "Usage: $cmd SOURCE_DIR"
   echo <<EOF

Create a PDF from all TIFFs in SOURCE_DIR. It is assumed that TIFF lexical
order is correct for PDF order.
EOF
}

print_help() {
  echo "$HELP"
}

################################################################################
### OPTIONS
while getopts "h" opt; do
  case $opt in
    h)
      usage
      print_help
      exit 1
      ;;
    \?)
      echo "ERROR Invalid option: -$OPTARG" >&2
      echo ""
      usage
      exit 1
      ;;
  esac
done

shift $((OPTIND-1))


################################################################################
### THESCRIPT


SOURCE_DIR=$1
if dir_exists "$SOURCE_DIR" SOURCE_DIR; then
  message "Using SOURCE_DIR: $SOURCE_DIR"
else
  error "Could not find SOURCE_DIR: $SOURCE_DIR"
fi

filelist=$tmp.1
# ls "$SOURCE_DIR"/*.tif | grep -v ref | head -4 > $filelist
ls "$SOURCE_DIR"/*.tif | grep -v ref > $filelist

if [ ! -s $filelist ]; then
  error "No TIFF's found in SOURCE_DIR: $SOURCE_DIR"
fi

PDF_DIR="$SOURCE_DIR/pdf"
if [ ! -d "$PDF_DIR" ]; then
  if ! mkdir "$PDF_DIR" ; then
    error "Could not make PDF dir $PDF_DIR"
  fi
fi

while read image
do
  base=`basename "$image" .tif`
  # jpg="$PDF_DIR/${base}.jpg"
  pdf="$PDF_DIR/${base}.pdf"

  # Convert  the image to a pdf, remove any thumbnail, shrink to 1800 px on
  # long side (do not enlarge; the '\>' operator), set px density to 150.
  convert "$image" -delete 1--1 -resize 1800x1800\> -density 150 "$pdf"
  if [ $? -eq 0 ]; then 
    message "Wrote $pdf"
  else
    warning "Error converting $image to PDF"
  fi
done < $filelist

basedir=`basename $SOURCE_DIR`
outpdf="$SOURCE_DIR/${basedir}.pdf"
if [ -f "$outpdf" ]; then
  backup="$outpdf"-`tstamp`
  warning "Output PDF exists backing up to: $backup"
  mv "$outpdf" "$backup"
fi

(
cd $PDF_DIR
pdfmerge *.pdf "$outpdf"
)
message "Wrote $outpdf"


################################################################################
### EXIT
# http://stackoverflow.com/questions/430078/shell-script-templates
rm -f $tmp.?
trap 0
exit 0
