

#TMD bioinformatic scripts

This project is made up of a bundle of scripts that aims to study transmembrane sequence data. There are several pipelines that can be easily used.


###Getting started | Requirements.

The scripts require `python2`, `python3`, `perl`, and `Biopython` to work fully.

Do not try and run simultaneous instances of these scripts as this will probably cause files to be lost and mishmashed together.

Hopefully you have a copy of python installed already. If not, head [here](https://www.python.org/downloads/) to download python. Currently the Python of the project is written in Python2 which is still the most supported and widely used version. There is a stub of a branch in GitHub that has the same scripts written in python3 however they are too buggy to currently be used.

####Installing Biopython:

Whilst python and perl are readily available witha  quick google, Biopython is a bit less common. After you have installed python,

 - Open a terminal.
 - Run the following commands:

 	`sudo easy_install pip`

 	`sudo pip install numpy`

	`sudo pip install Biopython`

  `sudo python3 -m pip install Biopython`

If you come across any errors it is probably because python is not installed in the default locations, or a package has already been installed before you did these commands. Type in the above commands one after the other regardless and then try running the TMD function.





#TMD_sorting

###Finding TMDs and flanking regions within an input file.



TMDs and flanking regions can be extracted from uniprot text files *(like these downloaded by* `FETCH` *).*

The script `scripts/TMDcatcher.py` is used to do this.

`TMDcatcher.py` is a script written in `biopython`.


TMDcatcher searches uniprot text files for domains labelled TRANSMEM and assumes the flanking regions are ±5 residues. This will be made to be adjustable in the future.

The output is divided into several `.fasta` files:

 1. *complete fasta sequence of the flanking regions and the TMD.*
 2. *N terminal fasta only.*
 3. *C terminal fasta only.*
 4. *TMD fasta only.*

It is important to note that polytopic proteins will have each TMH returned as a separate fasta file.

This is particularly relevant for SA or TA studies where only a single TMD is present in each protein.

If an ID appears in more than 1 fasta sequence then the protein has been annotated as having multiple TMDs.

Due to the way biopython works, the script sort of runs 4 times and makes a new output file each time rather than once and saving 4 output files. This is more obvious when looking at the source code of the script, and is only worth worrying about if you are interested in streamlining the script for whatever reason!

Currently it counts all the flanking regions and TMD 4 times and saves different variations of them to the fasta files (TMD only, all together, N only, and then C only).

###Identify the single or multi-pass transmembrane proteins in a fasta file generated by the `TMD` function.

`COUNT` reports if the domains are from a single pass protein or a multipass protein and makes new fasta files of the single pass in one fasta file, and the multi-pass in another.

This means that you will end up with three fasta files:

 1. `input/` *The original file.*
 2. `output/` *Single pass only TMDs.*
 3. `output/` *Multi pass TMDs.*

`COUNT` requires a `fasta` format like that outputted from `TMD`. `TMD` in turn requires a `.dat` file like that generated from `FETCH`.

The script is not robust. It will only report succesfully if the domains for each protein are adjacent (see section on `FASTA` files).

#COUNT2

###Sorts the output of a `TMD` function into fasta files according to their number of transmembrane domains.

In addition to the sorted fasta files, which are stored in the outputs folder, `COUNT2` also calculates the Kyte & Doolittle Hydrophobicity for each transmembrane domain in a seperate file and divides these files according to how many transmembrane domains there were.

*`COUNT2` requires `Biopython` and `Python3` to run.*




 ---

#Kyte & Doolittle

###Calculating windowed Kyte & Dolittle hydropathy values.


This function calculates 19AA windowed average at each position along a sequence. It was developed by Dr Jim Warwicker at the University of Manchester.

There is other information that can be generated by this script. The biggest differences between KDs for successive TMDs in the sequence. Currently the source code is not available for open development.

<!-- setenv (or bash or whatever equivalent) TM_GOODBAD yes nice KDcalc.pl >&! runB.log First just look at TM_goodbad_ranking.txt, from the KDcalc.pl run, which is only made with the env variable setting above. -->

---

#Uniprot

###Downloads uniprot files using a search query or from a given list.


This function searches your text query against the uniprot database and return a complete `.dat` file of all the hits. This file can be used as an input for the function `TMD`. A log file is also made containing the list of hits.

#####Under the hood.

`FETCH` calls a uniprot url search and saves the ID hit list to a txt file. Then each call is queried against uniprot and the results are appended to another text file. This has a drawback in that the file is perfectly stable if the download is interrupted. Perhaps some sort of error handling should be used here.

In python it is as simple as:

` urllib.urlretrieve('http://www.uniprot.org/uniprot/%s.txt' % i, filename='uniget.dat') `

Where `i` is a variable that contains a uniprot ID.

Instead of spaces use a plus symbol `+` and the query should be fine, however some of the more sophisticated syntax is a bit weird.

*Possible future problem:* This method of retrieval is typically not a great idea since uniprot may change their url file structure. If this method ever fails *(as it probably will at some point)* then [here](https://www.biostars.org/p/85645/) is an alternative way of getting the file via `FTP` rather than `HTTP`.

<!--Need to add url syntax guidance here-->


---




---


#`.fasta` Files


Key points:

 - Must be on two lines
 - Domains from the same protein must be directly next to one another for correct identification in `COUNT`.
 - The header line must end in a `'` symbol.


To keep things consistent, all fasta sequences I/O should be kept in a multi-line fasta format. *i.e* fasta files should contain:

	>'P51648'|TMH:'464-480'|N-terminalflank:'458-463'|C-terminalflank:'481-485'
	FNKEKLGLLLLTFLGIVAAVLVAEYY
    >'P21397' | N-terminal flank: '1-5'
    RNLPS
	>'P21397'|TMH:'498-518'|N-	terminalflank:'492-497'|C-terminalflank:'519-524'
	RNLPSVSGLLKIIGFSTSVTALGFVLKYKLL
	>'P27338'|TMH:'490-516'|N-terminalflank:'484-489'|C-terminalflank:'517-520'
	HLPSVPGLLRLIGLTTIFSATALGFLAHKRGLVRV`

not:

	>'P51648' | N-terminal flank: '458-463' FNKEK
	>'P21397' | N-terminal flank: '1-5' RNLPS
    >'P21397' | N-terminal flank: '492-497' RNLPS
	>'P27338' | N-terminal flank: '484-489' HLPSV

Fasta entries are often split by domains for what we are performing. Domains from the same protein (*i.e* have the same uniprot ID) should be directly above and below one another. This is especially important for `TMDcount`.

The script `1fasta2fasta.pl` can be used to correct this if you end up with a one line output. In bash:

	echo
	mv scripts/1fasta2fasta.pl 1fasta2fasta.pl
	mv input.fasta process.in
	perl 1fasta2fasta.pl
	mv process.output input.fasta
	mv 1fasta2fasta.pl scripts/1fasta2fasta.pl

A `'` symbol is required as an "end of header" marker for some of the scripts. For example:
    >'P51648'|TMH:'464-480'|N-terminalflank:'458-463'|C-terminalflank: '481-485'
not:
    >'P51648'|TMH:'464-480'|N-terminalflank:'458-463'|C-terminalflank:'481-485

not:
    >Abbi_Custom_cyt_G’
    DSSCSWWTNWVIPAISAVAVALMYRLGMAEDGPNFYVPFSNKTG

---
