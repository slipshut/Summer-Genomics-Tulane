###Intro to Cypress

All the information you ever needed, including setting up an account, can be found here
https://wiki.hpc.tulane.edu/trac/wiki/cypress#CodingonCypress

* Please make sure you have reviwed the cypress wiki before Thursday and in particular, how to submit job requests (https://wiki.hpc.tulane.edu/trac/wiki/cypress/using#SubmittingJobsonCypress)

Once your account is set up, logging in is easy. Replace "eenbody" with your Tulane username and then type your Tulane password when prompted.

```bash
ssh eenbody@cypress1.tulane.edu
```

###pyRAD on cypress

Now we can run the pyRAD tutorial on the cluster (http://nbviewer.jupyter.org/gist/dereneaton/1f661bfb205b644086cc/tutorial_RAD_3.0.ipynb)

* Open a new terminal window (not on cypress) and download the pyRAD RAD tutorial data into your current directory

```bash
wget -q dereneaton.com/downloads/simRADs.zip
unzip simRADs.zip
```

* Next, we're going to copy the folder over to your directory on cypress. Just replace eenbody with your username.

```bash
scp ./simRADs/ eenbody@cypress1.tulane.edu:/home/eenbody
```

* Either log into cypress or return to the window where you had cypress open. 
* Navigate into the simRADs directory. 
* Create a new job ticket file

```bash
nano pyrad_n.srun
```

* The contents should be this (make sure you understand what each line means, via the wiki above). pyRAD is already installed as a module on cypress and to run it you must load it in (module load pyrad) and execute it using pyrad (lowercase). 

```bash

#!/bin/bash
#SBATCH --job-name=OneHourJob ### Job Name
#SBATCH --nodes=1             ### Node count required for the job
#SBATCH --ntasks-per-node=1   ### Nuber of tasks to be launched per Node

module load pyrad

pyrad -n
```

* Execute the command using sbatch

```bash
sbatch pyrad_n.srun
```

* Just like running this on your desktop, if this ran successfully, you should have a params.txt file in your directory.

* On cypress, you should have no issues editing the params.txt file using sed

```bash
%%bash
sed -i '/## 7. /c\2                   ## 7. N processors... ' params.txt
sed -i '/## 10. /c\.85                ## 10. lowered clust thresh... ' params.txt
sed -i '/## 14. /c\c85m4p3            ## 14. outprefix... ' params.txt
sed -i '/## 24./c\8                   ## 24. maxH raised ... ' params.txt
sed -i '/## 30./c\*                   ## 30. all output formats... ' params.txt
```

* Now, create a new job request file called pyrad1.srun with the following info, then run it with sbatch. You should be able to do the same with all 7 steps of the RAD tutorial. 

```bash
#!/bin/bash
#SBATCH --job-name=OneHourJob ### Job Name
#SBATCH --nodes=1             ### Node count required for the job
#SBATCH --ntasks-per-node=1   ### Nuber of tasks to be launched per Node

module load pyrad

pyrad -p params.txt -s 1
```





