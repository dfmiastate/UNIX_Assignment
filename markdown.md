# UNIX Assignment

## Data Inspection

### Attributes of fang_et_al_genotypes.txt
Code used for inspection:
~~~
$ head -3 fang_et_al_genotypes.txt
$ tail -3 fang_et_al_genotypes.txt
$ wc fang_et_al_genotypes.txt
~~~

By inspecting this file I learned that: 

1. contains a header of Sample_ID, JG_OTU, Group, abph1.20, etc., appears to have amino acid location (example:  PZA00006.13) 
2. need to transpose the columns to become rows
3. There are 2783 lines, 2744038 words, 11051939 bytes

### Attributes of snp_position.txt
Code used for inspection:
~~~
$ head -3 snp_position.txt
$ tail -3 snp_position.txt
$ wc snp_position.txt
~~~

By inspecting this file I learned that:

1. Contains header SNP_ID
2. Chromosome position of all snps, each snp found in fang_et_al_genotypes.txt
3. 984 lines, 13198 words, 82763 bytes

## Data Processing
### Maize Data
Code used:
~~~
$ cut -f 1,3,4 snp_position.txt > snp_position.cut.txt| tail -n +1 snp_position.cut.txt | sort -k1,1 snp_position.cut.txt > snp_position.cut.sorted.txt
~~~
Removes unnecessary data and makes the first three columns SNP_ID, Chromosome, Position and sorts the data
~~~
$ sed 's/Sample_ID/SNP_ID/g' fang_et_al_genotypes.txt > fang_et_al_genotypes.id.txt 
~~~
Make the sample ID tags the same for both files.
~~~
$ grep -E "ZMMIL" fang_et_al_genotypes.id.txt > ZMMIL.txt | grep -E "ZMMLR" fang_et_al_genotypes.id.txt > ZMMLR.txt | grep -E "ZMMMR" fang_et_al_genotypes.id.txt > ZMMMR.txt
$ head -1 fang_et_al_genotypes.txt | cat fang_et_al_genotypes.txt ZMMIL.txt ZMMLR.txt ZMMMR.txt >> ZMMIL.ZMMLR.ZMMMR.txt
~~~
Pull Maize Data from the file and into its own file
~~~
$ awk -f transpose.awk ZMMIL.ZMMLR.ZMMMR.txt > ZMMIL.ZMMLR.ZMMMR.transpose.txt
~~~
Transpose fang_et_al_genotypes.id.txt
~~~
$ tail -n +3 ZMMIL.ZMMLR.ZMMMR.transpose.txt | sort -k1,1 ZMMIL.ZMMLR.ZMMMR.transpose.txt > ZMMIL.ZMMLR.ZMMMR.transposed.sorted.txt
~~~
sorted  fang_et_al_genotypes.id.transposed.txt
~~~
$ join -1 1 -2 1 snp_position.cut.sorted.txt ZMMIL.ZMMLR.ZMMMR.transposed.sorted.txt > maize.joined.txt
~~~
Joined maize data with snp_position data
~~~
$ awk '$2~/1/' maize.joined.txt > Maize.Chr1.txt
$ sort -k3,2n Maize.Chr1.txt > Maize.Chr1.increasing.txt

$ awk '$2~/2/' maize.joined.txt > Maize.Chr2.txt
$ sort -k3,2n Maize.Chr2.txt > Maize.Chr2.increasing.txt

$ awk '$2~/3/' maize.joined.txt > Maize.Chr3.txt
$ sort -k3,2n Maize.Chr3.txt > Maize.Chr3.increasing.txt

$ awk '$2~/4/' maize.joined.txt > Maize.Chr4.txt
$ sort -k3,2n Maize.Chr4.txt > Maize.Chr4.increasing.txt

$ awk '$2~/5/' maize.joined.txt > Maize.Chr5.txt
$ sort -k3,2n Maize.Chr5.txt > Maize.Chr5.increasing.txt

$ awk '$2~/6/' maize.joined.txt > Maize.Chr6.txt
$ sort -k3,2n Maize.Chr6.txt > Maize.Chr6.increasing.txt

$ awk '$2~/7/' maize.joined.txt > Maize.Chr7.txt
$ sort -k3,2n Maize.Chr7.txt > Maize.Chr7.increasing.txt

$ awk '$2~/8/' maize.joined.txt > Maize.Chr8.txt
$ sort -k3,2n Maize.Chr8.txt > Maize.Chr8.increasing.txt

$ awk '$2~/9/' maize.joined.txt > Maize.Chr9.txt
$ sort -k3,2n Maize.Chr9.txt > Maize.Chr9.increasing.txt

