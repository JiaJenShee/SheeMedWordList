# How The FDA File Is Managed in This Project # 
1. Download the FDA file to a certain folder
   - source:[ FDA Orange Boob Data Files ](https://www.fda.gov/drugs/drug-approvals-and-databases/orange-book-data-files)
   - source File:[ Appendix A: Product Name Index ](https://www.fda.gov/media/71494/download?attachment)

2. Work under PowerShell of M$ Windows, use the most of " **Get-content** *File* | **Set-content** *FileOutPut* "
3. Extract the first colon of that "Appendix A" file by
   - using " **(-replace '~.*')** "   to erase all characters after the first separator:"~".
4. Make Each word in each line by replace in-word space Carriage Return+ Line Feed
   - using " **(-replace " ","`r`n")**
5. If a word is leading by numbers or special characters, remove them 
   - using " **(-replace '[0-9,;"\x27\(\)\-]', '' )**
6. Keep first characre upper Case and the other character to lower cases
   - using " **(  ForEach-Object { [regex]::Replace($_, '^(.)(.*)', { param($m) $m.Groups[1].Value + $m.Groups[2].Value.ToLower() } )**
7. Chain the above command with Pipe.
   ~~~
   Get-content *File* | -replace '~.*' | -replace " ","`r`n" | -replace '[0-9,;"\x27\(\)\-]', '' |  ForEach-Object { [regex]::Replace($_, '^(.)(.*)', { param($m) $m.Groups[1].Value + $m.Groups[2].Value.ToLower() } | Set-   content *FileOutPut*
   ~~~
9.  
