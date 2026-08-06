# How The FDA File Is Managed in This Project # 
1. Download the FDA file to a certain folder
   - source:[ FDA Orange Boob Data Files ](https://www.fda.gov/drugs/drug-approvals-and-databases/orange-book-data-files)
   - source File:[The compressed (.ZIP) data file](https://www.fda.gov/media/76860/download?attachment)

2. extract the "products.txt" file to work folder

3. Work under PowerShell of M$ Windows, use the most of " **Get-content** *File* | **Set-content** *FileOutPut* "

4. Extract the first colon of that "Appendix A" file by
   - using " **(-replace '~.*')** "   to erase all characters after the first separator:"~".
   ~~~
   (Get-content FileName) -replace '~.*' | Set-content FileOutPut
   ~~~

5. Make Each word in each line by replace in-word space Carriage Return+ Line Feed
   - using " **(-replace " ","`r`n")**
   ~~~
   (Get-content FileName) -replace " ","`r`n" | Set-content FileOutPut
   ~~~
   
6. If a word is leading by numbers or special characters, remove them 
   - using " **(-replace '[0-9,;"\x27\(\)\-]', '' )**
   ~~~
   (Get-content FileName) -replace '[0-9,;"\x27\(\)\-]', '' | Set-content FileOutPut
   ~~~

7. Chain the above command together or step by step .
   ~~~
   (Get-content FileName) -replace '~.*'  -replace " ","`r`n" -replace '[0-9,;"\x27\(\)\-]'  | Set-content FileOutPut1
   ~~~

8. Sort and remove dublicate in the file list contents.
   - using " **( Sort-Object -Unique)**
   ~~~
   (Get-Content FileOutPut1) | Sort-Object -Unique | Set-Content FileOutPut2
   ~~~

9. Keep first characre upper Case and the other character to lower cases
   - using " **(  ForEach-Object { [regex]::Replace($_, '^(.)(.*)', { param($m) $m.Groups[1].Value + $m.Groups[2].Value.ToLower() } )**
   ~~~
   (Get-Content FileOutPut2) | ForEach-Object { [regex]::Replace($_, '^(.)(.*)', { param($m) $m.Groups[1].Value + $m.Groups[2].Value.ToLower() })} | Set-Content FileOutPutFinal
   ~~~
10. Final Check the result.
