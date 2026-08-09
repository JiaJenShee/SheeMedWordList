# How The FDA File Is Managed in This Project # 

1. Download the FDA file to a certain folder
   - source:[ FDA Orange Boob Data Files ](https://www.fda.gov/drugs/drug-approvals-and-databases/orange-book-data-files)
   - source File:[The compressed (.ZIP) data file](https://www.fda.gov/media/76860/download?attachment)

2. Extract the "products.txt" file to a work folder
   
## Using Linux ##
3. Work under Linux; use most of " **sed** 's/A/B/g *File*>  *FileOutPut* " and **sort**
4. Extract the first colon of the "products.txt" file by erasing all characters after the first separator:"~".
   ~~~
   sed 's/~.*//' FileName > FileOutPut1
   ~~~
5. Make one word in each line by replacing in-word spaces with Line Feed \n
   ~~~
   sed 's/ /\n/g' FileOutPut1 > FileOutPut2
   ~~~
6. If a word is leading by numbers or special characters, remove those characters 
   ~~~
   sed "s/[0-9,;\"\x27() -]//g" FileOutPut2 > FileOutPut3
   ~~~
7. Sort and remove duplicates in the file list contents.
   ~~~
   sort -u FileOutPut3 > FileOutPut4
   ~~~
8. Keep the first character uppercase and the other characters lowercase
   - use a high-level Regular Expression rule: ***"group"*** concept!
   ~~~
   sed 's/^\(.\)\(.*\)/\1\L\2/' FileOutPut4 > FileOutPut5
   ~~~
9. Use sed delete line command: '/(Pattern)/d' to remove a line with a single character
   ~~~
   sed '/^[A-Z]$/d' FileOutPut5 > FileOutPutFinal
   ~~~
NOTE: We may chain those above commands in one command line by using the sed -e option and pipelines between sed-sort-sed
   ~~~
   sed -e 's/~.*//' -e 's/ /\n/g' -e "s/[0-9,;\"\x27() -]//g" FileName | sort -u | sed -e 's/^\(.\)\(.*\)/\1\L\2/' -e '/^[A-Z]$/d' > FileOutPutFinal
   ~~~
