# Delete curious filename

0. The Problem:
```
01:26 vagrant:c_poc~> ls
?$????            c_poc_compile      libcrypto.a              memory_management.c  README.md               tests
?$????            file_exts.txt      libssl.a  
```
While writing a C program that writes files to disk, I had a bug that created filenames like this: ``?$????``, and I couldn't delete them with the regular ``rm -f ?$????`` command, or with ``rm -f '?$????'``.

1. I ran the ``ls`` command to list out all the inode numbers of the files:
```
01:28 vagrant:c_poc~> ls -i
655381 ?$????            655380 file_operations.c        655375 memory_management.c     655365 search.c
655367 ?$????            655374 fournights.h             655376 note 
```

2. I used the find command to locate the file in question with the delete flag: 
``find . -inum '655381' -delete && find . -inum '655367' -delete``

3. Problem solved.
