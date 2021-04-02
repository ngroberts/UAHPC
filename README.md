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

## UAHPC Partitions:

Useful commands if you know what you are doing:
>scontrol show partition <br/>
>sacctmgr show qos <br/>
>sinfo -N -l <br/>

Note that sinfo will show you the wrong memory for the partitions due to the nature of how nodes are distributed on UAHPC.

## Here is what you need to know:

| Partition Name: | Nodes: | Cores: | Memory:                                       |
|-----------------|--------|--------|-----------------------------------------------|
| long            | 8      | 192    | 768GB                                         |
| threaded        | 2      | 80     | 5376GB (Technically infinite)                 |
| loko            | 1      | 16     | 1TB                                           |
| highmem         | 2      | 32     | 768 (Technically much more, depending on use) |
| ultrahigh       | 3      | 142    | 3.072TB                                       |

However it is a bit more nuanced: 
If you use
>sacctmgr show qos 

You will see that THREADED has a MAXTRES of 16 which means that per job there are only 16 available cores however you can run up to four jobs in parrallel.
16 Cores per Job. 

As well you will see using 
