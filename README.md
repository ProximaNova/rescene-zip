# rescene-zip
Rescene a ZIP file, create info on how to bit-identically reconstruct a ZIP file from its extracted files.

## Requirements
The Bash shell and some programs. Use Linux, Cygwin in Windows, or WSL in Windows. I didn't use Windows Subsystem for Linux (WSL).

## How to do it
1. Run `grep -Poab "\x50\x4b\x03\x04" file.zip | cut -f 1 -d ":" - > prs1.txt`
2. Run `grep -Poab "\x50\x4b\x01\x02" file.zip | cut -f 1 -d ":" - > prs2.txt`
3. Run `cat prs1.txt | xargs -d "\n" sh -c 'for args do dd if="file.zip" skip=$args bs=1 count=14 | dd of="file_test.zip" seek=$args bs=1 count=14 conv=notrunc; done' _`
4. Run `cat prs2.txt | xargs -d "\n" sh -c 'for args do dd if="file.zip" skip=$args bs=1 count=300 | dd of="file_test.zip" seek=$args bs=1 count=300 conv=notrunc; done' _`
5. Test to see the no-timestamps-created "file_test.zip" is the same as the original file "file.zip" by running `sha1sum file.zip file_test.zip`

## Todo
Rearrange inputs or output for dd from/to a small file, no need to test against a duplicate large file, unless wanted.
