## Step 1 - Preparing your data

This part of the **Michigan Imputation Server workshop** will guide you through all steps required to prepare a 1000 Genomes Phase 3 Imputation.

### General Information
Michigan Imputation Server only accepts VCF files compressed with [bgzip](http://samtools.sourceforge.net/tabix.shtml). To upload valid files to the server, we have fulfill the following requirements:

- Prepare a separated vcf.gz file for each chromosome.
- Use GRCh37 or GRCh38 coordinates. Michigan Imputaton Server will automatically lift your files to the selected reference panel. 

For this workshop we will use remote cloud instances (Amazon AWS) to prepare our data and submit jobs! 

So let's start preparing our data:
    
### Step 1a - Connect to our AWS Instance

Please open a terminal on your local computer (Windows: please use [putty](https://www.putty.org/)) and connect to the instance. You find the personal credentials located on your desk (e.g. user-id: `ashg-000`, user-password: `ashg-000`) 

````sh
ssh <user-id>@<cloud-ip-address>
````

Example:

````sh
ssh ashg000@18.221.123.245
````

After entering your password, you should be now connected to the instance and ready to prepare the data! 

### Step 1b - Prepare your data
Our initial files are located under `/data/plink-data`. You will find three different files (*.bed, bim, *.fam*) in this directory, the so called **PLINK binary format**. 

William Rayner (University of Oxford) provides a tool to validate PLINK binary data: [Pre-imputation Checks](http://www.well.ox.ac.uk/~wrayner/tools/). The script checks the strand information, alleles, positions, Ref/Alt assignments and frequency differences.

Initially, please copy the PLINK data files to your local directory:

````sh
cp /data/plink-data/raw-22-filtered/* ~/.
````

#### Create Frequency File
 The script requires a frequency file, which can be simply created with plink:
 
````sh
plink --freq --bfile ~/raw-22-filtered --out ~/raw-22-filtered
````

#### Run Pre-Imputation Step
To run the script (a) a frequency file (see above) and (b) a 1000 Genomes Phase3 site list must be available. We already prepared the site list and the following command can be executed:

````sh
perl /opt/tools/imputation-preparation/HRC-1000G-check-bim.pl -b ~/raw-22-filtered.bim -f ~/raw-22-filtered.frq -r /opt/tools/imputation-preparation/1000GP_Phase3_combined.legend.gz -g -p EUR
````
#### Run Script
A set of plink commands to update or remove SNPs has now been created. We can now produce a cleaned file by entering the following command:

````sh
sh Run-plink.sh
````

### Step 1c - Compress VCF File
To use with Michigan Imputation Server, the cleaned PLINK file needs converted to VCF:

````
bgzip raw-22-filtered-updated-chr22.vcf
````

### Step 1d - Download VCF
Open FileZilla/WinSCP and download the `raw-22-filtered-updated-chr22.vcf.gz` file to your local file system. Now we are ready to submit out first job!


## Step 2 - Submit Job
Please connect to our [Michigan Imputation Server Ddemo instance](http://imputation-demo-2058360856.us-east-2.elb.amazonaws.com/index.html#!) available for this workshop.

### Register a new account
Since this is a demo instance, please register a new account. *Note:* There is no email validation required, you can simply login by using your selected username and password. 

### Submit Job!
You can now select the local file and upload it to our demo instance of Michigan Imputation Server. Please select the **1000 Genomes Phase 3** reference panel and select **EUR** as the Quality Control population. After several minutes, the imputed data can be downloaded and extracted. Congratulations! 

if you need help submitting a job or download results or the QC report, please have a look at our [Getting Started Guide](https://imputationserver.readthedocs.io/en/latest/getting-started/).

## What's next?
Preparing data for Michigan Imputation Server is quite easy. In the next part you will learn how to make everything reproducibile by using  the new Michigan Imputation Server API!


