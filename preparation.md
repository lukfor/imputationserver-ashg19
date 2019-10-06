### Data preparation

Michigan Imputation Server accepts VCF files compressed with [bgzip](http://samtools.sourceforge.net/tabix.shtml). Please make sure the following requirements are met:

- Create a separate vcf.gz file for each chromosome.
- Variations must be sorted by genomic position.
- GRCh37 or GRCh38 coordinates are required.

!!! note
    Several \*.vcf.gz files can be uploaded at once.
