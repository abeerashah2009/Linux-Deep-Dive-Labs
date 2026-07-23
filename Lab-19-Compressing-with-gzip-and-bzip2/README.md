# Lab 19: Compressing with gzip and bzip2

## Objective

Learn how to compress and decompress files using gzip and bzip2.

## Commands Used

echo "This is a sample file for gzip compression." > example.txt

ls -lh example.txt

gzip example.txt

ls -lh example.txt.gz

gunzip example.txt.gz

ls -lh example.txt

echo "This is another sample file for bzip2 compression." > sample.txt

ls -lh sample.txt

bzip2 sample.txt

ls -lh sample.txt.bz2

bunzip2 sample.txt.bz2

ls -lh sample.txt

## Results

- Successfully compressed a file using gzip.
- Successfully decompressed using gunzip.
- Successfully compressed a file using bzip2.
- Successfully decompressed using bunzip2.
- Verified all files after each operation.
