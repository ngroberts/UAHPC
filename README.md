# Useful Commands for the Kmkocot Usergrp on UAHPC:

## Group ID and Group Ownership

Check groups you belong to:
>id -Gn

Check ownership of files in a folder:
>ls -la

>drwxr-xr-x  6 ngroberts kmkocot  136 Apr  2 10:22 . <br/>
>drwxrwxr-- 13 kmkocot   kmkocot 4096 Mar 11 16:19 .. <br/>
>drwxr-xr-x  9 ngroberts kmkocot 4096 Dec 29 13:44 2020-10-01_206_Taxa_Tree <br/>
>drwxr-xr-x  4 ngroberts kmkocot 4096 Mar 14 15:44 2021-01-25_Assemblies <br/>
>drwxr-xr-x  4 ngroberts **users**  135 Mar 29 13:51 2021-03-26-Orthofinder_gene_trees <br/>

If a file is on users it both cannot be edited by kmkocot users and takes up memory thats not allocated to kmkocot.
You need to change this with.
>chgrp kmkocot file_name_here

or recursively (a folder and all its contents with)
>chrgrp -R kmkocot folder_name_here

If you login and notice that files you create belong to users type:
>newgrp - kmkocot 

Check storage quota on /grps2/kmk/
>quota -sg kmkocot

## Running jobs:

Check the queue for any partition:
>squeue -p partition_name_here

Check whats running for you:
>squeue -u uahpc_user_name

Check where your jobs are running:
>squeue -u $USER -o "%i %Z"

See how a job is running in realtime:
> tail -f slurm_output_file

## UAHPC Partitions:

Useful commands if you know what you are doing:
>scontrol show partition <br/>
>sacctmgr show qos <br/>
>sinfo -N -l <br/>

Note that sinfo will show you the wrong memory for the partitions due to the nature of how nodes are distributed on UAHPC.

## Here is what you need to know:
| Partition Name: | Nodes: | Cores: | Memory:                                       | Walltime:  |
|-----------------|--------|--------|-----------------------------------------------|------------|
| long            | 8      | 192    | 768GB                                         | 7-00:00:00 |
| threaded        | 2      | 80     | 5376GB (Technically infinite)                 | 48:00:00   |
| loko            | 1      | 16     | 1TB                                           | Infinite   |
| highmem         | 2      | 32     | 768 (Technically much more, depending on use) | Infinite   |
| ultrahigh       | 3      | 142    | 3.072TB                                       | Infinite   |

However it is a bit more nuanced: 
If you use
>sacctmgr show qos 

You will see that THREADED has a MAXTRES of 16and MXTRESPU 48  which means that per job there are only 16 available cores however you can run up to four jobs in parrallel with max per user CPU amount of 48.
16 Cores per job with 48 cores per user.

As well you will see using: 
>scontrol show partition 

that per core most partitions have a memory limit of 0.25GB per core. This limit will often be appplied! Without you even knowing!
You can override this by adding:
>#SBATCH --mem-per-cpu=64G

However allocating a mem amount with:
>#SBATCH --mem=1000G 

Will allocateb the right memory per core. If your program requires a per core memory of a certain amount please make sure you are allocating it. 

## SBATCH Script Setup:
>#!/bin/bash
>#SBATCH --job-name=**job_name_here**
>#SBATCH -n **1** #tasks <br/>
>#SBATCH -N **1** #nodes <br/>
>#SBATCH -c **1** #cores here <br/>
>#SBATCH --mem=**memory** goes here ex: 127G <br/>
>#SBATCH -o slurm_output-**job_name**.%J <br/>
>#SBATCH -e slurm_error-**job_name.**%J <br/>
>#SBATCH -p **partition** <br/>
>#SBATCH -q **qos** <br/>
>#SBATCH --mail-type=ALL <br/>
>#SBATCH --mail-user=**your_email@email_adress.com** <br/>

This actually varies per partition and even per qos as some partitions do not have a qos (quality of service).
### Example for loko:
>#!/bin/bash
>#SBATCH --job-name=**job_name_here**
>#SBATCH -n **1** #tasks <br/>
>#SBATCH -N **1** #nodes <br/>
>#SBATCH -c **1** #cores here <br/>
>#SBATCH --mem=**memory** goes here ex: 127G <br/>
>#SBATCH -o slurm_output-**job_name**.%J <br/>
>#SBATCH -e slurm_error-**job_name.**%J <br/>
>#SBATCH -p ultrahigh <br/>
>#SBATCH -q loko <br/>
>#SBATCH -A loko_grp <br/>
>#SBATCH --mail-type=ALL <br/>
>#SBATCH --mail-user=**your_email@email_adress.com** <br/>

### Example for threaded:
>#!/bin/bash
>#SBATCH --job-name=**job_name_here**
>#SBATCH -n **1** #tasks <br/>
>#SBATCH -N **1** #nodes <br/>
>#SBATCH -c **1** #cores here <br/>
>#SBATCH --mem=**memory** goes here ex: 127G <br/>
>#SBATCH -o slurm_output-**job_name**.%J <br/>
>#SBATCH -e slurm_error-**job_name.**%J <br/>
>#SBATCH -p threaded <br/>
>#SBATCH -q threaded <br/>
>#SBATCH --mail-type=ALL <br/>
>#SBATCH --mail-user=**your_email@email_adress.com** <br/>

### Example for long:
>#!/bin/bash
>#SBATCH --job-name=**job_name_here**
>#SBATCH -n **1** #tasks <br/>
>#SBATCH -N **1** #nodes <br/>
>#SBATCH -c **1** #cores here <br/>
>#SBATCH --mem=**memory** goes here ex: 127G <br/>
>#SBATCH -o slurm_output-**job_name**.%J <br/>
>#SBATCH -e slurm_error-**job_name.**%J <br/>
>#SBATCH -p long <br/>
>#SBATCH -q long <br/>
>#SBATCH --mail-type=ALL <br/>
>#SBATCH --mail-user=**your_email@email_adress.com** <br/>

### Example for ultrahigh:
>#!/bin/bash
>#SBATCH --job-name=**job_name_here**
>#SBATCH -n **1** #tasks <br/>
>#SBATCH -N **1** #nodes <br/>
>#SBATCH -c **1** #cores here <br/>
>#SBATCH --mem=**memory** goes here ex: 127G <br/>
>#SBATCH -o slurm_output-**job_name**.%J <br/>
>#SBATCH -e slurm_error-**job_name.**%J <br/>
>#SBATCH -p ultrahigh <br/>
>#SBATCH --mail-type=ALL <br/>
>#SBATCH --mail-user=**your_email@email_adress.com** <br/>

## Addtional SBATCH script options:
>#SBATCH -t #Allows you to set a time limit for a job <br/>
>#SBATCH -test-only #Validates your batch script to see if it would run with the specified mem and cores or other options <br/>
>#SBATCH -x #exclude certain nodes from running the job (if certain past nodes have given problems) <br/>
>#SBATCH --mem-per-cpu= #Set the memory per cpu <br/>
>#SBATCH --kill-on-invalid-dep #Kills the job if a dependency is invalid (often doesnt work but can stop a job from sitting idle <br/>


