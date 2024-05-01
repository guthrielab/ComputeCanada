# SRA Functions: fastq-dump
Below is a quick guide on how to use SRA functions (eg. downloading SRA files from the NIH database) in Compute Canada. 
The eample used is fastq-dump.

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

TO FINISH
