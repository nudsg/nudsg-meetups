# Munging data on the command line

## Unix Tools:
- cat concatenate and print files (see what's in a file)
- grep file pattern searcher (search)
- head (tail) display first(last) lines of a file (see the top)
- awk pattern-directed scanning and processing language
- find walk a file hierarchy
- sort sort lines of text files
- wc word, line, character, and byte count
- xargs construct argument list(s) and execute utility

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

## Bitly data hacks
https://github.com/bitly/data_hacks

## Exercise

### Some Data

https://gist.github.com/campeterson/5946446

### Get the file
    wget http://www.bit.ly/aviationquery

### Rename the file to AviationData.txt
    mv aviationquery AviationData.txt

### How many lines?
    cat AviationData.txt | wc -l 

### Check out the first few lines
    head AviationData.txt

### Check out the last few lines
    tail AviationData.txt

### Split the data with 'awk' based on the '|'
    head AviationData.txt | awk '{split($0,a,"|"); print a[1],a[2],a[3]}'

### ...
    cat AviationData.txt | awk '{split($0,a,"|"); print a[5]}' | sort | uniq -c | sort -rn

### Top 10 most accident prone airports/cities
    cat AviationData.txt.crdownload | awk '{split($0,a,"|"); print a[5]}' | sort | uniq -c | sort -rn | head -n 10

### Histogram & Bar graph
    cat AviationData.txt.crdownload | awk '{split($0,a,"|"); print a[5]}' | sort | uniq -c | sort -rn | awk '{print $1}' | histogram.py | sort
    cat AviationData.txt.crdownload | awk '{split($0,a,"|"); print a[5]}' | sort | uniq -c | sort -rn | awk '{print $1}' | bar_graph.py | sort

What should we be counting?
