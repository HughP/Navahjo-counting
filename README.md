# Navahjo-counting
Counting the phonemes used in a Navajo text.


##Goal
The goal of this script is to take a corpus in a given orthography, and also take a list of the phoneme equivelents and count howmany times a given phoneme is used in the corpus.

##Logic flow

```
1. Lets define the location of the text to be counted. i.e. james or the cleaned wiki text.

lang_text=$(cat corpus.txt)

2. In this courpus are strings which sometimes represent a single sound. Just like in English we have <sh>; a digram representing a single sound. Lets call these stirngs which represent a single sound "functional units".

Lets define the list of funtional units. This list must be in order of largets unit to smallest unit, where previous counts of a given unit are not going to interfere with later counts. For instance the 'ab' contained in the string 'abc' should not be  counted when we count the string 'ab'. An example in English would be that we would want to count all instances of <sh> before we count instances of <s> and instnces of <h> so that our counts of <sh> do not contribute to the total count of <s>, or <h>.

funtional_units="units-list.txt"

3. Lets tail the units list to get rid of the metadata in the header of the file. Then lets only get the first column of data. Lets put that into a file.

cat ${funtional_units} | tail -n +6 | cut -d',' -f1 > functional-units.txt

4. Pull the exact data. For each item (line) in the funtional units list, we need a count of how many times that functional unit is used in the corpus. We need this in the format of "functional unit - count" where "count" is a number. In this case capitalization does not matter and should be ignored.

for i in $(cat functional-units.txt)
do
    #sed 's/${i}/${i}\n/g' ${lang_text} | grep -c ${i} | wc -l
    echo "${i} - `grep -io "${i}" ${lang_text} | wc -w`,"
    #neither of the above two lines work at all. Something is botched... and I don't think that wc really does what I want it to do anyway. So I am willing to scratch the above code two lines for some better code (that works).
done

5. Lets also put the output of the counts in a file so that we can use it later.

As an example the output data should be in the format of the following:
  b - 43
  ch - 35
  aah - 12
  ```
