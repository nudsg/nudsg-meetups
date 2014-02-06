# Munging data on the command line

## Getting command line tools on Windows
- Cygwin
-- http://cygwin.com
-- http://www.howtogeek.com/howto/41382/how-to-use-linux-commands-in-windows-with-cygwin/
-- http://lifehacker.com/180690/geek-to-live--introduction-to-cygwin-part-ii-+-more-useful-commands

## Available on Cygwin
### Cheatsheet http://www.oardc.ohio-state.edu/tomato/HCS806/LinuxCheatSheet2.doc
- awk (gawk) - GNU awk, a pattern scanning and processing language
- grep - search and print textual input for lines which match a specified pattern
- multitail - View one or multiple files like tail but with multiple windows

## Exercise

### Tools
- man <tool> (view manual for the tool)
- wget (file downloader)
- mv (move files/folders)
- wc (count lines, words characters and more)
- cat (concatenate and print files)
- head (displays first lines of a file)
- tail (displays last lines of a file)
- awk (pattern scanning)
- sort (sort lines of text files)
- uniq (filter out repeated lines in a file)
- ========= Extra Credit ==========
- grep (file pattern searching)
- cut (cut out selected portions of each line of a file)

### Get the file
    wget http://www.bit.ly/aviationquery

Looks like it's "|" delimited

### Rename the file to AviationData.txt
    mv aviationquery AviationData.txt

### Try out 'cat'
    cat AviationData.txt

### How many lines?
    cat AviationData.txt | wc -l 

### Check out the first few lines
    head AviationData.txt

### Check out the last few lines
    tail AviationData.txt

### Split the first few rows of data with 'awk' based on the '|', and print out 'Date', 'Make', 'Model'
    head AviationData.txt | awk '{split($0,a,"|"); print a[4],a[15],a[16]}'

### Do the above on the whole dataset
    cat AviationData.txt | awk '{split($0,a,"|"); print a[4],a[15],a[16]}'

### Output the above as tab-delimited
    cat AviationData.txt | awk '{split($0,a,"|"); print a[4],"\t",a[15],"\t",a[16]}'

### Output the above as tab-delimited and save as tsv
    cat AviationData.txt | awk '{split($0,a,"|"); print a[4],"\t",a[15],"\t",a[16]}' > accident_date_make_model.tsv

### Let's use 'sort' & 'uniq' (Goal: Top 50 most accident prone aircraft makes)
We will count the occurance of each 'make'

    cat AviationData.txt | awk '{split($0,a,"|"); print a[15]}' | sort | uniq -c

Better sort them again from highest to lowest

    cat AviationData.txt | awk '{split($0,a,"|"); print a[15]}' | sort | uniq -c | sort -rn

And just show the top 50

    cat AviationData.txt | awk '{split($0,a,"|"); print a[15]}' | sort | uniq -c | sort -rn | head -n 50

#### Question: is there more to the story here?

#### Top 50 most accident prone airports/cities
    cat AviationData.txt | awk '{split($0,a,"|"); print a[5]}' | sort | uniq -c | sort -rn | head -n 50

### Clean up some duplicates by uppercasing them all
Compare:

    cat AviationData.txt | awk '{split($0,a,"|"); print a[15]}' | sort | uniq -c | sort -rn | head -n 50

    15931  CESSNA
    8780  PIPER
    7744  Cessna
    4097  Piper
    2898  BEECH
    1748  Beech
    1431  BELL
    1116  BOEING
     893  GRUMMAN
     889  Bell
     774  Boeing

With:

    cat AviationData.txt | awk '{split($0,a,"|"); print a[15]}' | tr '[:lower:]' '[:upper:]' | sort | uniq -c | sort -rn | head -n 50

    23675  CESSNA
    12877  PIPER
    4646  BEECH
    2320  BELL
    1890  BOEING

### Using 'grep'
Show only "CESSNA" accidents

    cat AviationData.txt | grep "CESSNA" | awk '{split($0,a,"|"); print a[4],a[15],a[16]}'

### Using 'cut' (Goal: Show "CESSNA" accident occurances by year)
Remove first part of 'date'

    cat AviationData.txt | grep "CESSNA" | awk '{split($0,a,"|"); print a[4]}' | cut -c8- | head

Sort and Uniq

    cat AviationData.txt | grep "CESSNA" | awk '{split($0,a,"|"); print a[4]}' | cut -c8- | sort -rn | uniq -c

------------

## Bitly data hacks
https://github.com/bitly/data_hacks

### Install Python in Cygwin on Windows

### Histogram & Bar graph
#### Bar Chart
Just for CESSNA

    cat AviationData.txt | grep "CESSNA" | awk '{split($0,a,"|"); print a[4]}' | cut -c8- | sort -rn | bar_chart.py

All Aircraft

    cat AviationData.txt | grep "CESSNA" | awk '{split($0,a,"|"); print a[4]}' | cut -c8- | sort -rn | bar_chart.py

#### Histogram
    cat AviationData.txt | grep "CESSNA" | awk '{split($0,a,"|"); print a[4]}' | cut -c8- | sort -rn | histogram.py

### What should we be counting?

-----------
## Grep
http://www.thegeekstuff.com/2009/03/15-practical-unix-grep-command-examples/

## More Data
https://gist.github.com/campeterson/5946446
