![SRA Downloader](https://raw.githubusercontent.com/s-andrews/sradownloader/master/logo/sradownloader.png)

SRAdownloader is a program which takes the annotation table from the SRA run selector tool and retrives the raw fastq files from either the ENA or NCBI databases for the selected samples, giving them meaningful names at the same time.  It is designed to make it slightly less painful to get data out of GEO and the SRA.

Installation
------------

### System requirements
This script needs to be run on a unix-like operating system (eg Linux).  It requires the presence of python3 (>= v3.6) on the system it's running on.  Be aware that the data this script downloads can be very large (many tens of gigabytes) so make sure you have enough space to accommodate them.

### Install the SRA toolkit
SRAdownloader will initially try to download data from ENA, but can also download from NCBI.  If you want to be able to download from the NCBI database then sradownloader will use the SRA toolkit to actually interact with the SRA so you'll need to install and configure this first.  If you're happy to just get data from ENA (which may not work for a small subset of studies), then you can skip this step.

Instructions for [downloading](https://github.com/ncbi/sra-tools/wiki/02.-Installing-SRA-Toolkit) and [configuring](https://github.com/ncbi/sra-tools/wiki/03.-Quick-Toolkit-Configuration) the toolkit can be found on their wiki page.

Please note that after installing the toolkit you must run ```vdb-config -i``` to set up the configuration.  You only need to do this once, but you won't be able to download from NCBI until you do.

### Download the latest release of sradownloader
If you have ```git``` installed on your system then the simplest thing to do would be to clone the latest version of sradownloader onto your system.  That way it makes it trivial to update the program in future.  You can clone the software using.

```git clone https://github.com/s-andrews/sradownloader.git```

..and later if you need to update the software you can simply run:

```git pull```

...in the sradownloader directory to update it.

If you don't have git then you can download the [latest release](https://github.com/s-andrews/sradownloader/releases/latest) as a ```tar.gz``` file.  You can then use ```tar -xzf [downloaded file]``` to unpack the program.


Downloading the test data
-------------------------

We ship an example config file called ```SraRunTable.txt``` in the installation bundle and you can use this to check that everything is working as expected.  Assuming you have put both ```sradownloader``` and ```fasterq-dump``` (from sratoolkit) into your PATH then you can test the program using.

```sradownloader --outdir TEST_DOWNLOAD SraRunTable.txt```

..and you should see the program download the example data into a folder called ```TEST_DOWNLOAD```.  The program should finish with a summary which looks like:

```
All done!

RESULTS
-------
SRR5413015:     SUCCEEDED
SRR5413016:     SUCCEEDED
```

..and you should see the following files:

```
$ ls -lh TEST_DOWNLOAD/
total 553M
-rw-rw-r-- 1 andrews andrews 216M Aug 13 16:28 SRR5413015_GSM2563533_Spt3_5min_Saccharomyces_cerevisiae_ChIP-Seq_1.fastq.gz
-rw-rw-r-- 1 andrews andrews 219M Aug 13 16:29 SRR5413015_GSM2563533_Spt3_5min_Saccharomyces_cerevisiae_ChIP-Seq_2.fastq.gz
-rw-rw-r-- 1 andrews andrews 209M Aug 13 16:30 SRR5413016_GSM2563533_Spt3_5min_Saccharomyces_cerevisiae_ChIP-Seq_1.fastq.gz
-rw-rw-r-- 1 andrews andrews 211M Aug 13 16:32 SRR5413016_GSM2563533_Spt3_5min_Saccharomyces_cerevisiae_ChIP-Seq_2.fastq.gz
```

It's possible that the file sizes of the compressed data might vary a bit on your system due to compression differences but they should be around this size.


Downloading your own data
-------------------------

SRAdownloader is designed to work either with files exported from the SRA run selector or with a simple list of SRR accession codes.  You can get to the run selector tool from the GEO page by clicking on the link at the bottom.

![GEO link to SRA run selector](https://raw.githubusercontent.com/s-andrews/sradownloader/master/screenshots/geo_selector.png "GEO link to SRA run selector")

Once in the run selector you can either get the data for all samples for that study, you you can use the checkboxes on the table at the bottom to pick just the samples you want.  You then need to download and save the metadata file.  You can either click on the "Metadata" link in the "Total" section to get everything, or the "Metadata" link in the "Selected" section to get only the samples for which you ticked the boxes.  Take the files which is downloaded and save it onto the machine where you installed ```sradownloader```

![SRA run selector](https://raw.githubusercontent.com/s-andrews/sradownloader/master/screenshots/sra_selector.png "SRA run selector")

Finally you can then run the downloader, passing in the file of metadata as an argument.  You can use the ```--outdir``` option to specify a directory to download into, otherwise the files will come into the current directory.

For simpler uses you can also just use a text file of SRR accession codes (one per line) as input to the tool.  You can also run it with a single SRR accession as the only argument rather than providing a file of accessions.

```
sradownloader SRR5413015
```


Reporting Problems / Bugs
-------------------------

You can report any problems or bugs, or add feature requests in our [issue tracker](https://github.com/s-andrews/sradownloader/issues).  You can also email simon.andrews@babraham.ac.uk.
















