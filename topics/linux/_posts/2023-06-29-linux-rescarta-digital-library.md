---
layout: post
title: ResCarta Digital Collection Toolkit on Linux
author: Ralf Eichinger
toc: true
---

This post describes how to install and use the ResCarta Toolkit to create and maintain a digital collection (of e.g. digitized books).

# Specification

* Homepage: <https://www.rescarta.org/>
* Current Version: 7.0.5
* Download: <https://www.rescarta.org/index.php/en/software/the-toolkit/download>
* Sourcecode: <https://software.rescarta.org/gitweb/?p=rc-tools.git;a=tree>, <git://software.rescarta.org/rc-tools.git>
* Documentation: <https://sourceforge.net/projects/rescarta/files/Documentation/>

# Installation

We will describe installation of ResCarta on Ubuntu Linux 22.04.2 LTS.

ResCarta is tested with Java 11. As we have installed Java 11 AND 17 already (see below `java -version` output), we have to set JAVA_HOME to Java 11 for installation and (later on) for starting tools.

Install Java, ghostscript, ffmpeg and vlc:

```sh
$ sudo apt update
$ sudo apt install openjdk-11-jdk

...
openjdk-11-jdk ist schon die neueste Version (11.0.19+7~us1-0ubuntu1~22.04.1).
0 aktualisiert, 0 neu installiert, 0 zu entfernen und 5 nicht aktualisiert.

$ java -version

openjdk version "17.0.7" 2023-04-18
OpenJDK Runtime Environment (build 17.0.7+7-Ubuntu-0ubuntu122.04.2)
OpenJDK 64-Bit Server VM (build 17.0.7+7-Ubuntu-0ubuntu122.04.2, mixed mode, sharing)

$ sudo apt install ghostscript

...
ghostscript ist schon die neueste Version (9.55.0~dfsg1-0ubuntu5.2).
ghostscript wurde als manuell installiert festgelegt.
0 aktualisiert, 0 neu installiert, 0 zu entfernen und 5 nicht aktualisiert.

$ sudo apt install ffmpeg

...
ffmpeg ist schon die neueste Version (7:4.4.2-0ubuntu0.22.04.1).
ffmpeg wurde als manuell installiert festgelegt.
0 aktualisiert, 0 neu installiert, 0 zu entfernen und 5 nicht aktualisiert.

$ sudo apt install vlc

...
vlc (3.0.16-1build7) wird eingerichtet ...
```

Download ResCarta Toolkit 7.0.5 from <https://sourceforge.net/projects/rescarta/files/ResCarta%20Tools/InstallRcTools_7/rc-tools-install-7.0.5.jar/download> into `~/Download`.

Start installer:

```
$ export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
$ cd ~/Download
$ $JAVA_HOME/bin/java -jar rc-tools-install-7.0.5.jar
Logging initialized at level 'INFO'
Commandline arguments: 
Detected platform: ubuntu_linux,version=5.15.0-76-generic,arch=x64,symbolicName=null,javaVersion=11.0.19
Resource customicons.xml not defined. No custom icons available
Cannot find named resource: 'packsLang.xml' AND 'packsLang.xml_eng'
```

Follow the prompts, accept the license, select a location where you have write permissions and
proceed until the last DONE button appears:

## Step 1: Welcome

![ResCarta installer step 1](/assets/topics/linux/rescarta/rescarta-installer-01.jpg)

Read welcome text and continue with "Next".

## Step 2: Information

![ResCarta installer step 2](/assets/topics/linux/rescarta/rescarta-installer-02.jpg)

Read information text. There are also YouTube-Videos mentioned under <https://www.youtube.com/watch?v=bP1NPiwIOLc>. Continue with "Next".

## Step 3: Licensing Agreements

![ResCarta installer step 3](/assets/topics/linux/rescarta/rescarta-installer-03.jpg)

Read license text. If you agree, click radio button "I accept the terms of this license agreement.". Continue with "Next".

## Step 4: Target Path

![ResCarta installer step 4](/assets/topics/linux/rescarta/rescarta-installer-04.jpg)

Select the installation path where you have write permissions, e.g. in your home directory (default is `~/RcTools-7.0.5`). Continue with "Next".

Confirm creation of new directory in upcoming dialog with "OK":

![ResCarta installer step 4 confirmation](/assets/topics/linux/rescarta/rescarta-installer-04a.jpg)

## Step 5: Setup Shortcuts

![ResCarta installer step 5](/assets/topics/linux/rescarta/rescarta-installer-05.jpg)

Confirm creation of a program group "ResCarta Tools 7.0.5" and shortcuts in the start menu for current user. Continue with "Next".

## Step 6: Select Installation Packages

![ResCarta installer step 6](/assets/topics/linux/rescarta/rescarta-installer-06.jpg)

Confirm installation of all packages (436.31 MB disk space needed) by clicking "Next".

## Step 7: Summary Configuration Data

![ResCarta installer step 7](/assets/topics/linux/rescarta/rescarta-installer-07.jpg)

A summary of installation settings is presented.

```
Installation Path
  /home/ralf/RcTools-7.0.5
Chosen Installation Packs
  Base
  Metadata Creation Tool 7.0.5
  Data Conversion Tool 7.0.5
  Textual Metadata Editor 7.0.5
  Audio Transcription Editor 7.0.5
  Collections Manager 7.0.5
  Indexer 7.0.5
  Checksum Verification Tool 7.0.5
  Kaltura Video Upload Tool 7.0.5
  Data Format Update Tool 7.0.5
  ResCarta-Web 7.0.5
  Documentation
  ResCarta Tools Post-Installation
```

Proceed installation by clicking "Next".

## Step 8: Installation

![ResCarta installer step 8](/assets/topics/linux/rescarta/rescarta-installer-08.jpg)

The installation progress is shown and you COULD exit installation by clicking "Quit".
As there is one step ahead we continue with "Next".

## Step 9: Installation Finished

![ResCarta installer step 9](/assets/topics/linux/rescarta/rescarta-installer-09.jpg)

We get final information that installation is finished and that there is an uninstaller application under `/home/ralf/RcTools-7.0.5/Uninstaller`.

Finish installation by clicking "Done".

