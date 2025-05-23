# Script for FID to field names conversion for UKB RAP Table Exporter

To extract data using the UKB RAP [Table Exporter](https://documentation.dnanexus.com/developer/apps/developing-spark-apps/table-exporter-application), "UK Biobank style" data showcase **FIDS** needs to be converted to "UKB-FORMAT" **field names** 

* Pattern for "UK Biobank style" data showcase **FIDS**: ###
* Pattern for "UKB-FORMAT" **field names**:p[FID]_i[instance]_a[array].<br/>
eg UKB-FORMAT **field name** p#### or p####_i#, p####_i# or p####_i##_a#, etc. , where **FID** is ####.<br/>
eg UKB-FORMAT **field name**  p1230_i1_a2 represents UK Biobank style 1230-1.2 or **FID** 1230, instance 1, array 2.
	* Field names are found in the **"data dictionary"**.
  	* [Instructions to get the data dictionary](https://community.ukbiobank.ac.uk/hc/en-gb/articles/15955597101085-Is-there-a-data-dictionary)
	* Data dictionary can be downloaded onto local servers. 

In the following script, 
- input file 'fids_required' is a .txt file with UKB FID list, one per line. Adding instances and arrays is not required.<br/>
  eg of fidlist.txt: <br/>
  25001<br/>
  12673<br/>
  40005<br/>

- input file 'data_dictionary' is a "XXX_XXX.dataset.data_dictionary.csv" file. 
- 'output_file' is a .txt file with list of field names ready for table exporter 

## Main Script grep_ifield_names.sh
```sh
#!/bin/bash
#Script: grep_ifield_names.sh
#Author: NaziaPathan
#Date: May2025

# Input files
fids_required="fidlist.txt" 
data_dictionary="XXX_XXX.dataset.data_dictionary.csv"
output_file="ifield_names_list.txt"

# Initialize the grep pattern
pattern=""

# Read FIDs from the file
while read -r fid; do
    # Append the pattern for this fid
	pattern+="\\bp${fid}(_i[0-9]+|_[0-9])?(_a[0-9])?\\b|"
done < "$fids_required"

# Remove the trailing '|'
pattern=${pattern%|}

# Run grep with the dynamically built pattern
grep -E "$pattern" "$data_dictionary"| cut -d "," -f 2 > "$output_file"
#end of script
```
## Change file permission

```sh
chmod +x grep_ifield_names.sh
```

## Run script

```sh
-c ' ./grep_ifield_names.sh'
```
```
