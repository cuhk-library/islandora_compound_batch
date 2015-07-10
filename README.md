# Islandora Compound Batch [![Build Status](https://travis-ci.org/discoverygarden/islandora_newspaper_batch.png?branch=7.x)](https://travis-ci.org/discoverygarden/islandora_newspaper_batch)

## Introduction

This module extends the Islandora batch framework so as to provide a Drush and
GUI option to add compound items.

The ingest is a two-step process:

* Preprocessing: The data is scanned and a number of entries are created in the
  Drupal database.  There is minimal processing done at this point, so preprocessing can
  be completed outside of a batch process.
* Ingest: The data is actually processed and ingested. This happens inside of
  a Drupal batch.

## Requirements

This module requires the following modules/libraries:

* [Islandora](https://github.com/islandora/islandora)
* [Tuque](https://github.com/islandora/tuque)


# Installation

Install as usual, see [this](https://drupal.org/documentation/install/modules-themes/modules-7) for further information.

## Configuration

N/A

### Usage

The base ZIP/directory preprocessor can be called as a drush script (see `drush help islandora_compound_batch_preprocess` for additional parameters):

`drush -v --user=admin --uri=http://localhost islandora_compound_batch_preprocess --type=zip --target=/path/to/archive.zip --namespace=mynamespace --parent=namespace:some_compound_object`

This will populate the queue (stored in the Drupal database) with base entries.

Islandora compound objects can contain child objects of any content model, including compound. 
For example, a compound object can have children that are images, PDF files, movies, or other compound objects. 
Given this flexibility, the compound object batch module provides a way for content managers to prepare their content so that it can be loaded into desired structures.
directory tree starting at ‘folder_containing_content’ contains two compound objects, each with several child objects:


* folder_containing_content
    *compound object_1
		* MODS.xml
        * child_1
        	* cmodel.txt
            * MODS.xml
            * OBJ.tif
        * child 2
            * MODS.xml
            * OBJ.mp4
        * child 3
            * MODS.xml
            * child 3a
                * MODS.xml
                * OJB.tif
            *child 3b
                *MODS.xml
                *OBJ.tif
        * child 4
            * MODS.xml
            * OBJ.tif
    * compound object 2
        * MODS.xml
        * child 1
            * MODS.xml
            * OBJ.tiff
        [etc.]

Files are assigned to object datastreams based on their basename.  
Other files, with base names corresponding to datastream IDs, can be included in each page subfolder, such as JP2.jp2, OCR.txt, and TN.jpg. 
If files like these are included, they will be used as the content of their respective datastreams, and the batch process will not recreate the datastreams.

A file named --METADATA--.xml can contain either MODS, DC or MARCXML which is used to fill in the MODS or DC streams (if not provided explicitly). Similarly, --METADATA--.mrc (containing binary MARC) will be transformed to MODS and then possibly to DC, if neither are provided explicitly.

If no MODS is provided at the compound_object level - either directly as MODS.xml, or transformed from either a DC.xml or the "--METADATA--" file discussed above - the directory name will be used as the title.

The queue of preprocessed items can then be processed:

`drush -v --user=admin --uri=http://localhost islandora_batch_ingest`

### Sample compound_object MODS.xml file
Here is a sample MODS file describing a compound_object. Note that it does not contain a `<relatedItem>` element as a direct child of `<mods>`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<mods xmlns="http://www.loc.gov/mods/v3" xmlns:mods="http://www.loc.gov/mods/v3" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xlink="http://www.w3.org/1999/xlink">
    <titleInfo>
      <title>Canadian Jewish Review, June 1, 1928</title>
    </titleInfo>
    <originInfo>
      <place>
        <placeTerm>Toronto, Ontario</placeTerm>
      </place>
      <publisher>Canadian Jewish Review </publisher>
      <dateIssued encoding="iso8601">1928-06-01</dateIssued>
    </originInfo>
    <language>
      <languageTerm>eng</languageTerm>
    </language>
    <subject>
      <topic>Jews, Canadian -- Ontario -- Toronto -- History -- Newspapers</topic>
      <topic>Jews, Canadian -- Quebec -- Montreal -- History -- Newspapers</topic>
      <topic>Jews -- History -- 20th century -- Newspapers</topic>
      <topic>Jews -- Canada -- Periodicals</topic>
      <topic>Canada -- History -- 20th century -- Newspapers</topic>
      <topic>Ontario -- History -- 20th century -- Newspapers</topic>
      <topic>Quebec -- History -- 20th century -- Newspapers</topic>
      <topic>Toronto (Ont.) -- History -- 20th century -- Newspapers</topic>
      <topic>Montreal (Que.) -- History -- 20th century -- Newspapers</topic>
    </subject>
    <identifier>Cjewish-1928-06-01</identifier>
</mods>
```

## Troubleshooting/Issues

Having problems or solved a problem? Check out the Islandora google groups for a solution.

* [Islandora Group](https://groups.google.com/forum/?hl=en&fromgroups#!forum/islandora)
* [Islandora Dev Group](https://groups.google.com/forum/?hl=en&fromgroups#!forum/islandora-dev)

## Maintainers/Sponsors

* [discoverygarden](https://github.com/discoverygarden)

This project has been sponsored by:

* [Simon Fraser University Library](http://www.lib.sfu.ca/)

## Development

If you would like to contribute to this module, please check out our helpful [Documentation for Developers](https://github.com/Islandora/islandora/wiki#wiki-documentation-for-developers) info, as well as our [Developers](http://islandora.ca/developers) section on the Islandora.ca site.

## License

[GPLv3](http://www.gnu.org/licenses/gpl-3.0.txt)