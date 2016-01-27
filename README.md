# RiboSeq

Manage ribo-seq experiment from raw data


## GSE.py

usage: GSE.py -g GSEnumber
option: -w workdir

output:
+GSEnumber.xml
+GSEnumber.sample
  
### What is GSE number:
GSE are GEO database identification. It is associated to a list of sample
used during the study. This number can be found on the GEO NCBI database  
and is often referred on the publication.
  
### What GSE.py do:
From a GSE number, the script download the xml file containing the description of  
the experiment. After parsing the xml file GSEnumber.xml, we extract the name of  
each sample, the species, the GSM and SRX number. These latter numbers are required
to download SRA files associated.
  
### What is the output:
At the end, the user obtains a GSEnumber.xml and a GSEnumber.sample.  
+ GSEnumber.xml is the raw file of the experiment. It contains all the details, protocoles etc...  
+ GSEnumber.sample resume the samples by giving GSM, sample's names, SRR and Genome assmebly. This file is required to download SRA data.
Output files is created in the current directory unless user specified -w option.
  
  
## SRA.py

usage: SRA.py -g GSE.sample
options: -w workdir

output:
+workdir directory if user uses -w
+GSM directory containing SRR.fastq files (if workdir is -w)
+GSE.sra

By default workdir is the current directory

This script needs SRA-Toolkit and EUTILS installed to work.

### What is GSE.sample:  
This file is generated by GSE.py and contains in the first column of each line the GSM number required to  
download SRR files. SRA.py will avoid line starting with # so the users can select only sample they want.  
Users can also provide a file with first column as GSM or SRX number to process.

### What SRA.py do:
Reading the GSM or SRX number, the script will get the SRR datas and download the files. For each GSM or SRX  
a new directory is created and the SRR files are downloaded inside. GSE.sra list the GSM directory and  
the SRR files belonging to.

### What is the output:
For each line non commented -- each GSM or SRX number -- a directory named GSM or SRX number is created and will contains  
the SRR files. A GSE.sra is created where the script is launched and contains the GSM directory and SRR files names.
+GSM diretories with SRR.fastq
+GSE.sra list of GSM directory and SRR files

### Dependencies

#### SRA-Toolkit

http://www.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=software
Add the bin directory path to the $PATH or edit the path in the dependency.config

To avoid to download cache in the ncbi dedicated directory, in bin directory, launch:    
> ./vdb-config -i
+ Unselect Enable Local File Caching  
+ Save  
+ Exit  

#### EUTILS 

http://www.ncbi.nlm.nih.gov/books/NBK179288/
Simply execute the following code where you want install EUTILS
  
  
perl -MNet::FTP -e \
    '$ftp = new Net::FTP("ftp.ncbi.nlm.nih.gov", Passive => 1); $ftp->login;
     $ftp->binary; $ftp->get("/entrez/entrezdirect/edirect.zip");'
unzip -u -q edirect.zip
rm edirect.zip
export PATH=$PATH:$HOME/edirect
./edirect/setup.sh

Add the bin directory path to the $PATH or edit the path in the dependency.config


#### Aspera (not used)
http://downloads.asperasoft.com/connect2///

