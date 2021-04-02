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
>drwxr-xr-x  4 ngroberts users  135 Mar 29 13:51 2021-03-26-Orthofinder_gene_trees <br/>

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

for example:
>squeue -p long

diplays

>   JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON) <br/>
           1715340      long RI_cupcf  asoyemi  R 2-12:10:02      1 compute-5-12


