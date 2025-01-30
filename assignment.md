## 1. Get the data files
**Question 1:** Download the following data files from the internet using the curl command: 
http://eaton-lab.org/pdsb/test.fastq.gz and http://eaton-lab.org/pdsb/iris-data-dirty.csv. 
Use the less or zless commands to look at each file. Describe what these commands do. Finally, 
use the head command to print the first 5 lines of the file iris-data-dirty.csv.

**Solution:** 
```bash
$ curl -L -O http://eaton-lab.org/pdsb/test.fastq.gz
$ curl -L -O http://eaton-lab.org/pdsb/iris-data-dirty.csv

## 
## -O: use the remote name
## -L: redirect
## without -L and -O, it gives
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx</center>
</body>
</html>
```

## 2. Clean the data
**Question 2:** Use grep, uniq, and sed for this question. Check that all of the species names 
are spelled correctly in the file iris-data-dirty.csv. Also check for missing values stored as NA. 
Create a new file where mispelled names are replaced with the correct values, and lines with NA are 
excluded, and save it as iris-data-clean.csv. Use cut, sort and uniq to list the number of data values 
there are for each species in the new cleaned data file. Describe your work.

```
yukiogawa@Yukis-MacBook-Air-2 hack-2-shell % head iris-data-dirty.csv 
5.1,3.5,1.4,0.2,Iris-setosa
4.9,3.0,1.4,0.2,Iris-setosa
4.7,3.2,1.3,0.2,Iris-setosa
4.6,3.1,1.5,0.2,Iris-setosa
5.0,3.6,1.4,0.2,Iris-setosa
5.4,3.9,1.7,0.4,Iris-setosa
4.6,3.4,1.4,0.3,Iris-setosa
5.0,3.4,1.5,0.2,Iris-setosa
4.4,2.9,1.4,0.2,Iris-setosa
4.9,3.1,1.5,0.1,Iris-setosa
```
### checking missing valies
```
yukiogawa@Yukis-MacBook-Air-2 hack-2-shell % grep NA iris-data-dirty.csv 
6.3,NA,4.9,1.5,Iris-versicolor
5.6,NA,4.1,1.3,Iris-versicolor
```

### checking misspelled names
```bash
yukiogawa@Yukis-MacBook-Air-2 hack-2-shell % sed 's/,/\n/g' iris-data-dirty.csv | grep Iris | uniq -c
  11 Iris-setosa
   1 Iris-setsa
  38 Iris-setosa
   1 Iris-versicolour
  49 Iris-versicolor
  50 Iris-virginica
```
Iris-setsa, Iris-versicolour might be misspelled.

```
yukiogawa@Yukis-MacBook-Air-2 hack-2-shell % sed 's/Iris-setsa/Iris-setosa/g' iris-data-dirty.csv | grep setosa | wc -l
      50

## sed (Stream Editor) is used for text substitution.
## 's/Iris-setsa/Iris-setosa/g':
## s/old/new/g → This is the substitution command.
## Iris-setsa is the incorrect spelling, and it's being replaced with Iris-setosa.
## g (global) ensures all occurrences in each line are replaced.
## iris-data-dirty.csv is the input file that sed reads from.
## | grep setosa
## The pipe (|) takes the output of sed and passes it as input to grep.
## grep setosa searches for lines containing the word "setosa" in the modified output.
## | wc -l
## The pipe (|) takes the output of grep and passes it to wc -l.
## wc -l (word count with -l option) counts the number of lines that contain "setosa".
```

