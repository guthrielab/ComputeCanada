# SRA Functions: fastq-dump
Below is a quick guide on how to use SRA functions (eg. downloading SRA files from the NIH database) in Compute Canada.  
The example used is fastq-dump. 
Note this was made before Compute Canada switched to StdEnv/2023 instead of StdEnv/2020. Should update this once it has been tested that StdEnv/2023 works as well. 

# Initializing the Job

## Loading Modules
After logging in, you would need to load in modules that are needed.  
Although you can just put this on the script, it is suggested to load the modules on the login node.  
```
module load gcc/9.3.0 StdEnv/2020 sra-toolkit/3.0.0
```
If other modules are needed for different jobs you can check if it available on the Compute Canada wiki.  
You can find it here: https://docs.alliancecan.ca/wiki/Available_software  
If it is not available then you would need to request it through the help desk.

## Setting up Config
Make sure temporary files are being kept in a location which has space.  
The default is the HOME directory, which has limited space and can result in your code stopping.  
This can be done with `vbd-config -i` which open an interactive channel showing where temporary files are being held.

# Making and Running the Script

## Creating a Script
To make a script you use `nano <scriptname>.sh`. Brings up a console where you can write your desired script.  
Additionally the files you want to download should be put onto a list (example `fastq_list.txt`) which guides the script what to download. 
An example script is below. The first 5 lines are needed in order to allocate the time and resources for your job. Any amount of resources can be requested, but enough memory should be requested so that your job does not fail. Check notes below the examples script. 
```
#!/bin/bash
#SBATCH --account=def-guthriej
#SBATCH --time=00:60:00
#SBATCH --mem-per-cpu=1024M
#SBATCH --cpus-per-task=2
module load gcc/9.3.0 StdEnv/2020 sra-toolkit/3.0.0
cat fastq_list.txt | parallel --eta fastq-dump --gzip --outdir 1_output --split-3 -v
```
The files should be executable already but ensures `chmod +x <scriptname>.sh` that.  
For SRA fastq-dump using 8 cores with 1GB memory seems to work (takes 2 minutes to download 1GB of files). To note is that the metadata from NCBI SRA underestimates the space/storage each takes (typically 1.2-1.5 times more than listed).   
Memory is indicated though the suffix M, G etc. indicating megabytes, gigabytes etc. Memory is allocated in intervals of 128 MB however if you put 1G a warning message will appear - do not worry it will automatically round it.  
Memory can be requested in total/per node (through `--mem`) or per core (through `--mem-per-cpu`).  

Storage is limited with the project folder of the cluster being 1 TB total for users in the group (not 1 TB per user). There are ways to get around this however:  
The scratch space is individualized for each user, plus carries a total space of 20 TB. 
Each cluster (seems to) have individualized spaces, with independent spacing restriction - i.e. 1 TB for each cluster, not 1 TB for all clusters. Thus, you can use another cluster’s project space if the space in your current clutter.  
Be sure to indicate this to Dr. Guthrie, so that spaces are accounted for.  

## Running the Script
Once the script, input file, and output directory are made, as well as having the appropriate modules loaded the script can be run.  
```
sbatch <scriptname>.sh
```

# Checking Job Status
Depending on how many jobs are running on the cluster and how many resources/time you allocated, your job can have widely variable times depending on when it starts and finishes.  
Thus it is important to survey your job as it runs.  Below are some things to keep in mind.  

## Prior to Running
When you submit, your job will be associated with a Job ID - shown when you submit the job and also when you use `sq`.  
`sq` also gives a list of all your jobs either running or pending and their priority. It also shows the resources you allocated.  
If you want to cancel a job before or during its run, you need to use `scancel <jobid>`. 

## As it Runs
As soon as your job starts there should be a slurm output. If the job is verbose it should write the file as it runs. This can be checked by:
`cat slurm-<jobID>.txt`  
If your job has output files (as in this example with fastq-dump it should) you can check if the files are being outputted as well. Checking this should not interfere with the job an can be done by just listing the files in the output folder: `ls -lh`  

## After Finishing 
You will know when your job finishes when there when you use `sq` the job will not appear.  
To check if it completed successfully you can use `seff <jobid>` where it shows either failed or completed. Additionally you can check the slurm output again. 
