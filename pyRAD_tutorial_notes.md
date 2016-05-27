##Dependencies

* For context, I am running OS X El Capitan 10.11.14 on a MacBook Pro. Everything is running in bash on the OSX terminal. 

* Install or update pip and brew. Make sure you are running the latest python. Updating is important!

* Use pip to install anything python related. Everything else related to pyRAD, including pyRAD, was downloaded from their respective git repositories and placed into a folder specific to this workshop.

http://dereneaton.com/software/pyrad/

```bash
pip install scipy 
pip install numpy
```

* You can install muscle and vsearch using brew. However, vsearch is outdated.

```bash
brew install homebrew/science/muscle
brew install homebrew/science/vsearch #Don't actually do this, its better to follow my directions below
```

* To get the latest version of vsearch, follow the vsearch website directions (https://github.com/torognes/vsearch)

* I downloaded the binary of vsearch for osx using these commands.
```bash
wget https://github.com/torognes/vsearch/releases/download/v1.11.1/vsearch-1.11.1-linux-x86_64.tar.gz
tar xzf vsearch-1.11.1-linux-x86_64.tar.gz
```

* Ideally you can call vsearch from anywhere by setting a path to the folder where you put vsearch (you need to link to the bin folder specifically)

```
export PATH=~/vsearch-1.11.1-osx-x86_64/bin/:$PATH
```

* Troubleshoot: I initially had issues putting vsearch in my path (for me, it was because one of my folders had a space in it). Following massive headaches and exploration and many dead ends trying to get this version in my path, here was my solution for running vsearch: Put the whole vsearch folder in my home folder - then linked to this in the params.txt file later (it would not work anywhere but my home folder!!!) - so line four in the params.txt file looks like this for me:
```
~/vsearch-1.11.1-osx-x86_64/bin/vsearch ##4. full path
```



* Graham did a bunch of things that were quite confusing to get my pyRAD.py into my python path (so you can call it from anywhere). End of the day put the following two things in my bash profile. Edit .bashrc for a profile that you have to source every time you open the terminal. 

```bash
cd #puts you in your home directory where your profiles are located
nano .bashrc

export PYTHONPATH='usr/local/lib/python2.7/site-packages:/Users/erikenbody/Google Drive/Tulane/Conferences_and_Workshops/Tulane_Summer_Genomics_Workgroup/pyrad/pyrad'
export PATH="$PATH:/Users/erikenbody/Google Drive/Tulane/Conferences_and_Workshops/Tulane_Summer_Genomics_Workgroup/pyrad/pyrad">'

source .bashrc
```

* Everytime you open your terminal, you will have to source your .bashrc file. If you want it to automatically load, then put the same lines of text into your .bash_profile as seen below. Anything you put here will load upon start. 

```bash
cd #puts you in your home directory where your profiles are located
nano .bash_profile

export PYTHONPATH='usr/local/lib/python2.7/site-packages:/Users/erikenbody/Google Drive/Tulane/Conferences_and_Workshops/Tulane_Summer_Genomics_Workgroup/pyrad/pyrad'
export PATH="$PATH:/Users/erikenbody/Google Drive/Tulane/Conferences_and_Workshops/Tulane_Summer_Genomics_Workgroup/pyrad/pyrad">'

source .bash_profile
```

###pyRAD RAD tutorial

####Running Step 1

-I'll work through this tutorial and only make notes where I had issues beyond what was obviously stated already.

-In RAD tutorial ln 4 these commands did not work so Graham edited them. However, params file didn't come out clean so there are still issues.

```bash
sed -i -e '/## 7. /c\'$'\012''2                   ## 7. N processors... ' params.txt
sed -i -e '/## 10. /c\'$'\012''.85                ## 10. lowered clust thresh... ' params.txt
sed -i -e '/## 14. /c\'$'\012''c85m4p3            ## 14. outprefix... ' params.txt
sed -i -e '/## 24./c\'$'\012''8                   ## 24. maxH raised ... ' params.txt
ed -i -e '/## 30./c\'$'\012''*                   ## 30. all output formats... ' params.txt
```

-he also suggested this. Which still didn't quite work.

```bash
sed -i -e '/$$## 7. / s/&^/../2/' params.txt

```

-For now I will just edit params.txt

```bash
nano params.txt
```

-I also tried just editing it using my favorite text editor and this worked fine too. It also worked to copy the text from the tutorial and paste it into a new params.txt file. Its worth familiarizing with what is in this file via the General tutorial. For example, it might be nice to set the working directory for the current project this way. 

####May 17th notes (working on my own)
-I had to locate the .bashrc profile which was in my home directory. Considering adding this to my .bash_profile so it is 
always added at login. Downsides to that?

After sourcing my new .bash_profile that contains the path information, I was able to run pyRAD as such

```bash
pyRAD.py
```

-However, it said it could not find numpy and scipy again! I had to go reinstall these with pip. Why did this happen?
```bash
pip install scipy
pip install numpy
```

-random useful command. This shows the environment you're running in, including PATH and PYTHON path. Also terminal profile information.
```bash
env
```

For inspecting the fastq file, I had to unzip the RAD file. Then view with less like in the tutorial. Alternatively, you can replace less with zless for this and the future commands. 
```bash
gunzip simRADs_R1.fastq
```
Here is information on .fastq files by line:

The fastq file
Each read has 4 lines of information, which include the following:
Sequence identifier
Sequence
Quality score identifier line (consisting of a +) Quality score
Each sequence identifier, the line that precedes the sequence and describes it, needs to be in the following format:
@<instrument>:<run number>:<flowcell ID>:<lane>:<tile>:<x- pos>:<y-pos> <read>:<is filtered>: <control number>:<index sequence>
* @ @ Each sequence identifier line starts with @
* <instrument> Characters allowed: Instrument ID a-z, A-Z, 0-9 and underscore
* <run number> Numerical Run number on instrument
* <flowcell ID> Characters allowed: a-z, A-Z, 0-9
* <lane> Numerical Lane number
* <tile> Numerical Tile number
* <x_pos> Numerical X coordinate of cluster
* <y_pos> Numerical Y coordinate of cluster
* <read> Numerical Read number. 1 can be single read or read 2 of paired-end
* <is filtered> Y or N Y if the read is filtered, N otherwise
* <control number> Numerical 0 when none of the control bits are on, otherwise it is an eve


-For line 9, I hit errors while running pyRAD. It turns out it was because my working folder was Goolge Drive and has a space in it. I've since edited it to be Google_Drive, but alternatively you could run this out of your home folder. No issues running step 1.

##Steps 2-7

-keep in mind running on unix on my mac I have to run pyRAD as pyRAD.py. 
-This was pretty cookie cutter with no problems through step 7

```bash
pyRAD.py -p params.txt -s 3
```

