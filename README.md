# Script to get field_names for FIDs of interest using the data dictionary in the UKB RAP

To extract data using table exporter, field_names corresponding to FIDS are required (eg.-ifield_names="p1200_i0"). 

Field names are found in the "data dictionary".
In "data dictionary", the field_names patter is p#### or p####_i0 p####_i2, etc. , where #### are the FIDS

Instructions to get the data dictionary is here: 
https://community.ukbiobank.ac.uk/hc/en-gb/articles/15955597101085-Is-there-a-data-dictionary

Data dictionary can be downloaded onto local servers. 

In the following script, 
- 'fids_required' is a UKB FID list, one per line
- 'output_file' is a list of ifield_names that include incidence 0-9. Change the "pattern" if FIDs include more than 9 incidence. 

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
	pattern+="\\bp${fid}(_i[0-9]+|_[0-9]+)?\\b|"
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
