## Step 1 - Preparing your data

This part of the Michigan Imputation Server workshop will guide you through all steps required to prepare a 1000 Genomes Phase 3 Imputation.

### General Information
Michigan Imputation Server only accepts VCF files compressed with [bgzip](http://samtools.sourceforge.net/tabix.shtml). To upload files to the server, make sure the following requirements are met:

- Prepare a separated vcf.gz file for each chromosome.
- Use GRCh37 or GRCh38 coordinates. Michigan Imputaton Server will automatically lift your files to the selected reference panel. 

For this workshop we will use Amazon AWS Instances to prepare our data and submit jobs 
    
### Step 1a - Connect to our AWS Instance

Please open a terminal on your local computer (Windows: please use [putty](https://www.putty.org/)) and connect to the instance. You find the personal credentials located on your desk (e.g. user-id: ashg-000, user-password: ashg-000) 

````sh
ssh <user-id>@<cloud-ip-address>
````

After entering your password, you should be now connected to the instance and ready to prepare the data. 

### Step 1b - Prepare your data
Our initial data is located at `/data/plink-data`. You will find three different files (.bed, bim, *.fam) in this directory, the so called **PLINK binary format**. 
William Rayner (University of Oxford) provides a tool to validate PLINK binary data: [Pre-imputation Checks](http://www.well.ox.ac.uk/~wrayner/tools/). The script checks strand, alleles, positions, Ref/Alt assignments and frequency differences.

#### Create Frequency File
 The script requires a frequency file, which can be simply created with plink:
 
````sh
plink --freq --bfile /data/plink-data/raw-22-filtered --out ~/raw-22-filtered
````

#### Run Preimputation Step
To run the script (a) a frequency file (see above) and (b) a 1000 Genomes Phase3 site list must be available, which have alrady been downloaded in advance. 

````sh
perl /opt/tools/imputation-preparation/HRC-1000G-check-bim.pl -b ~/raw-22-filtered -f ~/raw-22-filtered.freq -r /opt/tools/imputation-preparation/1000GP_Phase3_combined.legend.gz -g -p EUR
````
#### Run Script
A set of  plink commands to update or remove SNPs has now been created. We can produce a ready-to-use VCF file by entering the following command:

````sh
sh Run-plink.sh
````

### Step 1c - Convert to VCF
The cleaned PLINK file needs finally convereted to VCF:

````
plink --bfile ~/raw-22-filtered-updated-chr22 --recode vcf bgz --out chr22-final
````

### Step 1c - Convert to VCF
Open FileZilla/WinSCP and download the `chr22-final.vcf.gz` file to your local file system.


## Step 2 - Submit Job
Now we are ready to submit our first Michigan Imputation Server job. Therefore, please connect to our [demo instance](http://imputation-demo-2058360856.us-east-2.elb.amazonaws.com/index.html#!).

### Register a new account
Since this is a demo instance, please register a new account. Note: There is no email validation rquired, you can simply login using your selected username and password. 

### Submit Job!
You can now select the file and upload your file to our demo instance of Michigan Imputation Server. Please select "1000 Genomes Phase 3" as the reference panel and select EUR as the population. After several minutes, the imputed data can be downloaded and extracted. Congraulations!

## What's next?
Preparing data for Michigan Imputation Server is quite easy. In the next part you will learn how to use the new Michigan Imputation Server API to submit a job **or** download results.


