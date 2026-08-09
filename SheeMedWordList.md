# How the *Updated Word List: SheeMedWordList.txt* File Is Managed in This Project # 
Well prepare the **products list of FDA** as our **HowTheFDAFileManaged.md** mentioned

Download the wordlist.txt file to a work folder, preferably the same folder as the **products list of FDA**
- source: **Aristotelis glutanimate**'s **wordlist-medicalterms-en** repository in GitHub <https://github.com/glutanimate/wordlist-medicalterms-en>
- source File: The **wordlist.txt** file <https://github.com/glutanimate/wordlist-medicalterms-en/blob/master/wordlist.txt>

## Using Linux ##
~~~
cat  wordlist.txt Products_Active_Integrant_List_2026_XX.txt  | sort -u > SheeMedWordList.txt
~~~
