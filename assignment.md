Assignment: Shell Tools and Data Analysis

Question 1: Get the data files

Problem
Download the following data files using the `curl` command:
- [test.fastq.gz](http://eaton-lab.org/pdsb/test.fastq.gz)
- [iris-data-dirty.csv](http://eaton-lab.org/pdsb/iris-data-dirty.csv)

Use `less` or `zless` to view the files. Finally, use the `head` command to print the first 5 lines of the file `iris-data-dirty.csv`.

Solution
Download files:

curl -O http://eaton-lab.org/pdsb/test.fastq.gz
curl -O http://eaton-lab.org/pdsb/iris-data-dirty.csv


View files:

less iris-data-dirty.csv
zless test.fastq.gz


Print first 5 lines of `iris-data-dirty.csv`:

head -n 5 iris-data-dirty.csv




Question 2: Clean the data

Problem
Check and clean the species names and missing values (`NA`) in `iris-data-dirty.csv`. Replace misspelled names, remove lines with `NA`, and save the cleaned data as `iris-data-clean.csv`. List the number of data values for each species.

Solution
Check for misspelled names and missing values:

grep -i 'NA' iris-data-dirty.csv
grep -v -i 'setosa\|versicolor\|virginica' iris-data-dirty.csv


Clean the data:

sed '/NA/d' iris-data-dirty.csv | \
sed 's/Setoosa/Setosa/g; s/Virsicolor/Versicolor/g' > iris-data-clean.csv


Count data values for each species:

cut -d ',' -f 5 iris-data-clean.csv | sort | uniq -c


---

Question 3: Find a sequence

Problem
Count the number of lines in `test.fastq.gz` that start with "TGCAG" and end with "GAG".

Solution
Count matching lines:

zgrep -c '^TGCAG.*GAG$' test.fastq.gz


---

Question 4: Find a sequence chunk

Problem
Extract all 4-line chunks containing "AAAACCCC". For each match, include the line above and the two lines below.

Solution
 Extract 4-line chunks:

zgrep 'AAAACCCC' test.fastq.gz | xargs -I {} grep -C 2 '{}' test.fastq.gz
