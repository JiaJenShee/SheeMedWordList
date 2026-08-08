# How The FDA File Is Managed in This Project # 

1. Download the FDA file to a certain folder
   - source:[ FDA Orange Boob Data Files ](https://www.fda.gov/drugs/drug-approvals-and-databases/orange-book-data-files)
   - source File:[The compressed (.ZIP) data file](https://www.fda.gov/media/76860/download?attachment)

2. extract the "products.txt" file to work folder
   
## Using Linux ##
3. work under Linux, use the most of " **sed** 's/A/B/g *File*>  *FileOutPut* " and **sort**
4. Extract the first colon of that "products.txt" file by erasing all characters after the first separator:"~".
   ~~~
   sed 's/~.*//' FileName > FileOutPut
   ~~~
5. Make one word in each line by replacing in-word spaces with Line Feed \n
   ~~~
   sed 's/ /\n/g' FileName > FileOutPut
   ~~~
6. If a word is leading by numbers or special characters, remove those characters 
   ~~~
   sed "s/[0-9,;\"\x27() -]//g" FileName > FileOutPut
   ~~~
7. Chain the above command together or step by step 
   ~~~
    sed -e 's/~.*//' -e 's/ /\n/g' -e  "s/[0-9,;\"\x27() -]//g" FileName > FileOutPut
   ~~~
8. Sort and remove duplicates in the file list contents.
   ~~~
   sort -u FileOutPut1 > FileOutPut2
   ~~~
9. Keep first characre upper Case and the other character to lower cases
   - use a high-level Regular Expression rule: ***"group"*** concept !
   ~~~
   sed 's/^\(.\)\(.*\)/\1\L\2/' FileOutPut2 > FileOutPutFinal
   ~~~

NOTE : May work in one command  using pipe-line
   ~~~
   sed -e 's/~.*//' -e 's/ /\n/g' -e "s/[0-9,;\"\x27() -]//g" FileName | sort -u | sed -e 's/^\(.\)\(.*\)/\1\L\2/' > FileOutPutFinal
   ~~~


## If insist to Work under PowerShell of M$ Windows ##
may use the most of " **Get-content** *File* | **Set-content** *FileOutPut* "

4. Extract the first colon of that "Appendix A" file by erasing all characters after the first separator:"~".
   ~~~
   (Get-content FileName) -replace '~.*' | Set-content FileOutPut
   ~~~ 
5. Make one word in each line by replacing in-word spaces with Carriage Return + Line Feed
   ~~~
   (Get-content FileName) -replace " ","`r`n" | Set-content FileOutPut
   ~~~
6. If a word is leading by numbers or special characters, remove those characters 
   ~~~
   (Get-content FileName) -replace '[0-9,;"\x27\(\)\-]', '' | Set-content FileOutPut
   ~~~
7. May chain the above command together or step by step 
   ~~~
   (Get-content FileName) -replace '~.*'  -replace " ","`r`n" -replace '[0-9,;"\x27\(\)\-]'  | Set-content FileOutPut1
   ~~~
8. Sort and remove duplicates in the file list contents.
   -  using " **( Sort-Object -Unique)**
   ~~~
   (Get-Content FileOutPut1) | Sort-Object -Unique | Set-Content FileOutPut2
   ~~~
9. Keep the first character uppercase and the other characters lowercase
   - use **(  ForEach-Object { [regex]::Replace($_, '^(.)(.*)', { param($m) $m.Groups[1].Value + $m.Groups[2].Value.ToLower() } )**
   ~~~
   (Get-Content FileOutPut2) | ForEach-Object { [regex]::Replace($_, '^(.)(.*)', { param($m) $m.Groups[1].Value + $m.Groups[2].Value.ToLower() })} | Set-Content FileOutPutFinal
   ~~~