$ awk '$2~/10/' maize.joined.txt > Maize.Chr10.txt
$ sort -k3,2n Maize.Chr10.txt > Maize.Chr10.increasing.txt
~~~
Pipe out all chromosome 1, sort by position in increasing integers, and make it into file Maize.Chr1.increasing.txt, do this for each chromosome
~~~
$ awk '$2~/1/' maize.joined.txt > Maize.Chr1.txt
$ sort -k3,2n -r | sed 's/"?"/-/g' Maize.Chr1.txt > Maize.Chr1.decreasing.txt

$ awk '$2~/2/' maize.joined.txt > Maize.Chr2.txt
$ sort -k3,2n -r | sed 's/"?"/-/g' Maize.Chr2.txt > Maize.Chr2.decreasing.txt

$ awk '$2~/3/' maize.joined.txt > Maize.Chr3.txt
$ sort -k3,2n -r | sed 's/"?"/-/g' Maize.Chr3.txt > Maize.Chr3.decreasing.txt

$ awk '$2~/4/' maize.joined.txt > Maize.Chr4.txt
$ sort -k3,2n -r | sed 's/"?"/-/g' Maize.Chr4.txt > Maize.Chr4.decreasing.txt

$ awk '$2~/5/' maize.joined.txt > Maize.Chr5.txt
$ sort -k3,2n -r | sed 's/"?"/-/g' Maize.Chr5.txt > Maize.Chr5.decreasing.txt

$ awk '$2~/6/' maize.joined.txt > Maize.Chr6.txt
$ sort -k3,2n -r | sed 's/"?"/-/g' Maize.Chr6.txt > Maize.Chr6.decreasing.txt

$ awk '$2~/7/' maize.joined.txt > Maize.Chr7.txt
$ sort -k3,2n -r | sed 's/"?"/-/g' Maize.Chr7.txt > Maize.Chr7.decreasing.txt

$ awk '$2~/8/' maize.joined.txt > Maize.Chr8.txt
$ sort -k3,2n -r | sed 's/"?"/-/g' Maize.Chr8.txt > Maize.Chr8.decreasing.txt

$ awk '$2~/9/' maize.joined.txt > Maize.Chr9.txt
$ sort -k3,2n -r | sed 's/"?"/-/g' Maize.Chr9.txt > Maize.Chr9.decreasing.txt

$ awk '$2~/10/' maize.joined.txt > Maize.Chr10.txt
$ sort -k3,2n -r | sed 's/"?"/-/g' Maize.Chr10.txt > Maize.Chr10.decreasing.txt
~~~
Pipe out all chromosome 1, sort by position in decreasing integers, change ?/? to - and make it into file Maize.Chr1.decreasing.txt. Do this for each chromosome
~~~
$ awk '$3~/unknown/' maize.joined.txt > maize.unknown.positions.txt
~~~
Pull all unknown positions and move to file maize.unknown.positions.txt
~~~
$ awk '$3~/multiple/ maize.joined.txt > maize.multiple.positions.txt
~~~
Pull all data with multiple positions and move to maize.multiple.positions.txt.

joined fang_et_al_genotypes with snp_position
### Teosinte Data
~~~
$ grep -E "ZMPBA" fang_et_al_genotypes.id.txt > ZMPBA.txt | grep -E "ZMPIL" fang_et_al_genotypes.id.txt > ZMPIL.txt | grep -E "ZMPJA" fang_et_al_genotypes.id.txt > ZMPJA.txt

$ head -1 fang_et_al_genotypes.txt | cat fang_et_al_genotypes.txt ZMPBA.txt ZMPIL.txt ZMPJA.txt >> ZMPBA.ZMPIL.ZMPJA.txt
~~~
Pull Teosinte Data from the file and into its own file and attach header
~~~
$ awk -f transpose.awk ZMPBA.ZMPIL.ZMPJA.txt > ZMPBA.ZMPIL.ZMPJA.transpose.txt
~~~
Transpose fang_et_al_genotypes.id.txt
~~~
$ tail -n +3 ZMPBA.ZMPIL.ZMPJA.transpose.txt | sort -k1,1 ZMPBA.ZMPIL.ZMPJA.transpose.txt > ZMPBA.ZMPIL.ZMPJA.transposed.sorted.txt
~~~
sorted  fang_et_al_genotypes.id.transposed.txt
~~~
$ join -1 1 -2 1 snp_position.cut.sorted.txt ZMPBA.ZMPIL.ZMPJA.transposed.sorted.txt > Teosinte.joined.txt
~~~
Joined Teosinte data with snp_position data

~~~
$ awk '$3~/unknown/' Teosinte.joined.txt > Teosinte.unknown.positions.txt
~~~
Pull all unknown positions and move to file Teosinte.unknown.positions.txt
~~~
$ awk '$3~/multiple/' Teosinte.joined.txt > Teosinte.multiple.positions.txt
~~~
Pull all data with multiple positions and move to Teosinte.multiple.positions.txt.

~~~
$ awk '$2~/1/' Teosinte.joined.txt > Teosinte.Chr1.txt
$ sort -k3,2n Teosinte.Chr1.txt > Teosinte.Chr1.increasing.txt

