# ERGP_Variome
Tools, Scripts, and cmd-line used in the ERGP Variome project


### Abstract
The United Arab Emirates Genome Project (EGP) aims to comprehensively map the genetic landscape of Emirati nationals. This study presents a detailed analysis of a subset comprising 43,608 individuals whose genomes were sequenced using Illumina technology. Our analysis identified 421,605,069 variants, 38% of which are previously unreported genetic variations. Of particular significance is the discovery that among the variants classified as common (43,491,009; 10%) within the studied population, 12% (5,296,683) were determined to be nov-el. While this cohort reflected a diverse ancestral background spanning European, Asian, and African populations, it also exhibited levels of homozygosity, particularly evident in long runs of homozygosity (ROHs). A significant difference (p<0.001) in the total lengths of ROHs was observed between third- and fourth-degree consanguineous marriages.
We observed high-impact variants with higher allele frequencies within our cohort than in global populations. These included rs532444320 in the TMEM59 gene (0.02 vs. 0.001), rs753628430 in the LACTBL1 gene (0.02 vs. <0.00001), and SNP rs775100038 in the SOS1 gene (0.02 vs. 0.00043). This may be due to the high frequency of consanguineous marriages, as regions with ROH harbored high-impact variants within specific genes (SLC22A1, ZAN, NPRL3), representing deleterious alleles in 15-33% of the EGP cohort. Furthermore, our findings elucidated the asso-ciation between consanguinity and chromosomal sexual and autosomal disorders, revealing that 90 out of 141 individuals (63.8%) with chromosomal disorders were consanguineously related, extending up to the fourth degree.
This subset represents the largest Middle Eastern cohort reported to date, providing an unprece-dented opportunity to elucidate the genetic intricacies of the diverse Emirati population. Addi-tionally, our study integrates these findings, facilitating closer investigations into culture-bound patterns of consanguinity. Overall, the EGP represents a significant improvement in the scale of genomics research within the Emirati population, paving the way for comprehensive understand-ing and targeted interventions in genetic health.






## to account for datalake extraction issues ##
# fix compression and replace header with own, working version
ls *.vcf.gz | grep -v / | sed 's/\./ /' | awk '{print "(cat new_header.txt && zcat "$1".vcf.gz | tail -n +41) | bgzip -c > "$1"_comp.vcf.gz"}' > 00_run_recompression.sh

# sort the files
ls *comp.vcf.gz | grep -v / | sed 's/\./ /' | awk '{print "bcftools sort "$1".vcf.gz --output-type z -o "$1"_sorted.vcf.gz"}' > 00_run_sort.sh

# and fix the weird concatenation issue
ls *sorted.vcf.gz | grep -v / | sed 's/\./ /' | awk '{print "bcftools norm -m +any "$1".vcf.gz --output-type z -o "$1"_normed.vcf.gz"}' > 00_run_norm.sh
