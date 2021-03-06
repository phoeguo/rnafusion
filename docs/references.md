# nfcore/rnafusion: Download references for tools

## 1. Using nextflow helper script

Downloading references manually is a tedious long process. To make the pipeline easier to work with, we provide a script to download all necessary references for fusion detection tools.

> **TL;DR:** Make sure to download the correct references for your need!

```bash
# Example how to download references
# Note: STAR-Fusion has two versions of reference:
#   * NCBI
#   * Ensembl (generated by the script)
nextflow run nf-core/rnafusion/download-references.nf
  -profile <PROFILE>
  --star_fusion
  --fusioncatcher
  --ericscript
  --pizzly
  --fusion_report --cosmic_usr <COSMIC_USER> --cosmic_passwd <COSMIC_PASSWD>
  --outdir <PATH>
```

For additional optional parameters run:

```bash
nextflow run nf-core/rnafusion/download-references.nf --help
```

## 2.  Manual download

### STAR-Fusion (NCBI)

```bash
wget -N https://data.broadinstitute.org/Trinity/CTAT_RESOURCE_LIB/GRCh38_v27_CTAT_lib_Feb092018.plug-n-play.tar.gz -O GRCh38_v27_CTAT_lib_Feb092018.plug-n-play.tar.gz
tar -xvzf GRCh38_v27_CTAT_lib_Feb092018.plug-n-play.tar.gz
```

> Update your custom configuration file to include the directory

```groovy
params {
  star_fusion_ref = "/path/to/GRCh38_v27_CTAT_lib_Feb092018/ctat_genome_lib_build_dir"
}
```

### STAR-Fusion (Custom Ensembl example)

```bash
# download all chromosomes from ensembl
wget ftp://ftp.ensembl.org/pub/release-77/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.chromosome.{1..22}.fa.gz
wget ftp://ftp.ensembl.org/pub/release-77/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.chromosome.{MT,X,Y}.fa.gz
gunzip -c Homo_sapiens.GRCh38.dna.chromosome.* > Homo_sapiens.GRCh38_r77.all.fa

# download fasta file
wget ftp://ftp.ensembl.org/pub/release-77/gtf/homo_sapiens/Homo_sapiens.GRCh38.77.chr.gtf.gz

# download
wget ftp://ftp.ebi.ac.uk/pub/databases/Pfam/current_release/Pfam-A.hmm.gz
gunzip Pfam-A.hmm.gz
hmmpress Pfam-A.hmm

prep_genome_lib.pl
  --genome_fa Homo_sapiens.GRCh38_r77.all.fa
  --gtf Homo_sapiens.GRCh38.77.chr.gtf
  --pfam_db Pfam-A.hmm
  --CPU 10
```

> Update your custom configuration file to include the directory

```groovy
params {
  star_fusion_ref = "/path/to/ctat_genome_lib_build_dir"
}
```

## Fusioncatcher

```bash
wget -N http://sourceforge.net/projects/fusioncatcher/files/data/human_v90.tar.gz.aa
wget -N http://sourceforge.net/projects/fusioncatcher/files/data/human_v90.tar.gz.ab
wget -N http://sourceforge.net/projects/fusioncatcher/files/data/human_v90.tar.gz.ac
wget -N http://sourceforge.net/projects/fusioncatcher/files/data/human_v90.tar.gz.ad
cat human_v90.tar.gz.* | tar xz
```

> Update your custom configuration file to include the directory

```groovy
params {
  fusioncatcher_ref = '/path/to/human_v90'
}
```

## Ericscript

```bash
wget -N https://raw.githubusercontent.com/circulosmeos/gdown.pl/dfd6dc910a38a42d550397bb5c2335be2c4bcf54/gdown.pl \
&& chmod +x gdown.pl \
&& ./gdown.pl "https://drive.google.com/uc?export=download&confirm=qgOc&id=0B9s__vuJPvIiUGt1SnFMZFg4TlE" ericscript_db_homosapiens_ensembl84.tar.bz2 \
&& rm gdown.pl
```

> Update your custom configuration file to include the directory

```groovy
params {
  ericscript_ref = '/path/to/ericscript_db_homosapiens_ensembl84'
}
```

## Pizzly

```bash
# transcriptome
wget -N ftp://ftp.ensembl.org/pub/release-94/fasta/homo_sapiens/cdna/Homo_sapiens.GRCh38.cdna.all.fa.gz \

# annotation
wget -N ftp://ftp.ensembl.org/pub/release-94/gtf/homo_sapiens/Homo_sapiens.GRCh38.94.gtf.gz && gunzip Homo_sapiens.GRCh38.94.gtf.gz
```

> Update your custom configuration file to include the directory

```groovy
params {
  pizzly_fasta = "/path/to/pizzly_ref/Homo_sapiens.GRCh38.cdna.all.fa.gz"
  pizzly_gtf = "/path/to/pizzly_ref/Homo_sapiens.GRCh38.94.gtf"
}
```

## Squid

Requires:

* STAR-Index (is either provided by the user or built by the pipeline)
* GTF file

## AWS iGenomes

```bash
mkdir -p igenomes/Homo_sapiens/NCBI/GRCh38/
aws s3 --no-sign-request --region eu-west-1 sync s3://ngi-igenomes/igenomes/Homo_sapiens/NCBI/GRCh38/ .
```

## FusionInspector

> Uses reference genome from STAR-Fusion (ctat_genome_lib_build_dir)

## fusion-report

```bash
fusion_report download --cosmic_usr <username> --cosmic_passwd <password> /output/databases
```
