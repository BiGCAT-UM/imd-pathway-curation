# WikiPathways Curation Template

Template repository to set up pathway curation for the subset of WikiPathways, as
specified in the `pathways.txt` file. Second, update the `website.txt` file 
(create if needed) to contain the link where the reports (particularly the JSON)
files are published.

Original tests are located here: https://github.com/BiGCAT-UM/WikiPathwaysCurator

## Step 1. Requirements

You need the following programs and experience with a `Makefile`:

* curl
* git
* zip
* java
* javac
* bash

You also need four Java archives in the `libs/` folder that you can download
(and update) with:

```
make install
```

## Step 2: Update the GPML files

First, make sure the `pathways.txt` files is updated to list the pathways you want
to curate.

Then, do:

```
make fetch
```

## Step 3: Make sure to have the BridgeDb ID mapping databases

Create a folder `/tmp/OPSBRIDGEDB` where you create a `config.properties` file.
To download the BridgeDb identifier mapping files, download them from
[here](https://bridgedb.github.io/data/gene_database/)
and save them in the `/path/to/where/the/bridge/files/are` folder, mathching what
you entered in the `config.properties` file above with the `bridgefiles=` parameter.
For the tests in this repository to work, you require the following BridgeDb files:
- GeneProtein mappings for humans (Homo Sapiens)
- Metabolite mappings
- Interaction mappings
Below we provide an example to download the mappings (geneproteins version 105, metabolites 20220707, interactions 20210109), newer version might be available!

```
mkdir /tmp/OPSBRIDGEDB
cd /tmp/OPSBRIDGEDB
wget -c https://zenodo.org/record/6502115/files/Hs_Derby_Ensembl_105.bridge?download=1 -O geneproteinMappings.bridge
wget -c https://figshare.com/ndownloader/files/36197283 -O metaboliteMappings.bridge
wget -c https://ndownloader.figshare.com/files/26003138 -O interactionMappings.bridge
nano config.properties
```

## Step 4: Create the RDF (Turtle)

```
make
```

Make sure that the GPML and the `.rev` files are under version control to track
the history of the pathway.

## Step 5: Run the validation tests

```
make check
```

The `tests.txt` file contains the list of tests that are run. By default it runs
all tests, identified with the `all` methods, from the generic tests and community-specific
tests are commented out (by starting the line with an `#`). By commenting out the
the `all` lines, you can uncomment specific tests in that class.

## Step 6: (optional) Publish everything online

For this, enable GitHub Pages and add the files in `reports/` to the revision
control, including the `index.md` in the root folder.