### creating a 'clean' data file
```
yukiogawa@Yukis-MacBook-Air-2 hack-2-shell % grep -v NA iris-data-dirty.csv | sed 's/Iris-setsa/Iris-setosa/g' | sed 's/Iris-versicolour/Iris-versicolor/g' > iris-data-clean.csv

## grep -v NA iris-data-dirty.csv: grep searches for patterns in a file.
## -v (invert match) : excludes lines that contain "NA", effectively filtering out missing or incomplete data.
## | sed 's/Iris-setsa/Iris-setosa/g': The pipe (|) passes the filtered output (without "NA" lines) to sed.
## sed 's/Iris-setsa/Iris-setosa/g' : corrects a typo by replacing "Iris-setsa" with "Iris-setosa".
## | sed 's/Iris-versicolour/Iris-versicolor/g' : corrects another typo by replacing "Iris-versicolour" with "Iris-versicolor".
## > iris-data-clean.csv : The output is redirected to iris-data-clean.csv.
```

Result
```
yukiogawa@Yukis-MacBook-Air-2 hack-2-shell % sed 's/,/\n/g' iris-data-clean.csv | grep Iris | uniq -c
  50 Iris-setosa
  48 Iris-versicolor
  50 Iris-virginica
```

## 3. Find a sequence
**Question 3:** Find how many lines in the data file test.fastq.gz start with "TGCAG" and end 
with "GAG". Describe your work.

```bash
yukiogawa@Yukis-MacBook-Air-2 hack-2-shell % gunzip -c test.fastq.gz| grep "^TGCAG" | grep "GAG$" | wc -l
      44
```

## 4. Find a sequence chunk
**Question 4:** Using grep and other tools if necessary find all lines that contain the sequence
"AAAACCCC" and for each print that line, the line above it, and two lines below it (so that a 4-line 
chunk around each search hit is printed). Describe your work.

```bash
yukiogawa@Yukis-MacBook-Air-2 hack-2-shell % gunzip -c test.fastq.gz | grep AAAACCCC
TGCAGAATAGATAGGAAACGTTTTGGCGCTGTAGACATTAAAACCCCAGTAGGACACGGGTATCACAACGTACA
TGCAGTGGATCGAAAACCCCGAGGCTCAAGGTCACGCCACCGTCTTCGTGGCCAAGTTCTTCGGCCGCGCCGGC

yukiogawa@Yukis-MacBook-Air-2 hack-2-shell % gunzip -c test.fastq.gz| grep AAAACCCC -B 1 
@32082_przewalskii.98 GRC13_0027_FC:4:1:5669:1669 length=74
TGCAGAATAGATAGGAAACGTTTTGGCGCTGTAGACATTAAAACCCCAGTAGGACACGGGTATCACAACGTACA
--
@33413_thamno.59 GRC13_0027_FC:4:1:5000:1620 length=74
TGCAGTGGATCGAAAACCCCGAGGCTCAAGGTCACGCCACCGTCTTCGTGGCCAAGTTCTTCGGCCGCGCCGGC

yukiogawa@Yukis-MacBook-Air-2 hack-2-shell % gunzip -c test.fastq.gz| grep AAAACCCC -B 1 -A 2 
@32082_przewalskii.98 GRC13_0027_FC:4:1:5669:1669 length=74
TGCAGAATAGATAGGAAACGTTTTGGCGCTGTAGACATTAAAACCCCAGTAGGACACGGGTATCACAACGTACA
+32082_przewalskii.98 GRC13_0027_FC:4:1:5669:1669 length=74
IIIIIIIIIIIIIIIIIIHIHIIIIIIIIGIIIGIIIIIIHIIIIIIIHIIIIHIIIIIIEHIHHIIIIICIHI
--
@33413_thamno.59 GRC13_0027_FC:4:1:5000:1620 length=74
TGCAGTGGATCGAAAACCCCGAGGCTCAAGGTCACGCCACCGTCTTCGTGGCCAAGTTCTTCGGCCGCGCCGGC
+33413_thamno.59 GRC13_0027_FC:4:1:5000:1620 length=74
IIIIIIIIIIIIIIIIIIIIDHIIHHIIIIIEIBGBGGGIIHEHHHIEBBHHIEGGDGIGGHAEFDBFBDDB?D
```


