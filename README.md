# ComputeCanada
Documentation on Submitting jobs to the Compute Canada Cluster

# Getting Started

## SSH log in
To use the Graham cluster, simply enter the following command in your terminal:
`ssh username@graham.computecanada.ca`

## Submitting jobs using SLURM
To run a simple job, start by creating a text file using the following example: `nano my_firstjob.sh`
```
#!/bin/bash
#SBATCH --time=00:05:00 
#SBATCH --account=def-username
echo 'Hello World'
sleep 30
```
Use the command **sbatch** to submit the job and you will have a submitted batch job notification:
```
$ sbatch my_firstjob.sh
Submitted batch job 123456
```
