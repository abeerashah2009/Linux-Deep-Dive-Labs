# Lab 18: Archiving with tar

## Objective
Learn how to create, list, and extract compressed tar archives.

## Commands Used

mkdir project

echo "This is file one." > project/file1.txt

echo "This is file two." > project/file2.txt

tar -czf project_archive.tar.gz project

tar -tzf project_archive.tar.gz

rm -r project

tar -xzf project_archive.tar.gz

ls project

## Results

- Created a compressed tar archive.
- Listed the contents of the archive.
- Extracted the archive successfully.
- Verified that the extracted files were restored.
