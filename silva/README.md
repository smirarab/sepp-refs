The SILVA data is based on version 128 downloaded from [here](https://www.arb-silva.de/fileadmin/silva_databases/qiime/Silva_128_release.tgz).
More specifically, we used the file called `SILVA_128_QIIME_release/rep_set_aligned/99/99_otus_aligned.fasta.gz`. 
The tree was also taken from there (but was refined as detailed below). 
This file corresponds to OTUs at 99% threshold and includes 395,440 sequences. 

Then, we removed any sites that has non-gap sequences for 0.5% (i.e., 1977) sequences or less. 
In other words, sites that are 99.5% or more gaps are removed. 
This leaves us with 2349 sites. 
We did this using internal scripts built in PASTA:
~~~bash
run_seqtools.py -masksites 1977 -infile 99_otus_aligned.fasta -outfile 99_otus_aligned_masked1977.fasta
~~~

The rest of the steps were similar to what we have [described](https://github.com/smirarab/sepp/tree/master/sepp-package/buildref) for GreenGenes.
The main steps are:
~~~bash
nw_topology -bI 99_otus.tre > 99_otus_nice.tree
~~~
First, resolve:
~~~bash
raxmlHPC-PTHREADS -s 99_otus_aligned_masked1977.fasta -m GTRCAT -n scoreF-99_otus_aligned_masked1977.fasta -g 99_otus_nice.tree -F -T 24 -p 8956
~~~
Then, compute branch lengths:
~~~bash
raxmlHPC-PTHREADS -s 99_otus_aligned_masked1977.fasta -m GTRCAT -n score-bl-99_otus_aligned_masked1977.fasta -F -f e -t RAxML_result.scoreF-99_otus_aligned_masked1977.fasta -T 24 -p 10625
~~~

