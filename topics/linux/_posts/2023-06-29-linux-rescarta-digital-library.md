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

**IMPORTANT**: Before starting usage of ResCarta, read carefully section "Troubleshooting" and prepare installed system for first use.

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







# Troubleshooting

## 1_ResCartaMetadataCreationTool.sh

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