**IMPORTANT**: Before starting usage of ResCarta, read carefully section ["Troubleshooting" below](#troubleshooting) and prepare installed system for first use.

# Usage

In this section we will describe shortest ways to add your digital objects to a ResCarta archive and bring your ResCarta archive online.

Note: As on our system two versions of Java are installed, we will call the ResCarta tools from commandline, giving each time the JAVA_HOME environment variable pointing to installed Java 11.

## Curating a collection of scanned books

In this use case we have a directory (e.g. "ALX") at hand under which (multi sublevels are detected) there are subdirectories, one for each book containing the scan images for each page of the book. The images are named in a way that their correct sequence is represented by alphanumeric sorting.
The images of a book are considered as a single "Digital Object".

Example directory:

```
$ ls ALX
 au_clair_de_la_lune
 bienenzucht
 die_uhr
'Die Wunder der Natur'
'Große Illustrierte Hauslegende'
 Kürschners_Handlexikon-600dpi
 landwirtschaft-bienenzucht
 maler-und-zimmermann
 mechanik1
 mechanik2
 Universum_des_Himmels_der_Erde_und_des_Menschen-1
 Universum_des_Himmels_der_Erde_und_des_Menschen-2
```

Example Book:

```
$ ls ALX/Kürschners_Handlexikon-600dpi/
alx-0001.jpg
...
alx-0935.jpg
```

### Step 1: ResCarta Metadata Creation Tool

The "ResCarta Metadata Creation Tool" is used to create object metadata (information about your image
files stored along with them in a digital format). Any new, digitized object needs object metadata in order to become part of your ResCarta archive.

Start the "ResCarta Metadata Creation Tool":

```
$ export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
$ cd ~/RcTools-7.0.5/bin
$ ./1_ResCartaMetadataCreationTool.sh
```

![ResCarta Metadata Creation Tool - start](/assets/topics/linux/rescarta/rescarta-metadata-creation-tool-01.jpg)

#### Open Data Directory

Open the top level directory containing all book directories, e.g. "ALX":

* Menu: File - Open Data Directory
* Dialog "Select Data Structure Type": "Object per directory"

![ResCarta Metadata Creation Tool - Open Data Directory](/assets/topics/linux/rescarta/rescarta-metadata-creation-tool-02.jpg)

* Dialog "Open Data Directory (Object Per Directory)": select top level directory, e.g. ".../ALX"

![ResCarta Metadata Creation Tool - Open Data Directory](/assets/topics/linux/rescarta/rescarta-metadata-creation-tool-03.jpg)

* Dialog "Image Data Directory Opened": information about amount of found objects in all subdirectories, e.g. "There were 17 objects found in .../ALX."

![ResCarta Metadata Creation Tool - Open Data Directory](/assets/topics/linux/rescarta/rescarta-metadata-creation-tool-04.jpg)

Directories for which no metadata has been created are indicated with an folder icon. As we did not create metadata, yet, all objects are indicated with the folder icon. Directories for which metadata exists will be indicated with an icon representing the object type, such as a book icon.

#### Create metadata for object

Select an object's directory (e.g. "./Kürschners_Handlexikon-600dpi") and click "Create metadata" on the right.

##### Select Source Institution

![ResCarta Metadata Creation Tool - Select Source Institution](/assets/topics/linux/rescarta/rescarta-metadata-creation-tool-05.jpg)

There does no source institution exist, yet. So we create our first institution the "alexana Digital Library" by clicking on "Add":

![ResCarta Metadata Creation Tool - Add Source Institution](/assets/topics/linux/rescarta/rescarta-metadata-creation-tool-06.jpg)

The source institution identifier (field "Id") **MUST** be eight characters (letters and/or numbers) in
length and be unique for each institution to avoid collisions when sharing data with other institutions.
A source institution identifier is required for each object.
Good practice is to use an already existing unique identifier your institution holds. e.g.

* "MARC Code List for Organizations" at <https://www.loc.gov/marc/organizations/>
* "ISIL" (for Germany) at <https://sigel.staatsbibliothek-berlin.de/en/vergabe/isil>

It is recommended to use that code, padded to the right with zeroes for a total length of eight characters.

Example: MARC code for the New York Public library is NN - recommended source institution identifier
for the New York Public Library is NN000000

We chose to use "ALX00000". Click "OK".

![ResCarta Metadata Creation Tool - Select Source Institution](/assets/topics/linux/rescarta/rescarta-metadata-creation-tool-07.jpg)

Choose "alexana Digital Library" and click "OK".

##### Select Aggregator and Root Id

After choosing the source institution identifier (here "ALX00000"), we have to give an aggregator and a root id.

Collectively, the source institution identifier (Id), aggregator (Aggregator), and root
identifier (Root id) specified comprise the object identifier, which must - in combination - be unique for each object, so that it can specify the location of that object. This object identifier/directory structure "institution identifier/aggregator/root identifier" is referenced throughout the ResCarta Toolkit.

The aggregator is just the name of a target subdirectory under the institutional directory for the reason of not having all book directories (with directory name is the root id, containing all the images of one book) on one level. Operating systems could have a problem having thousands of directories on one level. The aggregator (= subdirectory name) you can choose by your liking.

What IS required: aggregator and root identifier must each be eight characters (letters and/or numbers) in length.

The tool's default is a number padded to eight characters (e.g. "00000001"). You can use the current date as a simple aggregator (e.g. 20230701) or the digitization date (e.g. 20221105) or something else. Just use something that really will contain more than one book (e.g. if you only scan one book a day and use the digitization date), because otherwise you will end up again with too many aggregator directories decreasing performance.

We plan to have 1000 books in one directory before starting a new aggregator. So we choose "00000001" (the default recommendation) for the first aggregator directory (books 1 to 1000) and "00000002" for the second (books 1001 to 2000) and so on.

![ResCarta Metadata Creation Tool - Select Aggregator and Rood Id](/assets/topics/linux/rescarta/rescarta-metadata-creation-tool-08.jpg)

##### Select Object Type

Next dialog is the "Select Object type" dialog. You have to select the type of the object ("Monograph", "Serial Monograph", "Serial", "Newspaper", "Photo"). In our case it is a book (not part of serial), so we choose "Monograph" and click "OK".

![ResCarta Metadata Creation Tool - Select Object Type](/assets/topics/linux/rescarta/rescarta-metadata-creation-tool-09.jpg)

#### Create monograph metadata

After selecting "Monograph", the monograph create metadata dialog appears.

![ResCarta Metadata Creation Tool - Monograph create metadata](/assets/topics/linux/rescarta/rescarta-metadata-creation-tool-11.jpg)

As we did not enter metadata (title), yet, the object's Id is shown in the list on the left: "ALX00000/00000001/00000001".

We now have to enter metadata. Required fields are:

* Title
* Volume (if you add a serial object)

For our monograph we enter "Title": "Kürschners Handlexikon für alle Wissensgebiete" and save this by clicking the floppy disc labeled button "Save Modifications" at the bottom. 

We get an error message "Invalid width entered. The width must a number representing width in inches.".
The "Size (inches)" fields of the object were automatically filled (see screenshot).

![ResCarta Metadata Creation Tool - Select Aggregator and Rood Id](/assets/topics/linux/rescarta/rescarta-metadata-creation-tool-12.jpg)

As the values seem to be invalid we empty the size fields and save again.

Now the title has been saved (along with some prefilled fields) and we see the title appear in the list of objects instead the Id of the object.

![ResCarta Metadata Creation Tool - Title saved](/assets/topics/linux/rescarta/rescarta-metadata-creation-tool-13.jpg)

For further editing of metadata we click the button "Modify Metadata" (card with pencil icon) on the left bottom side under the list of objects. Fill as much fields as possible.

Editable metadata are:

* Title
* Volume (for serial monograph or serial documents only)
* Abstract
* Alternative titles
* Names (Author, Owner, etc.): for possible roles of a named person, corporate body, conference or family (or "n/a") see [Appendix "Available Roles" below](#availableRoles)
* Type of resource: for selectable values (MODS typeOfResource) see [Appendix "Types of resource" below](#typeOfResource)
* Genre
* Publisher name
* Place of publication
* Publication date (actual)
* Publication date (questionable, as printed)
* Capture date
* Languages
* Size (inches)
* Notes
* Subjects
* Access Restrictions
* Alternate Identifiers
* Issues
* Sections
* Pages

For our example we entered:

* Title: "Kürschners Handlexikon für alle Wissensgebiete"
* Volume: -
* Abstract: -
* Alternative titles: -
* Names:
  * Corporate: Name: "alexana Digital Library", Role(s): "Owner"
  * Personal: Family: "Kürschner", Given: "Joseph", Date: "† 29.07.1902", Role(s): "Creator"
* Type of resource: "text"
* Genre: "encyclopedia"
* Publisher name: "Union Deutsche Verlagsgesellschaft"
* Place of publication: "Stuttgart, Berlin, Leipzig"
* Publication date (actual): 1929
* Publication date (questionable, as printed): -
* Capture date: -
* Languages: "German"
* Size (inches): -
* Notes: -
* Subjects: -
* Access Restrictions: -
* Use and reproduction restrictions: -
* Alternate Identifiers: -

#### Review Tool Preferences

Let's have a review of the default Preferences and if they correctly work for our book.
Choose menu "File - Preferences".

##### Sorting

The files of our digital object are named "alx-00001.jpg" up to "alx-0935.jpg".
The default sorting "Alphanumeric" correctly sorted the files. The sequence in the generated METS-XML (see tab "METS XML") is sorted correctly. So we do not have to change this for this book / digital object.

If images would be named like "1.jpg", "2.jpg", ... , "10.jpg", ... , "935.jpg" the alphanumeric sorting would be not correct. You should choose "Alphabetic" sorting then.

##### Object Id Assignment

As we work on a local machine with no shared data "Object ID source" - "User configuration" is ok.
As we do not want to assign root IDs manual, the checkbox "Allow manual root ID assignment" is not checked.I
##### Metadata

We change default language from "English" to "German" as most of our books are in german language.
Default pagination numbers are created "Sequential" starting from 1.

##### Layout

The tools layout is ok. We keep "Object display position" as "Above metadata".

##### Spelling

For our german books we install the dictionary for "German (Germany)" and select it.

#### Final METS-XML

We click modify metadata again and save without changes to make sure that we have the most recent METS-XML generated according to our preferences.

Close "ResCarta Metadata Creation Tool" ("File - Exit").

### Step 2: ResCarta Data Conversion Tool

#### Configuration

As we do want the results of optical character recognition (OCR) of our images to be written in separate ALTO (Analyzed Layout and Text Object)-files (and not in TIFF-headers), we have to configure this.

File `~/.RcTools/config/RcSystem.properties`:

```
# ALTO
#
# If set to false text will stored in TIFF headers.
org.rescarta.alto.output.enabled=true
```

For more information about MODS/ALTO, refer to the website at the Library of Congress:
<http://www.loc.gov/standards/alto> or <http://www.loc.gov/mods>

#### Do conversion

The "ResCarta Data Conversion Tool" is used to convert your TIFF, JPEG, PDF (image only), PDF (image and text),Wav or MP4 files into *ResCarta archive data format*. Any new, digitized object needs object metadata in order to become part of your ResCarta archive.

Start the "ResCarta Data Conversion Tool":

```
$ export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
$ cd ~/RcTools-7.0.5/bin
$ ./2_ResCartaDataConversionTool.sh
```

![ResCarta Data Conversion Tool - start](/assets/topics/linux/rescarta/rescarta-data-conversion-tool-01.jpg)

* Click the button to the right of the "Source data directory" text field to specify the directory in which your source files are located. In our case (see above): "/media/ralf/TOSHIBA EXT/Gescannte Bücher/ALX/"

Note: In "Object per Directory mode" ResCarta toolkit requires that all files representing a particular
object are stored in the same directory and all directories representing other objects are stored under
the same "parent" directory (here "ALX"), enter or browse to that parent directory to work with any or all of the
objects included within it.

* Click the button to the right of the "Destination directory" text field to specify the directory into which
your converted objects will be placed. Again choose a parent directory.

We create and select a new directory "/media/ralf/TOSHIBA EXT/TEMP/RCDATA01" for our (temporary for the conversion process) ResCarta archive.

**Important:** The destination directory can be located wherever you specify, if it does not exist at the
time you begin the conversion process it will be created, it must be named `RCDATA01` (upper case).

**Safety Note**: As a good practice do not use your ResCarta archive (the intended final location of your
data) as your destination directory; use an intermediate location during the conversion process, and
then transfer converted data from this intermediate location to your ResCarta archive.

* Select the "Source data type" as "Object per directory" from the pull down and select the "Source metadata type" as "ResCarta METS".

**Note:** The "Source metadata type" for "objects per directory" is currently restricted to "ResCarta METS".
Future versions of the *ResCarta Data Conversion Tool* will include support for other source metadata types.

* Select "TIFF compression (color & gray)" as "Uncompressed"

**Suggestion**: We suggest that your archive contain uncompressed color and gray TIFF images for long term preservation
advantages. But if you are concerned with storage space or recompression of existing JPEG compressed images,
then use of the Data Conversion Tool TIFF compression dialog is advised. When compression is set to JPEG @
100%, original compressed tiles or strips from JPEG compressed TIFF source files will be reused to prevent image
degradation.

* Select "Enable OCR" for optical character recognition (with `tesseract` engine).

* Select "Source metadata filename" as "metadata.xml".

**Note**: If you created metadata using the *ResCarta Metadata Creation Tool*, the source
metadata filename is `metadata.xml`. If the source metadata filename you specify does not exist in an
object directory, the conversion process will fail.

* Finally click on the tools button on the right side and specify the locations for ghostscript and ffmpeg (not neede for our book, but good to set it now for later usage): Ghostscript (/usr/bin/gs), FFmepg (!typo) (/usr/bin/ffmpeg)

* Start conversion by clicking "Begin Conversion"

If you get trouble with
* missing "metadata.xml" files in subdirectories of "ALX"
* missing language mapping for german language for tesseract OCR
* `libtesseract.so.3` not found
* `libpng12.so.0` not found
* getting a tesseract core dump "actual_tessdata_num_entries_ <= TESSDATA_NUM_ENTRIES"
* getting a tesseract core dump "Illegal min or max specification!"

see solutions under [Troubleshooting](#troubleshooting2).

Unfortunately we ran into the second tesseract problem that could not be solved, so we deactivated "OCR" :-(p

* Restart "Begin Conversion" without OCR

We started at 16:52h ... so let's see how long conversion for 935 files take ...

![ResCarta Data Conversion Tool - conversion](/assets/topics/linux/rescarta/rescarta-data-conversion-tool-03.jpg)

It finished after 18 minutes:

![ResCarta Data Conversion Tool - conversion finished](/assets/topics/linux/rescarta/rescarta-data-conversion-tool-04.jpg)

Close tool.

The converted result is in target directory `/media/ralf/TOSHIBA EXT/TEMP/RCDATA01/ALX00000/00000001/00000001/` and contains these files:

* `00000001.tif` ... `00000935.tif`
* `metadata.xml` with updated file paths

### Step 3: Create collections

To be able to index data it is required to create collections first.

Start *ResCarta Collections Manager*:
 
```
$ export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
$ cd ~/RcTools-7.0.5/bin
$ ./5_ResCartaCollectionsManager.sh
```

![ResCarta Collections manager Tool - start](/assets/topics/linux/rescarta/rescarta-collections-manager-tool-01.jpg)

**Important**: You should copy your ResCarta data from its intermediate location to your ResCarta archive
(its intended final location) before using the ResCarta Collections Manager.

(* Resize window to a bigger size / fullscreen, to avoid again preview image crashes...)
* move content of temporary `TEMP/RCDATA01` to final destination `/media/ralf/TOSHIBA EXT/RCDATA01/`

```
$ mkdir /media/ralf/TOSHIBA\ EXT/RCDATA01/
$ mv /media/ralf/TOSHIBA\ EXT/TEMP/RCDATA01/* /media/ralf/TOSHIBA\ EXT/RCDATA01/
```

**Note**: After making any changes to your ResCarta collections, objects (ResCarta-formatted objects), or
metadata, the ResCarta Indexer must be run to update the index!

* Open ResCarta archive directory: Menu "File - Open ResCarta Data Volume": we select "/media/ralf/TOSHIBA EXT/RCDATA01"
* Add a collection "Encyclopedias" by clicking "+"-button ("New Collection")
* Add object to collection by clicking second "+"-button ("Add Objects to Collection"), select book
* Save collections with menu "File - Save Collections Data" (or "Ctrl-S")

The collections metadata is stored as METS in file `/media/ralf/TOSHIBA EXT/RCDATA01/metadata.xml`:

```
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<mets xmlns="http://www.loc.gov/METS/" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" TYPE="ResCarta Collection Metadata v7.0" xsi:schemaLocation="http://www.loc.gov/METS/ http://www.loc.gov/standards/mets/mets.xsd http://www.loc.gov/mods/v3 http://www.loc.gov/standards/mods/v3/mods-3-4.xsd" xmlns:mets="http://www.loc.gov/METS/" xmlns:mods="http://www.loc.gov/mods/v3">
 <metsHdr CREATEDATE="2023-07-03T18:16:38.433+02:00" RECORDSTATUS="Complete">
  <agent ROLE="CREATOR" TYPE="ORGANIZATION">
   <name>ResCarta Foundation, Inc.</name>
  </agent>
 </metsHdr>
 <dmdSec ID="DMD0001">
  <mdWrap MDTYPE="MODS" MIMETYPE="text/xml">
   <xmlData>
    <mods:mods>
     <mods:titleInfo>
      <mods:title>Encyclopedias</mods:title>
     </mods:titleInfo>
     <mods:identifier type="local">a248a6bd-4d5b-4e20-b09c-32041cb8cde1</mods:identifier>
    </mods:mods>
   </xmlData>
  </mdWrap>
 </dmdSec>
 <dmdSec ID="DMD0002">
  <mdWrap MDTYPE="MODS" MIMETYPE="text/xml">
   <xmlData>
    <mods:mods>
     <mods:titleInfo>
      <mods:title>Kürschners Handlexikon für alle Wissensgebiete</mods:title>
     </mods:titleInfo>
     <mods:name type="corporate">
      <mods:namePart>alexana Digital Library</mods:namePart>
      <mods:role>
       <mods:roleTerm authority="marcrelator" type="code">own</mods:roleTerm>
       <mods:roleTerm authority="marcrelator" type="text">Owner</mods:roleTerm>
      </mods:role>
     </mods:name>
     <mods:name type="personal">
      <mods:namePart type="family">Kürschner</mods:namePart>
      <mods:namePart type="given">Joseph</mods:namePart>
      <mods:namePart type="date">† 29.07.1902</mods:namePart>
      <mods:role>
       <mods:roleTerm authority="marcrelator" type="code">cre</mods:roleTerm>
       <mods:roleTerm authority="marcrelator" type="text">Creator</mods:roleTerm>
      </mods:role>
     </mods:name>
     <mods:typeOfResource>text</mods:typeOfResource>
     <mods:genre authority="marcgt">encyclopedia</mods:genre>
     <mods:originInfo>
      <mods:place>
       <mods:placeTerm type="text">Stuttgart, Berlin, Leipzig</mods:placeTerm>
      </mods:place>
      <mods:publisher>Union Deutsche Verlagsgesellschaft</mods:publisher>
      <mods:dateIssued encoding="iso8601">1929</mods:dateIssued>
      <mods:dateCaptured encoding="iso8601">2023-07-03</mods:dateCaptured>
      <mods:issuance>monographic</mods:issuance>
     </mods:originInfo>
     <mods:language>
      <mods:languageTerm authority="iso639-2b" type="code">ger</mods:languageTerm>
     </mods:language>
     <mods:identifier type="local">ALX00000/00000001/00000001</mods:identifier>
     <mods:location>
      <mods:url>ALX00000/00000001/00000001</mods:url>
     </mods:location>
    </mods:mods>
   </xmlData>
  </mdWrap>
 </dmdSec>
 <structMap TYPE="LOGICAL">
  <div TYPE="collections">
   <div DMDID="DMD0001" TYPE="collection">
    <div DMDID="DMD0002" TYPE="monograph"/>
   </div>
  </div>
 </structMap>
</mets>
```

* Close tool.

### Step 4: ResCarta Indexer

For indexing of the whole archive:

Start Indexer:

```
$ export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
$ cd ~/RcTools-7.0.5/bin
$ ./6_ResCartaIndexer.sh
```

* Add "ResCarta Data Volume Directory (RCDATA01)": "/media/ralf/TOSHIBA EXT/RCDATA01"
* "Index Output Directory" is set automatically to "/media/ralf/TOSHIBA EXT/RCDATA01/index.ir7"
* Options: "Verbose"

![ResCarta Indexer Tool - conversion](/assets/topics/linux/rescarta/rescarta-indexer-tool-01.jpg)

Click "Begin indexing".

As we only have one object this is fast:

![ResCarta Data Conversion Tool - indexing finished](/assets/topics/linux/rescarta/rescarta-indexer-tool-02.jpg)

Close tool.

### Step 5: ResCarta Web

The installation of ResCarta Toolkit dis also install an Apache Tomcat with a ResCarta Web webapp and sample data.

Let's have a look:

```
$ ~/RcTools-7.0.5/apache-tomcat-8.5.31/bin/startup.sh
Using CATALINA_BASE:   /home/ralf/RcTools-7.0.5/apache-tomcat-8.5.31
Using CATALINA_HOME:   /home/ralf/RcTools-7.0.5/apache-tomcat-8.5.31
Using CATALINA_TMPDIR: /home/ralf/RcTools-7.0.5/apache-tomcat-8.5.31/temp
Using JRE_HOME:        /usr/lib/jvm/java-11-openjdk-amd64
Using CLASSPATH:       /home/ralf/RcTools-7.0.5/apache-tomcat-8.5.31/bin/bootstrap.jar:/home/ralf/RcTools-7.0.5/apache-tomcat-8.5.31/bin/tomcat-juli.jar
Tomcat started.
```

Browse <http://localhost:8302/ResCarta-Web/>

![ResCarta Web - sample data](/assets/topics/linux/rescarta/rescarta-web-01.png)

To change served archive to your archive, we have to reconfigure the webapp:

* Log into restricted user area by clicking link "Log in" in the upper right: <http://localhost:8302/ResCarta-Web/jsp/RcWebUserWelcome.jsp>
* Authenticate with "admin"/"password"
* Click tab "Server Administration"
* Change "ResCarta Data Volume Directory" to your ResCarta archive directory, e.g. "/media/ralf/TOSHIBA EXT/RCDATA01/"
* Click "Save"

The configuration got saved into file `~/RcTools-7.0.5/apache-tomcat-8.5.31/webapps/ResCarta-Web/work/conf/rcWebConf.xml`:

```
<rcDataVolume>/media/ralf/TOSHIBA EXT/RCDATA01</rcDataVolume>
```

* View new content by clicking on tab "Browse Titles"

![ResCarta Web - own data](/assets/topics/linux/rescarta/rescarta-web-02.png)

Success! We see our first digital object (book) and we can view it by clicking on it:

![ResCarta Web - viewer](/assets/topics/linux/rescarta/rescarta-web-03.png)

What is missing:

* preview image
* search facets/filter

#### Generate thumbnails

For thumbnails we have to generate them:

```
$ java -Xmx1G -cp "/home/ralf/RcTools-7.0.5/lib/*" \
org.rescarta.tools.cmd.RcWebThumbnailGenerator /media/ralf/TOSHIBA\ EXT/RCDATA01 /media/ralf/TOSHIBA\ EXT/RCDATA01/thumbs
Generating thumbnails for ALX00000/00000001/00000001...(Done)
```

Now we see the preview image in the web:

![ResCarta Web - preview image](/assets/topics/linux/rescarta/rescarta-web-04.png)

#### Facets/Filter

ResCarta-Web allows the user to limit a browse listing to particular metadata elements of objects. These
can be anyone of the following facets:

```
TITLE, VOLUME, ISSUE, EDITION, ROLE_AUTHOR, ROLE_CREATOR, ROLE_PHOTOGRAPHER,
ROLE_PUBLISHER, DIGITAL_PUBLISHER_NAME, ,TYPE_OF_RESOURCE, GENRE,
PLACE_OF_PUBLICATION, DATE_PUBLISHED, YEAR_PUBLISHED, MONTH_PUBLISHED,
DAY_PUBLISHED, QUESTIONABLE_DATE_PUBLISHED, DATE_CAPTURED, LANGUAGE,
SUBJECT_TOPIC, SUBJECT_GEOGRAPHIC, SUBJECT_TEMPORAL, SUBJECT_COUNTRY,
SUBJECT_STATE, SUBJECT_COUNTY, SUBJECT_CITY, ALTERNATE_IDENTIFIER
```

Enable and configure facets in `~/RcTools-7.0.5/apache-tomcat-8.5.31/webapps/ResCarta-Web/work/conf/rcWebConf.xml`

```
<!--
Enable / disable facets
true or false
-->
<facetsEnabled>true</facetsEnabled>
```

This was documented in PDF-documentation (I think no longer in use, was not in config file!). But no change: facets not shown. Maybe facets are only shown if more than one object found?

Yes! we see facets after adding another book (to new collection "Fact Books"!

![ResCarta Web - facets](/assets/topics/linux/rescarta/rescarta-web-05.png)

**Important**: When you change metadata of a book, e.g. correcting issue year from 1929 to 1925 and index data again. You will not see the changes in the Web UI (even after restart of Tomcat). Why? Because all metadata is copied to collections' `metadata.xml`! To update it there based on your objects' `metadata.xml`: Open `./5_ResCartaCollectionsManager.sh` remove changed object from collection and add it again. After this start indexer `./6_ResCartaIndexer.sh` (no complete reindexing needed).

Now we get updated data and facets:

![ResCarta Web - updated facets](/assets/topics/linux/rescarta/rescarta-web-06.png)

And what we notice, too: facets having only one value are not shown either: "language" facet is not shown, because only german books are indexed...

And another viewer (Mirador) is installed for browsing collections and objects:

<http://localhost:8302/ResCarta-Web/mirador>

#### Customizing Webapp

##### Images in Header

Header images (left and right one) are configured in `~/RcTools-7.0.5/apache-tomcat-8.5.31/webapps/ResCarta-Web/work/conf/rcWebConf.xml`

```
<!--
  Left header image location
  ______________________________________________________________________
  A path based at the webapp root, or empty for no left header image.
 -->

 <leftHeaderImageLocation>work/header-images/alexana.png</leftHeaderImageLocation>

 <!--
  Left header image HREF
  ______________________________________________________________________
  A link target for the left header image, or empty for no link.
 -->

 <leftHeaderImageHref>/</leftHeaderImageHref>

 <!--
  Right header image location
  ______________________________________________________________________
  A path based at the webapp root, or empty for no right header image.
 -->

 <rightHeaderImageLocation/>

 <!--
  Right header image HREF
  ______________________________________________________________________
  A link target for the right header image, or empty for no link.
 -->

 <rightHeaderImageHref>http://www.rescarta.org</rightHeaderImageHref>
```

In above code we already changed

* the left image location to `work/header-images/alexana.png` and the left image's url to `/`.
* the right image location to "none" (empty) to disable right image display

Copy the referenced image(s) into webapp's directory `~/RcTools-7.0.5/apache-tomcat-8.5.31/webapps/ResCarta-Web/work/header-images/`.

(This configuration and upload could also be done comfortable in browser after login to admin area of webapp.)

##### Custom CSS

If you want to style the page to a higher degree, it is possible to put custom CSS into place.

The standard location ResCarta Web looks for this is `work/css/RcWebCustom.css`.

(for local installation testing this is under `~/RcTools-7.0.5/apache-tomcat-8.5.31/webapps/ResCarta-Web/work/css/RcWebCustom.css`)

This CSS makes a big difference to our styling:

```
#rc-nav-tabs {
    border-bottom-color: #000;
    padding-bottom: 5px;
    border-top: solid 1px #000;
    padding-top: 0px;
    text-transform: uppercase;
}
#rc-nav-tabs li, #rc-nav-tabs li:hover {
    border: none;
    background: none;
}
#rc-nav-tabs li:hover a {
    color: #2b5db4ff;
}
#rc-left-header-area {
    text-align: center;
}
#rc-right-header-area {
    display: none;
}
.rc-nav-tab-content {
    border: none;
}
#rc-nav-tabs li#current a {
    color: #2b5db4ff;
}
```

##### Remove login link

Finally we do not want the login link to be shown. We can deactivate it again in `~/RcTools-7.0.5/apache-tomcat-8.5.31/webapps/ResCarta-Web/work/conf/rcWebConf.xml`:

```
<!--
  Enable / disable login link
  ______________________________________________________________________
  true or false
 -->

 <loginLinkEnabled>false</loginLinkEnabled>
```

Setting `loginLinkEnabled` to `false`.

And here is the result with custom header image, custom CSS and deactivated login link:

![ResCarta Web - customizing](/assets/topics/linux/rescarta/rescarta-web-07.png)

##### Install on Server

* Transfer archive to server location:

Login to your webserver and prepare directory for archive directory:

```
root@yourserver# cd /var/www/
# mkdir www.yourdomain.com
# chown nginx:nginx www.yourdomain.com/
# chmod 777 www.yourdomain.com
```

Example using `scp`:

```
$ cd /media/ralf/TOSHIBA\ EXT
$ scp -r RCDATA01 username@yourdomain.com:/var/www/www.yourdomain.com/
```

* Transfer Tomcat to server:

```
$ cd ~/RcTools-7.0.5
$ tar -zcvf  apache-tomcat-8.5.31.tar.gz apache-tomcat-8.5.31
$ scp apache-tomcat-8.5.31.tar.gz username@yourdomain.com:~/
```

* Login to your webserver and unpack Apache Tomcat to a suitable directory:

```
root@yourserver# cd /usr/local/bin/
# tar xvfz /home/username/apache-tomcat-8.5.31.tar.gz
```

* Configure archive directory:

```
$ nano /usr/local/bin/apache-tomcat-8.5.31/webapps/ResCarta-Web/work/conf/rcWebConf.xml
<rcDataVolume>/var/www/www.yourdomain.com/RCDATA01</rcDataVolume>
```

* Configure start and shutdown scripts and change lines according to your environment, e.g.:

```
$ nano /usr/local/bin/apache-tomcat-8.5.31/bin/startup.sh
cd "/usr/local/bin/apache-tomcat-8.5.31/bin"
export JAVA_HOME="/usr/lib/jvm/adoptopenjdk-11-hotspot-amd64"
$ nano /usr/local/bin/apache-tomcat-8.5.31/bin/shutdown.sh
cd "/usr/local/bin/apache-tomcat-8.5.31/bin"
export JAVA_HOME="/usr/lib/jvm/adoptopenjdk-11-hotspot-amd64"
```

##### Run Server

Execute `startup.sh` on server:

```
# /usr/local/bin/apache-tomcat-8.5.31/bin/startup.sh
```

Browse: <http://www.yourdomain.com:8302/ResCarta-Web/jsp/RcWebBrowse.jsp>

TODO: Put webserver in front of tomcat and redirect to browse page...

#### Syncing local archive with remote archive

Sync remote archive (make sure your user has write access to target directory) with local one:

**Hint**: add option `--dry-run` to simulate the execution of the command to be sure if all is correct.

* Sync only new files on the local machine, that do not exist on the remote machine, e.g.:

```
$ rsync -av --ignore-existing /media/ralf/TOSHIBA\ EXT/RCDATA01/* username@yourdomain.com:/var/www/www.yourdomain.com/RCDATA01/
```

* Sync only updated or modified files on the remote machine that have changed on the local machine, e.g.:

```
$ rsync -av --dry-run --update /media/ralf/TOSHIBA\ EXT/RCDATA01/* username@yourdomain.com:/var/www/www.yourdomain.com/RCDATA01/
```

Restart Tomcat on remote machine.

#### Customize landing page at `/ResCarta-Web/`

When we browse to our webapp without dedicated path to a specific page (browse, search, collections, ...) 
a default landing page is shown.

<http://localhost:8302/ResCarta-Web/> shows:

![ResCarta Web - default landing page](/assets/topics/linux/rescarta/rescarta-web-08.png)

What we want to achieve is a direct redirect to the "browse titles" view under `/ResCarta-Web/jsp/RcWebBrowse.jsp`.

The default landing page is locally located under `~/RcTools-7.0.5/apache-tomcat-8.5.31/webapps/ResCarta-Web/index.htm`

We replace the HTML-code with this automatic redirect code:

```
<!DOCTYPE HTML>
<html lang="en-US">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="refresh" content="0; url='/ResCarta-Web/jsp/RcWebBrowse.jsp'">
        <script type="text/javascript">
            window.location.href = "/ResCarta-Web/jsp/RcWebBrowse.jsp"
        </script>
        <title>Page Redirection</title>
    </head>
    <body>
        If you are not redirected automatically, follow this <a href='/ResCarta-Web/jsp/RcWebBrowse.jsp'>link to browsing titles</a>.
    </body>
</html>
```

When we now browse to <http://localhost:8302/ResCarta-Web/> we get redirected automatically to <http://localhost:8302/ResCarta-Web/jsp/RcWebBrowse.jsp>.

Note: This could be achieved also by using `.htaccess` on Apache Webserver. Above approach is independent of what webserver you use.

After this successful test, we do this change on our productive server, too.
On our installation the file to change is `/usr/local/bin/apache-tomcat-8.5.31/webapps/ResCarta-Web/index.htm`.

#### Putting Webserver in front of Tomcat

Until now our webapp runs in Tomcat under port 8302, e.g. <http://alexana.org:8302/ResCarta-Web/>.

We could configure tomcat to use the port 80 to serve without specifying port in URL.
But we also want hide Tomcat behind a webserver that may do load balancing, SSL-Securing, only let dedicated access to tomcat webapps.
Until now Tomcat also serves another ROOT-webapp with welcome page and examples, host-manager and manager.
When putting Tomcat behind webserver these webapps are no longer accessible to the public (or you delete them from tomcat).

Here is the way to put Apache Tomcat behind a NGinx-webserver (see <https://nginx.org/>).
Our example's target is to put ResCarta-Web <http://alexana.org:8302/ResCarta-Web/> at address <http://alexana.org/ResCarta-Web/> (no SSL, yet).

##### Configure virtual host for domain on NGinx

On server (with already running NGinx webserver): Our nginx configuration `/etc/nginx/nginx.conf` pulls in / includes all configurations iles for all our domains on the webserver from `/etc/nginx/conf.d/*.conf`. So we create a new configuration file for our domain `alexana.org` to be included:

```
$ sudo su -
# nano /etc/nginx/conf.d/alexana.org.conf
server {
  listen 80;
  listen [::]:80;

  server_name alexana.org www.alexana.org;

  root /var/www/www.alexana.org/public_html;

  index index.html index.htm;

  location / {
    try_files $uri $uri/ =404;
  }
}
```

And let's put a `index.html` file in `public_html` directory as configured:

```
# mkdir -p /var/www/www.alexana.org/public_html
# nano /var/www/www.alexana.org/public_html/index.html
<!DOCTYPE html>
<html>
  <head>
    <title>alexana.org</title>
  </head>
<body>
Welcome to alexana.org!
</body>
</html>
```

Note: testing just the single domain file fails because it is not a complete configuration just a part of it (`server` is a subelement of `http`).
To test / validate the new configuration we have to test the complete configuration file `/etc/nginx/nginx.conf`:

```
# nginx -t -c /etc/nginx/nginx.conf 
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

To activate it, we reload the server:

```
# systemctl reload nginx
```

When we now browse <http://alexana.org> we see a simple welcome (out `index.html` file):

```
Welcome to alexana.org!
```

Now we are ready to establish the connection between NGinx and our Apache Tomcat ResCarta webapp.

##### Proxy to Apache Tomcat webapp

As we want build our configuration step by step we start with a simple Proxy from URI `/ ` to the webapp on Tomcat under `http://alexana.org:8302/ResCarta-Web`.

```
# nano /etc/nginx/conf.d/alexana.org.conf
...
location / {
    proxy_pass http://alexana.org:8302/ResCarta-Web/;
}

location /ResCarta-Web/ {
    proxy_pass http://alexana.org:8302/ResCarta-Web/;
}
...
# nginx -t -c /etc/nginx/nginx.conf
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
# systemctl reload nginx
```

When browsing "/" we will get `index.htm` from within ResCarta-Web that redirects use to the browse titles Url at `alexana.org/ResCarta-Web/jsp/RcWebBrowse.jsp`.

All further requests are handled by the second location `/ResCarta-Web`.

###### Setting Request Headers

As clients will now first connect to NGinx before their request is proxied to Tomcat, we have to make sure,
that headers are set into Tomcat request so that the webapp does not get NGinx specific values, but the client values.

TODO

##### Secure connection with TLS/SSL

TODO

# Appendix

## Questions

* Size in inches: How is this calculated? How to change to metric (cm)?
* How to fill and index field DIGITAL_PUBLISHER_NAME? (no GUI)
* Would it be possible to add virtual books (expanding software's target of being an archive management tool to additionally reference remote images (IIIF)...) but local preview image and metadata/METS referencing remote images?
* How to set preview image to another image than 00000001?
* After updating archive: always tomcat restart needed?
* TIF-sizes are very big for usage as web presentation (remote, not original archive). How to create a second (compressed) "presentation" archive? Just do compressed conversion step to another directory and copy index over?
* Why are native libraries used? I know of Java libraries (<https://github.com/haraldk/TwelveMonkeys> to be used for image reading/writing)

## Cheat Sheet of fastest way to curate archive

```
$ export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
$ cd ~/RcTools-7.0.5/bin
$ ./1_ResCartaMetadataCreationTool.sh
$ ./2_ResCartaDataConversionTool.sh
$ ./5_ResCartaCollectionsManager.sh
$ ./6_ResCartaIndexer.sh
$ java -Xmx1G -cp "/home/ralf/RcTools-7.0.5/lib/*" org.rescarta.tools.cmd.RcWebThumbnailGenerator /media/ralf/TOSHIBA\ EXT/RCDATA01 /media/ralf/TOSHIBA\ EXT/RCDATA01/thumbs
```

Sync local with remote archive:

```
$ rsync -av --ignore-existing /media/ralf/TOSHIBA\ EXT/RCDATA01/* username@yourdomain.com:/var/www/www.yourdomain.com/RCDATA01/
$ rsync -av --update /media/ralf/TOSHIBA\ EXT/RCDATA01/* username@yourdomain.com:/var/www/www.yourdomain.com/RCDATA01/
```

Restart Webapp/Tomcat:

```
$ sudo su -
# /usr/local/bin/apache-tomcat-8.5.31/bin/shutdown.sh
# /usr/local/bin/apache-tomcat-8.5.31/bin/startup.sh
```

## Available Roles
{: #availableRoles }

Available roles (MARC relators, taken from sourcecode) are:

Enum value | acronym | Role name | description
-|-|-|-
ABRIDGER | "abr" | "Abridger" | "A person, family, or organization contributing to a resource by shortening or condensing the original work but leaving the nature and content of the original work substantially unchanged. For substantial modifications that result in the creation of a new work, see author")
ACTOR | "act" | "Actor" | "A performer contributing to an expression of a work by acting as a cast member or player in a musical or dramatic presentation, etc."
ADAPTER | "adp" | "Adapter" | "A person or organization who 1) reworks a musical composition, usually for a different medium, or 2) rewrites novels or stories for motion pictures or other audiovisual medium."
ADDRESSEE | "rcp" | "Addressee" | "A person, family, or organization to whom the correspondence in a work is addressed"
ANALYST | "anl" | "Analyst" | "A person or organization that reviews, examines and interprets data or information in a specific area"

TODO

## Types of resource
{: #typeOfResource }

Supported values resource types (MODS typeOfResource, taken from sourcecode) are:

"text", "cartographic", "notated music", "sound recording", "sound recording-musical", "sound recording-nonmusical", "still image", "moving image", "three dimensional object", "software, multimedia", "mixed material"

# Troubleshooting
{: #troubleshooting }

## Preparation

Problem: I just have files in JPEG2000 (*.jp2) format.

Solution: Convert them to TIFF:

```
$ mogrify -format tif *.jp2
```

## 1_ResCartaMetadataCreationTool.sh
{: #troubleshooting1 }

### "SubsampleAverage" operation`s value for parameter "scaleX" is invalid.

Problem:

After selecting of object type in modal dialog of metaata creation tool, the preview image and metadata form fields are supposed to be shown.

![ResCarta Metadata Creation Tool - Monograph create metadata](/assets/topics/linux/rescarta/rescarta-metadata-creation-tool-10.jpg)

If you get an error dialog showing "java.lang.IllegalArgumentException: "SubsampleAverage" operation`s value for parameter "scaleX" is invalid.", the creation of the thumbnail view for the first image of the book failed.
ResCarta could not resize the thumbnail because the desired width is invalid.

Solution:

This is caused if you resized the window of the tool to a too small size.
Resize the window to a bigger size and try again.

## 2_ResCartaDataConversionTool.sh
{: #troubleshooting2 }

### RcTools-7.0.5/lib/libz.so.1

Problem:

When starting `2_ResCartaDataConversionTool.sh` the application crashes with stacktrace:

```
$ ./2_ResCartaDataConversionTool.sh 
Exception in thread "main" java.lang.UnsatisfiedLinkError: /usr/lib/jvm/java-11-openjdk-amd64/lib/libfontmanager.so: /home/ralf/RcTools-7.0.5/lib/libz.so.1: version `ZLIB_1.2.9' not found (required by /lib/x86_64-linux-gnu/libpng16.so.16)
```

Solution:

Change to installation location and move/rename the `lib/libz.so.1` file:

```
$ cd ~/RcTools-7.0.5
$ mv lib/libz.so.1 libz.so.1.save
$ ls
apache-tomcat-8.5.31  bin  docs  lib  libz.so.1.save  RcToolsLicense.txt  tessdata  Uninstaller
```
### metadata.xml does not exist

Problem:

If you get an error message like this:

```
java.io.IOException: Unable to read METS record from /media/ralf/TOSHIBA EXT/Gescannte Bücher/ALX/advent_und_weihnacht/data/done/metadata.xml - /media/ralf/TOSHIBA EXT/Gescannte Bücher/ALX/advent_und_weihnacht/data/done/metadata.xml does not exist
```

you selected a parent-directory containing subdirectories with images of books that do not have `metadata.xml` created, yet.

Solution:

Choose as source data directory directly the directory containing a ready book, e.g. `/media/ralf/TOSHIBA EXT/Gescannte Bücher/ALX/Kürschners_Handlexikon-600dpi`

### No OCR language mapping found

Problem:

`metadata.xml` defines a content language not yet installed.

You will get an error message like this:

```
No OCR language mapping found for ger.
```

because tesseract does not have german language mapping, yet.
By default ResCarta Data Converion Tool only supports english language.

Solution:

Adding Addtional OCR Language Support to `tesseract` OCR engine.

To support other languages you will need to download a language pack from the Tesseract site. The packs are available in compressed formats. You will need to uncompress them and add them to the ResCarta Toolkit installation directory `tessdata` directory which by default is `~/RcTools-7.0.X/tessdata`.

For Tesseract 3 traineddata, browse <https://github.com/tesseract-ocr/tessdata/tree/3.04.00>

We download

* `deu_frak.traineddata` - support for german language in fraktur typography
* `deu_frak.traineddata`

After downloading we have to uncompress them and place into directory `~/RcTools-7.0.5/tessdata`

See <https://tesseract-ocr.github.io/tessdoc/Data-Files>:

```
$ cd ~/RcTools-7.0.5/tessdata
$ cp ~/Downloads/deu*.traineddata .
$ combine_tessdata -u deu.traineddata deu.

Extracting tessdata components from deu.traineddata
Wrote deu.unicharset
Wrote deu.unicharambigs
Wrote deu.inttemp
Wrote deu.pffmtable
Wrote deu.normproto
Wrote deu.punc-dawg
Wrote deu.word-dawg
Wrote deu.number-dawg
Wrote deu.freq-dawg
Wrote deu.shapetable
Wrote deu.bigram-dawg
Wrote deu.params-model
Wrote deu.version
Version string:Pre-4.0.0
1:unicharset:size=7836, offset=192
2:unicharambigs:size=3859, offset=8028
3:inttemp:size=1211942, offset=11887
4:pffmtable:size=886, offset=1223829
5:normproto:size=14016, offset=1224715
6:punc-dawg:size=5154, offset=1238731
7:word-dawg:size=1038274, offset=1243885
8:number-dawg:size=1650, offset=2282159
9:freq-dawg:size=1594, offset=2283809
13:shapetable:size=66226, offset=2285403
14:bigram-dawg:size=11014922, offset=2351629
16:params-model:size=688, offset=13366551
23:version:size=9, offset=13367239

$ combine_tessdata -u deu_frak.traineddata deu_frak.

Extracting tessdata components from deu_frak.traineddata
Wrote deu_frak.unicharset
Wrote deu_frak.inttemp
Wrote deu_frak.pffmtable
Wrote deu_frak.normproto
Wrote deu_frak.punc-dawg
Wrote deu_frak.word-dawg
Wrote deu_frak.number-dawg
Wrote deu_frak.freq-dawg
Wrote deu_frak.version
Version string:Pre-4.0.0
1:unicharset:size=1716, offset=192
3:inttemp:size=1183293, offset=1908
4:pffmtable:size=799, offset=1185201
5:normproto:size=16901, offset=1186000
6:punc-dawg:size=170, offset=1202901
7:word-dawg:size=569218, offset=1203071
8:number-dawg:size=90, offset=1772289
9:freq-dawg:size=206322, offset=1772379
23:version:size=9, offset=1978701
```

Language support is controlled by the contents of the file `~/.RcTools/config/RcTessLangMap.properties`.

```
#Map ISO languages to Tesseract languages for ResCarta data conversion
#Fri Jun 30 14:42:32 CEST 2023
rus=rus
fre=fra
eng=eng
```

The above shows Russian, French and English settings for mapping from ISO language to Tesseract language. Note that French
and English ISO language three character codes do not map directly to the Tesseract codes.

We add the german mappings but activate only the fraktur type, as we need this mapping for our example book that is printed in fraktur.

```
#ger=deu
ger=deu_frak
```

Remember to change this configuration in the future if you have a german book being not printed in fraktur!

### Unable to load library 'libtesseract.so.3'

Problem:

If you start conversion with enabled OCR, you may get the error:

```
Error converting page 1 of alx-0001.jpg: Unable to load library 'libtesseract.so.3': Native library (linux-x86-64/libtesseract.so.3) not found in resource path (../lib/rc-ir-sdk-7.0.5.jar:../lib/rc-audiomd-sdk-7.0.5.jar:../lib/rc-jedit-syntax-2.2.2.1.jar:../lib/rc-ir-indexer-7.0.5.jar:../lib/FastInfoset-1.2.16.jar:../lib/commons-logging-1.2.jar:../lib/lucene-grouping-7.3.0.jar:../lib/xmpcore-6.1.10.jar:../lib/rc-cmgr-7.0.5.jar:../lib/rc-mets-sdk-7.0.5.jar:../lib/slf4j-api-1.6.6.jar:../lib/poi-4.0.1.jar:../lib/commons-codec-1.11.jar:../lib/rc-dct-7.0.5.jar:../lib/jhighlight-1.0.2.jar:../lib/rc-sdk-7.0.5.jar:../lib/commons-lang-2.6.jar:../lib/xmlbeans-3.0.2.jar:../lib/paranamer-2.7.jar:../lib/json-20160810.jar:../lib/opensans-1.10.jar:../lib/jna-4.5.1.jar:../lib/rc-ate-7.0.5.jar:../lib/jaxb-api-2.3.1.jar:../lib/rc-vorbis-sdk-7.0.5.jar:../lib/hunspell-en-us-20070829.jar:../lib/lucene-facet-7.3.0.jar:../lib/log4j-1.2.15.jar:../lib/vlcj-3.10.1.jar:../lib/commons-compress-1.18.jar:../lib/poi-ooxml-schemas-4.0.1.jar:../lib/sphinx4-1.0.0-beta6.jar:../lib/orika-core-1.4.6.jar:../lib/lucene-queries-7.3.0.jar:../lib/rc-cvt-7.0.5.jar:../lib/ipaddress-4.1.0.jar:../lib/commons-io-2.6.jar:../lib/rc-jai-codec-1.1.3.3.jar:../lib/istack-commons-runtime-3.0.8.jar:../lib/slf4j-simple-1.7.12.jar:../lib/txw2-2.3.2.jar:../lib/xml-apis-1.4.01.jar:../lib/javax.activation-api-1.2.0.jar:../lib/rc-mct-7.0.5.jar:../lib/jpedal-lgpl-4.51b32.jar:../lib/rc-jai-core-1.1.3.3.jar:../lib/javassist-3.19.0-GA.jar:../lib/jaxb-runtime-2.3.2.jar:../lib/commons-cli-1.5.0.jar:../lib/KalturaClient-3.3.1.jar:../lib/rc-tme-7.0.5.jar:../lib/commons-httpclient-3.1.jar:../lib/rc-dfut-7.0.5.jar:../lib/curvesapi-1.05.jar:../lib/jcodec-javase-0.2.2.jar:../lib/pdfbox-2.0.16.jar:../lib/javahelp-2.0.02.jar:../lib/xercesImpl-2.11.0.jar:../lib/tritonus-share-0.3.6.jar:../lib/rc-bwf-sdk-7.0.5.jar:../lib/ejml-0.16.jar:../lib/rc-mods-sdk-7.0.5.jar:../lib/tritonus-remaining-0.3.6.jar:../lib/subtitle-0.9.0.jar:../lib/jcodec-0.2.2.jar:../lib/xml-resolver-1.2.jar:../lib/lucene-core-7.3.0.jar:../lib/bcprov-jdk16-1.46.jar:../lib/fontbox-2.0.16.jar:../lib/commons-lang3-3.8.1.jar:../lib/rc-mix-sdk-7.0.5.jar:../lib/sphinx4-hub4-1.0.0-beta6.jar:../lib/rc-kvut-7.0.5.jar:../lib/java-sizeof-0.0.4.jar:../lib/jbig2-imageio-3.0.2.jar:../lib/poi-ooxml-4.0.1.jar:../lib/mediainfo-java-api-1.0.0.RELEASE.jar:../lib/rc-revtmd-sdk-7.0.5.jar:../lib/jna-platform-4.1.0.jar:../lib/stax-ex-1.8.1.jar:../lib/commons-collections4-4.2.jar:../lib/commons-math3-3.6.1.jar:../lib/lucene-analyzers-common-7.3.0.jar:../lib/jai-imageio-1.1.jar:../lib/concurrentlinkedhashmap-lru-1.2_jdk5.jar)
```

Only JAR-files are listed, but existing file `~/RcTools-7.0.5/lib/libtesseract.so.3` can not be found.

Solution:

Open start script and add `lib`-directory containing `libtesseract.so.3` to CLASSPATH:

File `~/RcTools-7.0.5/bin/2_ResCartaDataConversionTool.sh`:

```
export CLASSPATH="../lib/*:/home/ralf/RcTools-7.0.5/lib"
```

### 'libpng12.so.0' not found

Problem:

During conversion we got error:

```
Error converting page 1 of alx-0001.jpg: libpng12.so.0: Kann die Shared-Object-Datei nicht öffnen: Datei oder Verzeichnis nicht gefunden
```

telling us that PNG-lib can not be opened because file or directory not found.

Solution:

Install `libpng` in wanted version:

The documented approach failed:

```
$ cd ~/Downloads/
$ wget http://security.ubuntu.com/ubuntu/pool/main/libp/libpng/libpng12-0_1.2.54-1ubuntu1.1_amd64.deb
$ sudo dpkg -i ./libpng12-0_1.2.54-1ubuntu1.1_amd64.deb
Vormals nicht ausgewähltes Paket libpng12-0:amd64 wird gewählt.
dpkg: Betreffend .../libpng12-0_1.2.54-1ubuntu1.1_amd64.deb, welches libpng12-0:amd64 enthält:
 usrmerge kollidiert mit libpng12-0 (<< 1.2.54-4~)
  libpng12-0:amd64 (Version 1.2.54-1ubuntu1.1) soll installiert werden.

dpkg: Fehler beim Bearbeiten des Archivs ./libpng12-0_1.2.54-1ubuntu1.1_amd64.deb (--install):
 Kollidierende Pakete - libpng12-0:amd64 wird nicht installiert
Fehler traten auf beim Bearbeiten von:
 ./libpng12-0_1.2.54-1ubuntu1.1_amd64.deb
```

So we install library according to <https://www.linuxuprising.com/2018/05/fix-libpng12-0-missing-in-ubuntu-1804.html>:

```
$ sudo add-apt-repository ppa:linuxuprising/libpng12
$ sudo apt update
$ sudo apt install libpng12-0
```

### tesseract core dump with "actual_tessdata_num_entries_ <= TESSDATA_NUM_ENTRIES"

Problem:

During conversion with OCR we got a tesseract core dump:

```
actual_tessdata_num_entries_ <= TESSDATA_NUM_ENTRIES:Error:Assert failed:in file tessdatamanager.cpp, line 53
#
# A fatal error has been detected by the Java Runtime Environment:
#
#  SIGSEGV (0xb) at pc=0x00007fcf0ba51f00, pid=42943, tid=42999
#
# JRE version: OpenJDK Runtime Environment (11.0.19+7) (build 11.0.19+7-post-Ubuntu-0ubuntu122.04.1)
# Java VM: OpenJDK 64-Bit Server VM (11.0.19+7-post-Ubuntu-0ubuntu122.04.1, mixed mode, sharing, tiered, compressed oops, g1 gc, linux-amd64)
# Problematic frame:
# C  [libtesseract.so.3+0x251f00]  ERRCODE::error(char const*, TessErrorLogCode, char const*, ...) const+0x190
#
# Core dump will be written. Default location: Core dumps may be processed with "/usr/share/apport/apport -p%p -s%s -c%c -d%d -P%P -u%u -g%g -- %E" (or dumping to /home/ralf/RcTools-7.0.5/bin/core.42943)
#
# An error report file with more information is saved as:
# /home/ralf/RcTools-7.0.5/bin/hs_err_pid42943.log
#
# If you would like to submit a bug report, please visit:
#   https://bugs.launchpad.net/ubuntu/+source/openjdk-lts
# The crash happened outside the Java Virtual Machine in native code.
# See problematic frame for where to report the bug.
#
```

Solution:

Searching the net I found <https://github.com/gali8/Tesseract-OCR-iOS/issues/299>.
So it seems that the downloaded "traineddata" files are not compatible with the used tesseract version.

The deployed `libtesseract.so.3` is for Tesseract 3 (see <https://pkgs.org/download/libtesseract.so.3()(64bit)>),
the traineddata from <https://github.com/tesseract-ocr/tessdata> are for Tesseract 4:

"These language data files only work with Tesseract 4.0.0 and newer versions."

So we have to delete language files from `tessdata` and install the version 3 traineddata.
(We updated above installation instructions to use the correct version 3.)

```
$ cd ~/RcTools-7.0.5/tessdata
$ rm deu*
```

### tesseract core dump with "Illegal min or max specification!"

Problem:

During conversion tesseract call throws core dump:

```
Error: Illegal min or max specification!
"Fatal error encountered!" == NULL:Error:Assert failed:in file globaloc.cpp, line 75
#
# A fatal error has been detected by the Java Runtime Environment:
#
#  SIGSEGV (0xb) at pc=0x00007f2c77651f00, pid=44522, tid=44581
#
# JRE version: OpenJDK Runtime Environment (11.0.19+7) (build 11.0.19+7-post-Ubuntu-0ubuntu122.04.1)
# Java VM: OpenJDK 64-Bit Server VM (11.0.19+7-post-Ubuntu-0ubuntu122.04.1, mixed mode, sharing, tiered, compressed oops, g1 gc, linux-amd64)
# Problematic frame:
# C  [libtesseract.so.3+0x251f00]  ERRCODE::error(char const*, TessErrorLogCode, char const*, ...) const+0x190
#
# Core dump will be written. Default location: Core dumps may be processed with "/usr/share/apport/apport -p%p -s%s -c%c -d%d -P%P -u%u -g%g -- %E" (or dumping to /home/ralf/RcTools-7.0.5/bin/core.44522)
#
# An error report file with more information is saved as:
# /home/ralf/RcTools-7.0.5/bin/hs_err_pid44522.log
#
# If you would like to submit a bug report, please visit:
#   https://bugs.launchpad.net/ubuntu/+source/openjdk-lts
# The crash happened outside the Java Virtual Machine in native code.
# See problematic frame for where to report the bug.
#
```

Solution:

Found discussion on <https://stackoverflow.com/questions/63634166/tesseract-fatal-error-encountered-nullerrorassert-failedin-file-globaloc>.

So I have Tesseract natively installed on my system:

```
$ tesseract --version
tesseract 4.1.1
 leptonica-1.82.0
  libgif 5.1.9 : libjpeg 8d (libjpeg-turbo 2.1.1) : libpng 1.6.37 : libtiff 4.3.0 : zlib 1.2.11 : libwebp 1.2.2 : libopenjp2 2.4.0
 Found AVX2
 Found AVX
 Found FMA
 Found SSE
 Found libarchive 3.6.0 zlib/1.2.11 liblzma/5.2.5 bz2lib/1.0.8 liblz4/1.9.3 libzstd/1.4.8
```

Uninstallation of tesseract 4 did not help.
So this is a dead end?

So as long ResCarta does not support Tesseract 4 we have to do Data Conversion without OCR :-(...

### Duplicate property or field node 'xmp:Rating'

Problem:

During conversion of a JPG file we got error

```
Error converting page 1 of 001.jpg: com.adobe.internal.xmp.XMPException: Duplicate property or field node 'xmp:Rating'
```

Solution:

Opened file with GIMP (<https://www.gimp.org>) and saved it (overwritting old file).

## 6_ResCartaIndexer.sh

### RCDATA01/metadata.xml does not exist

Problem:

After starting indexing you get a message like this:

```
/media/ralf/TOSHIBA EXT/RCDATA01/metadata.xml does not exist.
```

Solution:

Indexing requires creation of collections before.
Create collections using `5_ResCartaCollectionsManager.sh`
## Developing / Compiling tools from source

If you are eager to contribute or want to debug the tools, you would have to compile them.

### launch4j / windres not found

Problem:

On Ubuntu 22.04 I could not compile completely all tools:

```
$ export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
$ cd ~/development/rescarta.org/rc-tools
$ mvn clean install

[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for ResCarta Tools Parent 7.0.5:
[INFO] 
[INFO] ResCarta Tools Parent .............................. SUCCESS [  0.262 s]
[INFO] ResCarta MIX SDK ................................... SUCCESS [  2.474 s]
[INFO] ResCarta MODS SDK .................................. SUCCESS [  2.133 s]
[INFO] ResCarta AUDIOMD SDK ............................... SUCCESS [  1.658 s]
[INFO] ResCarta rcVTMD SDK ................................ SUCCESS [  2.561 s]
[INFO] ResCarta METS SDK .................................. SUCCESS [  2.134 s]
[INFO] ResCarta Broadcast Wave Format SDK ................. SUCCESS [  1.584 s]
[INFO] ResCarta Vorbis SDK ................................ SUCCESS [  1.315 s]
[INFO] ResCarta SDK ....................................... SUCCESS [  9.459 s]
[INFO] ResCarta Data Format Update Tool ................... FAILURE [  0.312 s]
[INFO] ResCarta Metadata Creation Tool .................... SKIPPED
[INFO] ResCarta Data Conversion Tool ...................... SKIPPED
[INFO] ResCarta Collections Manager ....................... SKIPPED
[INFO] ResCarta IR SDK .................................... SKIPPED
[INFO] ResCarta Indexer ................................... SKIPPED
[INFO] ResCarta Textual Metadata Editor ................... SKIPPED
[INFO] ResCarta Audio Transcription Editor ................ SKIPPED
[INFO] ResCarta Checksum Verification Tool ................ SKIPPED
[INFO] ResCarta Kaltura Video Upload Tool ................. SKIPPED
[INFO] ResCarta-Web ....................................... SKIPPED
[INFO] ResCarta Web Browser Launcher ...................... SKIPPED
[INFO] ResCarta Tools Uninstaller Launcher ................ SKIPPED
[INFO] ResCarta Tools Installer ........................... SKIPPED
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  24.055 s
[INFO] Finished at: 2023-07-02T18:25:11+02:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.bluestemsoftware.open.maven.plugin:rc-launch4j-plugin:1.5.0.2:launch4j (exe) on project rc-dfut: Failed to build the executable; please verify your configuration.: net.sf.launch4j.ExecException: java.io.IOException: Cannot run program "/home/ralf/.m2/repository/org/bluestemsoftware/open/maven/plugin/rc-launch4j-plugin/1.5.0.2/rc-launch4j-plugin-1.5.0.2-workdir-linux/bin/windres": error=2, File or directory not found -> [Help 1]
[ERROR]
```

But when I list the file it is there:

```
$ ls -al /home/ralf/.m2/repository/org/bluestemsoftware/open/maven/plugin/rc-launch4j-plugin/1.5.0.2/rc-launch4j-plugin-1.5.0.2-workdir-linux/bin/windres
-rwxr-xr-x 1 ralf ralf 412348 Mai 25  2013 /home/ralf/.m2/repository/org/bluestemsoftware/open/maven/plugin/rc-launch4j-plugin/1.5.0.2/rc-launch4j-plugin-1.5.0.2-workdir-linux/bin/windres
```

Stackoverflow leads to <https://github.com/TheBoegl/gradle-launch4j/issues/52>, mentioning that it is a `ld` problem
pointing to <https://sourceforge.net/p/launch4j/feature-requests/74/#2051>:

```
I still have the same issue when reinstalling launch4j 3.5 on a new Debian 7 server. Actually, it's even worse as I didn't install ia-libs32 which required adding the i686 architecture and may jeopardize the server stability.

My new workaround under Debian 7 is :
Backup the provided bin/ld and bin/windres. They can't work on a 64 bits system as stated before (No such file or directory)
Install gcc and binutils-mingw-w64-x86-64 packages
* Link to the 32 bits ld and windres provided by mingw :
$ cd launch4j/bin
$ ln -s /usr/bin/x86_64-w64-mingw32-ld ./ld
$ ln -s /usr/bin/x86_64-w64-mingw32-windres ./windres

It may a good idea to provide these new 64 binaries or at least add this in a FAQ (if not already done). Thanks.

...

This should be fixed with version 3.10 and later.
```

So we replace `ld` and `windres` as mentioned above:

```
$ sudo apt install gcc
...
gcc ist schon die neueste Version (4:11.2.0-1ubuntu1).
gcc wurde als manuell installiert festgelegt.
0 aktualisiert, 0 neu installiert, 0 zu entfernen und 5 nicht aktualisiert

$ sudo apt install binutils-mingw-w64-x86-64
...
Die folgenden NEUEN Pakete werden installiert:
  binutils-mingw-w64-x86-64
...
Holen:1 http://de.archive.ubuntu.com/ubuntu jammy/universe amd64 binutils-mingw-w64-x86-64 amd64 2.38-3ubuntu1+9build1 [3.308 kB]
...
binutils-mingw-w64-x86-64 (2.38-3ubuntu1+9build1) wird eingerichtet ...

$ cd ~/.m2/repository/org/bluestemsoftware/open/maven/plugin/rc-launch4j-plugin/1.5.0.2/rc-launch4j-plugin-1.5.0.2-workdir-linux/bin

$ mv ld ld.old
$ ln -s /usr/bin/x86_64-w64-mingw32-ld ./ld

$ mv windres windres.old
$ ln -s /usr/bin/x86_64-w64-mingw32-windres ./windres
```

After this the build still fails now with:

```
[INFO] launch4j: /home/ralf/.m2/repository/org/bluestemsoftware/open/maven/plugin/rc-launch4j-plugin/1.5.0.2/rc-launch4j-plugin-1.5.0.2-workdir-linux/bin/ld: cannot open linker script file ldscripts/i386pe.x: Datei oder Verzeichnis nicht gefunden
[ERROR] 
net.sf.launch4j.BuilderException: net.sf.launch4j.ExecException: Exec failed(1): [Ljava.lang.String;@6aa0c66
```

May be a build on linux is not supported? ... Giving up for now...
