# Lab 20 - Working with zip/unzip

## Objective
Learn how to compress and extract files using the zip and unzip commands.

## Commands Used
zip -v
unzip -v
echo "This is file 1" > file1.txt
echo "This is file 2" > file2.txt
zip myarchive.zip file1.txt file2.txt
unzip -l myarchive.zip
rm file1.txt file2.txt
unzip myarchive.zip
ls
cat file1.txt
cat file2.txt

## Result
Successfully compressed multiple files into a ZIP archive, verified its contents, deleted the original files, extracted them again, and confirmed successful restoration.