$ awk '$2~/2/' Teosinte.joined.txt > Teosinte.Chr2.txt
$ sort -k3,2n Teosinte.Chr2.txt > Teosinte.Chr2.increasing.txt

$ awk '$2~/3/' Teosinte.joined.txt > Teosinte.Chr3.txt
$ sort -k3,2n Teosinte.Chr3.txt > Teosinte.Chr3.increasing.txt

$ awk '$2~/4/' Teosinte.joined.txt > Teosinte.Chr4.txt
$ sort -k3,2n Teosinte.Chr4.txt > Teosinte.Chr4.increasing.txt

$ awk '$2~/5/' Teosinte.joined.txt > Teosinte.Chr5.txt
$ sort -k3,2n Teosinte.Chr5.txt > Teosinte.Chr5.increasing.txt

$ awk '$2~/6/' Teosinte.joined.txt > Teosinte.Chr6.txt
$ sort -k3,2n Teosinte.Chr6.txt > Teosinte.Chr6.increasing.txt

$ awk '$2~/7/' Teosinte.joined.txt > Teosinte.Chr7.txt
$ sort -k3,2n Teosinte.Chr7.txt > Teosinte.Chr7.increasing.txt

$ awk '$2~/8/' Teosinte.joined.txt > Teosinte.Chr8.txt
$ sort -k3,2n Teosinte.Chr8.txt > Teosinte.Chr8.increasing.txt

$ awk '$2~/9/' Teosinte.joined.txt > Teosinte.Chr9.txt
$ sort -k3,2n Teosinte.Chr9.txt > Teosinte.Chr9.increasing.txt

$ awk '$2~/10/' Teosinte.joined.txt > Teosinte.Chr10.txt
$ sort -k3,2n Teosinte.Chr10.txt > Teosinte.Chr10.increasing.txt
~~~
Pipe out all chromosome 1, sort by position in increasing integers, and make it into file Teosinte.Chr1.increasing.txt, do this for each chromosome
~~~
$ awk '$2~/1/' Teosinte.joined.txt > Teosinte.Chr1.txt
$ sort -k3,2n -r | sed 's/"?"/-/g' Teosinte.Chr1.txt > Teosinte.Chr1.decreasing.txt

$ awk '$2~/2/' Teosinte.joined.txt > Teosinte.Chr2.txt
$ sort -k3,2n -r Teosinte.joined.txt | sed 's/"?"/-/g' Teosinte.Chr2.txt > Teosinte.Chr2.decreasing.txt

$ awk '$2~/3/' Teosinte.joined.txt > Teosinte.Chr3.txt
$ sort -k3,2n -r Teosinte.joined.txt | sed 's/"?"/-/g' Teosinte.Chr3.txt > Teosinte.Chr3.decreasing.txt

$ awk '$2~/4/' Teosinte.joined.txt > Teosinte.Chr4.txt
$ sort -k3,2n -r Teosinte.joined.txt | sed 's/"?"/-/g' Teosinte.Chr4.txt > Teosinte.Chr4.decreasing.txt

$ awk '$2~/5/' Teosinte.joined.txt > Teosinte.Chr5.txt
$ sort -k3,2n -r Teosinte.joined.txt | sed 's/"?"/-/g' Teosinte.Chr5.txt > Teosinte.Chr5.decreasing.txt

$ awk '$2~/6/' Teosinte.joined.txt > Teosinte.Chr6.txt
$ sort -k3,2n -r Teosinte.joined.txt | sed 's/"?"/-/g' Teosinte.Chr6.txt > Teosinte.Chr6.decreasing.txt

$ awk '$2~/7/' Teosinte.joined.txt > Teosinte.Chr7.txt
$ sort -k3,2n -r Teosinte.joined.txt | sed 's/"?"/-/g' Teosinte.Chr7.txt > Teosinte.Chr7.decreasing.txt

$ awk '$2~/8/' Teosinte.joined.txt > Teosinte.Chr8.txt
$ sort -k3,2n -r Teosinte.joined.txt | sed 's/"?"/-/g' Teosinte.Chr8.txt > Teosinte.Chr8.decreasing.txt

$ awk '$2~/9/' Teosinte.joined.txt > Teosinte.Chr9.txt
$ sort -k3,2n -r Teosinte.joined.txt | sed 's/"?"/-/g' Teosinte.Chr9.txt > Teosinte.Chr9.decreasing.txt

$ awk '$2~/10/' Teosinte.joined.txt > Teosinte.Chr10.txt
$ sort -k3,2n -r Teosinte.joined.txt | sed 's/"?"/-/g' Teosinte.Chr10.txt > Teosinte.Chr10.decreasing.txt
~~~
Pipe out all chromosome 1, sort by position in decreasing integers, change ?/? to - and make it into file Teosinte.Chr1.decreasing.txt. Do this for each chromosome
