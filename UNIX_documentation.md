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


```
head -n 1 snp_position.txt > snp_sorted.txt | tail -n+2 snp_position.txt | sort -k1,1 >> snp_sorted.txt
```
A file called `snp_sorted.txt` with header was created using `head` and appended sorted data into `snp_sorted.txt` file using `tail`and `sort`.


```
awk 'BEGIN {FS="\t"; OFS="\t"} {print $1, $3, $4, $2, $5,$6,$7,$8,$9,$10,$11,$12,$13,$14,$15}' snp_sorted.txt > snp_sorted_col.txt
```
Columns in the `snp_sorted.txt` file was rearranged using `awk` and `snp_sorted_col.txt` file was created.


##Data Processing for Maize


```
head -n 1 fang_et_al_genotypes.txt > maize_genotype.txt | grep -E "(ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt >> maize_genotype.txt
```
`maize_genotype.txt` file with header was created using `head` and selected and separated maize data and appended data into `maize_genotype.txt` file.


```
awk -f transpose.awk maize_genotype.txt > maize_genotype_transposed.txt
```
maize data were transposed using the given `transpose.awk` file using `awk` and `maize_genotype_transposed.txt` file was created.


```
head -n 1 maize_genotype_transposed.txt > maize_sorted.txt | tail -n+4 maize_genotype_transposed.txt | sort -k1,1 >> maize_sorted.txt
```
`maize_sorted.txt` file was created with header and appended necessary data into the `maize_sorted.txt` after sorting using the `sort` command.


```
join -1 1 -2 1 --header snp_sorted_col.txt maize_sorted.txt > maize_joined.txt | sed -i "s/ /\t/g" maize_joined.txt
```
Two sorted files `snp_sorted_col.txt` and `maize_sorted.txt` were joined using `join` command and `maize_joined.txt` file was created. Some changes were done employing the `sed` command they were written to the original `maize_joined.txt` file using `-i` flag.



###10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by the symbol '?'


```
head -n 1 maize_joined.txt | awk 'BEGIN {FS="\t"; OFS="\t"}{ for (i=1; i <= 10; i++) print $0 > "maize_chr_asc_"i".txt"}' | tail -n +2 maize_joined.txt | sort -k2,2 -k3,3n | awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 >= 1 && $2 <= 10) {print >> "maize_chr_asc_"$2".txt"}}'
```
For each chromosome 10 files were created numbering from 1 to 10 and data for SNPs ordered based on increasing position values and missing data encoded by the symbol `?` were appended into the created files.


###10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by the symbol '-'


```
head -n 1 maize_joined.txt | awk 'BEGIN {FS="\t"; OFS="\t"}{ for (i=1; i <= 10; i++) print $0 > "maize_chr_desc_"i".txt"}' | tail -n +2 maize_joined.txt | sort -k2,2 -k3,3rn | sed 's/?/-/g' | awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 >= 1 && $2 <= 10) {print >> "maize_chr_desc_"$2".txt"}}'
```
For each chromosome 10 files were created numbering from 1 to 10 and data for SNPs ordered based on decreasing position values and missing data encoded by the symbol `-` were appended into the created files.


###All SNPs with unknown positions in the genome


```
head -n 1 maize_joined.txt > maize_chr_unknown_positions.txt | tail -n +2 maize_joined.txt | awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 == "unknown") {print >> "maize_chr_unknown_positions.txt"}}'
```
A file called `maize_chr_unknown_positions.txt` was created with headers from `maize_joined.txt` and appended all the data with unknown positions into the file.


###All SNPs with multiple positions in the genome


```
head -n 1 maize_joined.txt > maize_chr_multi_positions.txt | tail -n +2 maize_joined.txt | awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 == "multiple") {print >> "maize_chr_multi_positions.txt"}}'
```
A file called `maize_chr_multi_positions.txt` was created with headers from `maize_joined.txt` and appended all the data with multiple positions into the file.



##Data Processing for teosinte


```
head -n 1 fang_et_al_genotypes.txt > teosinte_genotype.txt | grep -E "(ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt >> teosinte_genotype.txt
```
`teosinte_genotype.txt` file with header was created using `head` and selected and separated maize data and appended data into `teosinte_genotype.txt` file.


```
awk -f transpose.awk teosinte_genotype.txt > teosinte_genotype_transposed.txt
```
teosinte data were transposed using the given `transpose.awk` file using `awk` and `teosinte_genotype_transposed.txt` file was created.


```
head -n 1 teosinte_genotype_transposed.txt > teosinte_sorted.txt | tail -n+4 teosinte_genotype_transposed.txt | sort -k1,1 >> teosinte_sorted.txt
```
`teosinte_sorted.txt` file was created with header and appended necessary data into the `teosinte_sorted.txt` after sorting using the `sort` command.


```
join -1 1 -2 1 --header snp_sorted_col.txt teosinte_sorted.txt > teosinte_joined.txt | sed -i "s/ /\t/g" teosinte_joined.txt
```
Two sorted files `snp_sorted_col.txt` and `teosinte_sorted.txt` were joined using `join` command and `teosinte_joined.txt` file was created. Some changes were done employing the `sed` command they were written to the original `teosinte_joined.txt` file using `-i` flag.



###10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol '?'


```
head -n 1 teosinte_joined.txt | awk 'BEGIN {FS="\t"; OFS="\t"}{ for (i=1; i <= 10; i++) print $0 > "teosinte_chr_asc_"i".txt"}' | tail -n +2 teosinte_joined.txt | sort -k2,2 -k3,3n | awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 >= 1 && $2 <= 10) {print >> "teosinte_chr_asc_"$2".txt"}}'
```
For each chromosome 10 files were created numbering from 1 to 10 and data for SNPs ordered based on increasing position values and missing data encoded by the symbol `?` were appended into the created files.


###10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by this symbol '-'


```
head -n 1 teosinte_joined.txt | awk 'BEGIN {FS="\t"; OFS="\t"}{ for (i=1; i <= 10; i++) print $0 > "teosinte_chr_desc_"i".txt"}' | tail -n +2 teosinte_joined.txt | sort -k2,2 -k3,3rn | sed 's/?/-/g' | awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 >= 1 && $2 <= 10) {print >> "teosinte_chr_desc_"$2".txt"}}'
```
For each chromosome 10 files were created numbering from 1 to 10 and data for SNPs ordered based on decreasing position values and missing data encoded by the symbol `-` were appended into the created files.


###All SNPs with unknown positions in the genome


```
head -n 1 teosinte_joined.txt > teosinte_chr_unknown_positions.txt | tail -n +2 teosinte_joined.txt | awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 == "unknown") {print >> "teosinte_chr_unknown_positions.txt"}}'
```
A file called `teosinte_chr_unknown_positions.txt` was created with headers from `teosinte_joined.txt` and appended all the data with unknown positions into the file.


###All SNPs with multiple positions in the genome


```
head -n 1 teosinte_joined.txt > teosinte_chr_multi_positions.txt | tail -n +2 teosinte_joined.txt | awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 == "multiple") {print >> "teosinte_chr_multi_positions.txt"}}'
```
A file called `teosinte_chr_multi_positions.txt` was created with headers from `teosinte_joined.txt` and appended all the data with multiple positions into the file.
