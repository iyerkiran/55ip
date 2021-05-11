# Task5

## Problem Statement:
Write a script for finding and deleting duplicate files or directories
Hint: Use checksum or hash

## Proposed Solution:
A shell script to identify and delete the duplicate files.

```sh
#!/bin/bash
declare -A list_for_chksum

for file in **; do
  [[ -f "$file" ]] || continue
  ## Directory is non-empty
  ## loop through the list of files in the directory
  read capture_chksum _ < <(md5sum "$file")
  if ((list_for_chksum[$capture_chksum]++)); then
    ## md5sum match found. duplicate file
    echo "duplicate file: $file"
	# Uncomment below line to delete the duplicate file
	#rm -rf $file
  fi
done
```
