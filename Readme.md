#TMD bioinformatic scripts

[![Join the chat at https://gitter.im/jbkr/TM_bioinformatics](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/jbkr/TM_bioinformatics?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

This project is made up of a bundle of scripts that aims to study transmembrane sequence data. There are several pipelines that can be easily used.

The overarching method would be to have an input file in the directory, change the file directory of the input variable in the script to that file, and then run the python script. *i.e* if you had a txt file from uniprot (a dat file), and you wanted to generate a fasta of all transmembrane sequences:

 1. Move input file to `/PATH/TO/bioinformatics/YOURFILE`
 1. Open `dat_to_fasta_TMDandflanks.py` in a text editor
 2. Change the input file to you txt file
 3. Run the script by doing `python dat_to_fasta_TMDandflanks.py`

Any problems, let me know by [email](mailto:baker.james.jb@gmail.com) or [log an issue](https://github.com/jbkr/bioinformatics/issues) in github


###Getting started | Requirements.

The scripts require `python`, `python3`, `perl`, and `Biopython` to work fully.

Do not try and run simultaneous instances of these scripts as this will probably cause files to be lost and mishmashed together.

Hopefully you have a copy of python installed already. If not, head [here](https://www.python.org/downloads/) to download python.

####Installing Biopython:

Whilst python and perl are readily available with a  quick google, Biopython is a bit less common. After you have installed python,

 - Open a terminal.
 - Run the following commands:

 	`sudo easy_install pip`

 	`sudo pip install numpy`

	`sudo pip install Biopython`

  `sudo python3 -m pip install Biopython`

If you come across any errors it is probably because python is not installed in the default locations, or a package has already been installed before you did these commands. Type in the above commands one after the other regardless and then try running the TMD function.
