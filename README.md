# SRA downloader
SRAdownloader is a script which takes the annotation table from the SRA run selector tool and uses the sratoolkit to download the fastq files for the selected samples, giving them meaningful names at the same time.  It is designed to make it slightly less painful to get data out of GEO and the SRA.

Installation
------------

### System requirements
This script needs to be run on a unix-like operating system (eg Linux).  It requires the presence of python3 and gzip on the system it's running on.  Be aware that the data this script downloads can be very large (many tens of gigabytes) so make sure you have enough space to accommodate them.

### Install the SRA toolkit
This script uses the SRA toolkit to actually interact with the SRA so you'll need to install and configure this first.

Instructions for installing and configuring the toolkit can be found on their [Github Pages Site](https://ncbi.github.io/sra-tools/install_config.html). 

Please note the last part of the installation instructions which say that after installing the toolkit you must run ```vdb-config -i``` to set up the configuration for the toolkit.  You only need to do this once, but nothing will work until you do.

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

..and you should see the program download the example data into a folder called ```TEST_DOWNLOAD```.  If the download immediately fails with an error which looks like:

```
Running: fasterq-dump --split-files --threads 1 --outfile SRR5413015_Spt3_ChEC-seq --progress --outdir GEO_Download SRR5413015
unrecognized option: '--progress'
Traceback (most recent call last):
  File "./sradownloader", line 156, in <module>
    main()
  File "./sradownloader", line 153, in main
    download_sample(sample,options)
  File "./sradownloader", line 45, in download_sample
    subprocess.run(command_options, check=True)
  File "/bi/apps/python/3.7.3/lib/python3.7/subprocess.py", line 487, in run
    output=stdout, stderr=stderr)
subprocess.CalledProcessError: Command '['fasterq-dump', '--split-files', '--threads', '1', '--outfile', 'SRR5413015_Spt3_ChEC-seq', '--progress', '--outdir', 'GEO_Download', 'SRR5413015']' returned non-zero exit status 64.
```

Then you're using a version of SRAtoolkit where there is a spelling error in one of the program options (they spelled 'progress' as 'progres'), so you will need to run:

```sradownloader --outdir TEST_DOWNLOAD --cantspell SraRunTable.txt```

Downloading your own data
-------------------------

SRAdownloader is designed to work with files exported from the SRA run selector.  You can get to this tool from the GEO page by clicking on the link at the bottom.

![GEO link to SRA run selector](https://raw.githubusercontent.com/s-andrews/sradownloader/master/screenshots/geo_selector.png "GEO link to SRA run selector")

Once in the run selector you can either get the data for all samples for that study, you you can use the checkboxes on the table at the bottom to pick just the samples you want.  You then need to download and save the metadata file.  You can either click on the "Metadata" link in the "Total" section to get everything, or the "Metadata" link in the "Selected" section to get only the samples for which you ticked the boxes.  Take the files which is downloaded and save it onto the machine where you installed ```sradownloader```

Finally you can then run the downloader, passing in the file of metadata as an argument.  You can use the ```--outdir``` option to specify a directory to download into, otherwise the files will come into the current directory.

Note that if you get an error as soon as the first download starts then you're probably using a version of SRAtoolkit where there is a spelling error in one of the program options (they spelled 'progress' as 'progres'), so you will need to add the ```--cantspell``` option to the command to work round this.

Reporting Problems / Bugs
-------------------------

You can report any problems or bugs, or add feature requests in our [https://github.com/s-andrews/sradownloader/issues](issue tracker).  You can also email simon.andrews@babraham.ac.uk.
















