# Garmire server login
ssh uniqname@garmire-login.dcmb.med.umich.edu
Then enter the password

# Great Lakes SLURM Tutorial

* Using the Great Lakes cluster and batch computing with SLURM
* You must establish a user login on Great Lakes by filling out [this form](https://arc.umich.edu/login-request).
* Contact IT to be added under `lgarmire` root account if you have not done so.

## Important notice on data storage
 -  People **should not** run jobs directly from login node, using [Greatlakes OnDemand](https://arc.umich.edu/open-ondemand/) or submit `sbatch` jobs instead.
 - Due to regulation, people **should not** store any Garmire's lab related data and code under default Greatlakes home directory: `/home/uniquname`
 - Instead, you **should** store all lab materials under Garmire's NAS: 
 ```
 /nfs/dcmb-lgarmire/uniqname	#personal NAS home
/nfs/dcmb-lgarmire/shared/  	#for group shared data
```
 - Use **scratch space** for large temporary data (purged after 60 days) at
 ```
 /scratch/lgarmire_root/lgarmire99/uniqname
 ```
  - Use **turbo** and **Armis2** for HIPAA data at
 ```
 /nfs/turbo/umms-lgarmire/home/uniqname
 ```



## Login

- Greatlakes login node: greatlakes.arc-ts.umich.edu
- Armis2 login node: [armis2.arc-ts.umich.edu](http://armis2.arc-ts.umich.edu/)
- Garmire lab login node: garmire-login.dcmb.med.umich.edu
- You can `ssh` login with terminal, MobaXTerm, PuTTY, etc.  (Authentication requires Level 1 password and DUO):
```
ssh uniqname@greatlakes.arc-ts.umich.edu
```
- Access your NAS home directory:
```
cd /nfs/dcmb-lgarmire/uniqname
```

## Greatlakes OnDemand: Interactive job with GUI
1. Log into https://greatlakes.arc-ts.umich.edu/pun/sys/dashboard
2. Select from `Interactive Apps` menu
3. Request for the time/memory/cores/modules as you need
4. Launch


## SLURM Commands
Full SLURM guide: https://arc.umich.edu/greatlakes/slurm-user-guide/
|Command         		|   Description                   					    |
|-----------------------|-------------------------------------------------------|
|`sbatch your_job.sbat`	|   Submit your job     			|
|`squeue -j jobid` 		|	Check job status by `jobid`            |
|`squeue -u uniqname`	|	Check job status by user's `uniqname`|
|`scancel jobid`		|	Cancel submitted job by `jobid`|
|`seff jobid`|Show total time and memory usage for job|
|`my_usage uniqname`	|	List resource usage for user `uniqname`|
|`sinfo`	|	Show node status by partition|
|`sstate`	|	Show CPU, GPU, memory allocation for each node|
|`scontrol show node node_name`	|	Show details for a job by `jobid`|
|`scontrol show job jobid`	|	Check job status by user's `uniqname`|

### Example of a .sbat job file

    #!/bin/bash  					#This has to be the first line
    #SBATCH --job-name=your_job
    #SBATCH --nodes=2
    #SBATCH --ntasks-per-node=2
    #SBATCH --time=24:00:00
    #SBATCH --mem-per-cpu=100g
    #SBATCH --partition=largemem  	#optional, largemem node has 1.5T
								    #regular node has 180G
    #SBATCH --mail-user=uniqname@umich.edu 
    #SBATCH --mail-type=END         
    #SBATCH --output=./%x-%j        
    #SBATCH --account=lgarmire99
	
    module load packages
    cd /nfs/dcmb-lgarmire/uniqname/work_directory/
    Rscript "/nfs/dcmb-lgarmire/uniqname/work_directory/script.R"




## Load modules
https://arc.umich.edu/document/environment-modules-for-using-software/

1. Pre-installed bioinformatics tools on Greatlakes including *seurat, bcftools, cellprofiler, cellranger, gatk, picard, plink, monocle*, etc.:  
```
module load Bioinformatics
```

2. Then load the packages (for example):
```
module load RSeurat/5.0.1
module load cellranger/7.2.0
```
3. search for a module
```
module spider <keyword>
```
4. Check full list of available modules: 
```
module avail
```
5. Check loaded modules:
```
module list
```
6. Remove loaded modules:
```
module rm RSeurat/5.0.1
```


## Setup conda environment
Rather than using the modules provided, people can also use conda to manage software dependencies.
#### I. Create new environment from scratch
1. Create a conda environment called `r-bioinfo` and install R base:
```
conda create -n myenv
```
2. Activate the environment and install more packages:
```
conda activate myenv
conda install conda-forge::r-seurat
```
#### II. Clone your existing conda environment to Greatlakes
1. Activate the environment
```
conda activate old_env
```
2. Export list of packages and versions
```
conda list --export > package-list.txt
```
3.  Upload the `package_list.txt` to GL and create an identical environment:
```
conda create -n new_env --file package-list.txt
```
## Hardware
![enter image description here](https://github.com/yhdu36/gl_manual/blob/main/Image/GL_Hardware.PNG?raw=true)

## Resources

* General tutorial: https://github.com/SchlossLab/Great_Lakes_SLURM
* Slides: [Introduction to the Great Lakes cluster and batch computing with SLURM](https://docs.google.com/presentation/d/1yZCyfBaK9GVCI64oUW-99HtUO5RNwSlqpeUNo8BjgWI/edit#slide=id.p1)
* Slides: [Advanced batch computing with SLURM on the Great Lakes cluster](https://github.com/SchlossLab/Great_Lakes_SLURM)
* Slides: [MPI profiling with Allinea MAP](https://cscar.research.umich.edu/wp-content/uploads/sites/5/2016/04/galexv20160606.pdf)
* ARC-TS: [Great Lakes overview](https://arc-ts.umich.edu/greatlakes/)
* ARC-TS: [Great Lakes Cheat Sheet](https://arc-ts.umich.edu/wp-content/uploads/sites/4/2020/05/Great-Lakes-Cheat-Sheet.pdf)
* ARC-TS: [SLURM user guide](https://arc-ts.umich.edu/greatlakes/slurm-user-guide/)
* ARC-TS: [Migrating from PBS-Torque to SLURM](https://arc-ts.umich.edu/migrating-from-torque-to-slurm/)
* ARC-TS: [Globus high-speed data transfer](https://arc-ts.umich.edu/globus/) 
* Kelly's [example of using Snakemake on HPC](https://github.com/kelly-sovacool/snakemake_hpc_mwe)
* [Snakemake profile for SLURM](https://github.com/Snakemake-Profiles/slurm)
* [conda on the cluster](https://github.com/um-dang/conda_on_the_cluster)

