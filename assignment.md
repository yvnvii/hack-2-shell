
# Assignment: Shell Tools and Data Analysis

## Question 1: Get the data files

### Problem
Download them
- [test.fastq.gz](http://eaton-lab.org/pdsb/test.fastq.gz)
- [iris-data-dirty.csv](http://eaton-lab.org/pdsb/iris-data-dirty.csv)

### Solution
#### Download files:
```bash
curl -O http://eaton-lab.org/pdsb/test.fastq.gz
curl -O http://eaton-lab.org/pdsb/iris-data-dirty.csv
```

#### View files:
```bash
less iris-data-dirty.csv
zless test.fastq.gz
```

#### Print first 5 lines of `iris-data-dirty.csv`:
```bash
head -n 5 iris-data-dirty.csv
```

---

## Question 2: Clean the data

### Problem
Check and clean the species names and missing values (`NA`) in `iris-data-dirty.csv`. Replace misspelled names, remove lines with `NA`, and save the cleaned data as `iris-data-clean.csv`. List the number of data values for each species.

### Solution
#### Check for misspelled names and missing values:
```bash
grep -i 'NA' iris-data-dirty.csv
grep -v -i 'setosa\|versicolor\|virginica' iris-data-dirty.csv
```

#### Clean the data:
```bash
sed '/NA/d' iris-data-dirty.csv | \
sed 's/Setoosa/Setosa/g; s/Virsicolor/Versicolor/g' > iris-data-clean.csv
```

#### Count data values for each species:
```bash
cut -d ',' -f 5 iris-data-clean.csv | sort | uniq -c
```

---

## Question 3: Find a sequence

### Problem
Count the number of lines in `test.fastq.gz` that start with "TGCAG" and end with "GAG".

### Solution
#### Count matching lines:
```bash
zgrep -c '^TGCAG.*GAG$' test.fastq.gz
```

---

## Question 4: Find a sequence chunk

### Problem
Extract all 4-line chunks containing "AAAACCCC". For each match, include the line above and the two lines below.

### Solution
#### Extract 4-line chunks:
```bash
zgrep 'AAAACCCC' test.fastq.gz | xargs -I {} grep -C 2 '{}' test.fastq.gz
```
