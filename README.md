# apache-faidx

## Installation

You *must* use the prefork worker module as htslib is not thread safe. Workers such as MPM will create unpredictable results.

## Apache directives

```
LoadModule faidx_module /path/to/mod_faidx.so

FaidxSet human /faidx/files/Homo_sapiens.GRCh38.dna.toplevel.fa.gz
FaidxSet cat /faidx/files/Felis_catus.Felis_catus_6.2.dna.fa
FaidxMD5 FFFFFFFF human 12

<Location /faidx>
        SetHandler faidx
</Location>

```

## Example curl command

```
curl "http://localhost/faidx?set=human&location=9%3A10000-15000"
curl -H "Content-type: text/x-fasta" "http://localhost/faidx/region/GRCh38?location=9%3A10000-15000"
curl -v -N -H 'Content-type: application/json' -X POST -d '{"location": ["1:1000-2000", "9:300000-305000"], "set": "GRCh38"}' "http://localhost/faidx/region/GRCh38"
curl "http://localhost/faidx/region/GRCh38?location=9%3A10000-15000&location=X%3A10000-11000"
curl -v -N -H "Accept: text/x-fasta" "http://localhost/faidx/md5/FFFFFFFF?location=2000-3000,11000-12000"
curl -v -N -H "Accept: text/x-fasta" "http://localhost/faidx/md5/FFFFFFFF?location=2000-3000,11000-12000:-1&translate=1"
curl -v -N -H "Accept: text/x-fasta" "http://localhost/faidx/md5/FFFFFFFF"

curl "http://localhost/faidx/sets"
curl "http://localhost/faidx/locations/human/"
curl "http://localhost/faidx/?set=GRCh38&location=12:43768112-43768272,43771220-43771365,43772180-43772362,43772912-43773042,43773045-43773072,43773965-43773971" (IRAK4, ENST00000448290.6)
curl "http://localhost/faidx/?set=GRCh38&location=12:43768112-43768272,43771220-43771365,43772180-43772362,43772912-43773042,43773045-43773072,43773965-43773971&translate=1" (IRAK4, ENSP00000390651.3)
curl -H "Accept: text/x-fasta" http://localhost/faidx/md5/FFFFFFFF
```

## TODO

* Missing fai files referred to in the config aren't properly removed from the fai linked list, needs fixing
* Chunked return type or content-length depending on return size, not implemented yet

[![Build Status](https://travis-ci.org/lairdm/apache-faidx.svg?branch=master)](https://travis-ci.org/lairdm/apache-faidx) [![Coverage Status](https://coveralls.io/repos/github/lairdm/apache-faidx/badge.svg?branch=master)](https://coveralls.io/github/lairdm/apache-faidx?branch=master)