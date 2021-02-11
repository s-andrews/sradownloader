#!/usr/bin/env python3
import urllib.request
import zlib
from ftplib import FTP
import sys

def main():
    # SRR12478073 is a good test
    accession,skip,sample = sys.argv[1:]
    url = get_url(accession)

    sample_url(url,int(skip),int(sample))


def sample_url(url,skip,sample):
    url_sections = url.split("/")
    server = url_sections[0]
    file = url_sections[-1]
    folder = "/".join(url_sections[1:-1])

    decompressor = collect_gzip_data(skip,sample)

    #print(f"URL is {url} server={server} folder={folder} file={file}")

    #print(f"Connecting to {server}")
    ftp = FTP(server)
    #print(f"Logging in")
    ftp.login()
    #print(f"Moving to {folder}")
    ftp.cwd(folder)
    ftp.retrbinary("RETR "+file,decompressor, blocksize=124)
    
    ftp.quit()


def collect_gzip_data(skip,collect):
    decompressor = zlib.decompressobj(zlib.MAX_WBITS|16)
    newline_count = 0

    def accept_data(chunk):
        nonlocal decompressor
        nonlocal newline_count

        new_data = decompressor.decompress(chunk).decode('utf-8')

        lines = new_data.split("\n")

        for i in range(len(lines)):
            if newline_count >= 4*skip:
                print(lines[i],end='')
                if i < len(lines)-1:
                    print("\n",end='')

            if i < len(lines)-1:
                newline_count += 1
            
            if newline_count >= 4*(skip+collect):
                sys.exit()

    return(accept_data)

def get_url(accession):
    # Get the FTP locations for the sample
    ena_rest_url = f"https://www.ebi.ac.uk/ena/portal/api/filereport?accession={accession}&result=read_run&fields=run_accession,fastq_ftp"


    with urllib.request.urlopen(ena_rest_url) as response:
        if response.getcode() != 200:
            raise IOError(f"[ENA] Got error code {response.getcode()} from ENA REST for accession {sample['accession']}")

        url = ""

        found_accession_line = False

        for line in response:
            line = line.decode("UTF-8").strip()
            #print(line)
            if line.startswith("SRR"):
                found_accession_line = True
                sections = line.split("\t")

                if len(sections) != 2:
                    raise IOError(f"[ENA] No ENA fastq files found for accession {sample['accession']}")

                url = sections[1].split(";")[0] # Just take the first read if there are multiple available
                return(url)

        raise IOError(f"[ENA] Found no accession in response from ENA REST for accession {sample['accession']}")



if __name__ == "__main__":
    main()