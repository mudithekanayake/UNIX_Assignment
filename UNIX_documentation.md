#BCB546 - UNIX Assignment - Mudith Ekanayake

##Data Inspection

###Attributes of `fang_et_al_genotypes`

To get the file sizes
```
ls -lh
```
To see the first line of the file for understanding the formatting of the file
```
head -n 1 fang_et_al_genotypes.txt
```
End of the files were inspected by tail.
```
tail -n 3 fang_et_al_genotypes.txt
```
less was used to view the file.
```
less fang_et_al_genotypes.txt
```
To get only the number of lines in the file
```
wc -l fang_et_al_genotypes.txt
```
To get the number of lines as well as number of words and number of bytes in the file
```
wc fang_et_al_genotypes.txt
```
To get the number of columns
```
awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt
```





By inspecting this file I learned that:

* File size: 11M
* Number of lines: 2783
* Number of columns: 986

###Attributes of `snp_position.txt`

```
ls -lh
head -n 1 snp_position.txt
tail -n 3 snp_position.txt
less snp_position.txt
wc -l snp_position.txt
wc snp_position.txt
awk -F "\t" '{print NF; exit}' snp_position.txt
```

By inspecting this file I learned that:

* File size: 81K
* Number of lines: 984
* Number of columns: 15

##Data Processing

###Maize Data

```
here is my snippet of code used for data processing
```

Here is my brief description of what this code does


###Teosinte Data

```
here is my snippet of code used for data processing
```

Here is my brief description of what this code does
