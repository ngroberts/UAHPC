# Useful Commands for the Kmkocot Usergrp:

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
