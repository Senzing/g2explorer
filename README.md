# g2explorer

## Overview

The [G2Explorer.py](G2Explorer.py) utility is an interactive command line utility that allows a user to 
- **search** Find an entity by name, DOB, address, etc.
- **get** Display an entity's records in either summary or detail format.
- **compare** – Compare two or more entities side by side.
- **why** – Display the internal engine keys and stats about an entity to help determine why it did or did not match another. 
- **export** – Export the json records that make up an entity for debugging or reingesting after configuration tweaks are made.

It can also be used to view the reports generated by the following companion projects ...
- https://github.com/Senzing/g2snapshot
- https://github.com/Senzing/g2audit

Usage:

```console
python3 G2Explorer.py  --help 
usage: G2Explorer.py [-h] [-c INI_FILE_NAME] [-s SNAPSHOT_FILE_NAME]
                     [-a AUDIT_FILE_NAME] [-D DEBUG_OUTPUT]

optional arguments:
  -h, --help            show this help message and exit
  -c INI_FILE_NAME, --config_file_name INI_FILE_NAME
                        name of the g2.ini file, defaults to
                        /etc/opt/senzing/G2Module.ini
  -s SNAPSHOT_FILE_NAME, --snapshot_json_file SNAPSHOT_FILE_NAME
                        the name of a json statistics file computed by
                        G2Snapshot.py
  -a AUDIT_FILE_NAME, --audit_json_file AUDIT_FILE_NAME
                        the name of a json statistics file computed by
                        G2Audit.py
  -D DEBUG_OUTPUT, --debug_output DEBUG_OUTPUT
                        print raw api json to screen or <filename.txt>
```

## Contents

1. [Prerequisites](#Prerequisites)
2. [Installation](#Installation)
3. [Typical use](#Typical-use)
4. [Exploring data](#Exploring-data)

### Prerequisites
- python 3.6 or higher
- Senzing API version 2.4 or higher
- python pretty table module (pip3 install ptable)

### Installation

1. Simply place the the following files in a directory of your choice ...  
    - [G2Explorer.py](G2Explorer.py) 

2. Set PYTHONPATH environment variable to python directory where you installed Senzing.
    - Example: export PYTHONPATH=/opt/senzing/g2/python

3. The senzing environment must be set to your project by sourceing the setupEnv script created for it.

Its a good idea to place these settings in your .bashrc file to make sure the enviroment is always setup and ready to go.
*These will already be set if you are using a Senzing docker image such as the sshd or console.*

### Typical use
```console
python3 G2Explorer.py 
```
Optional parameters ...
- The -c config_file parameter is only required if your project's G2Module.ini file can't be found in the usual location.
- The -s snapshot_file_name parameter is for convenience if you just took a snapshot and want to immediately view it.
- The -a audit_file_name parameter is for convenience if you just ran and audit and want to immediately view it.
- The -d debug_output parameter turns on debug to either screen or to a file name.  This shows you what the API is returning 
as you search and get records.  
-- use -d SCREEN to print the debug output to the screen
-- or -d /path/to/filename.txt to print the debug output to whatever file you choose.

### Exploring data

The best documentation for how to use the G2Explorer and its companion projects is located here ...
- https://senzing.zendesk.com/hc/en-us/sections/360009388534-Exploratory-Data-Analysis-EDA- 

But here are some basics ..

```console
python3 G2Explorer.py 

  ____|  __ \     \    
  __|    |   |   _ \   Senzing G2
  |      |   |  ___ \  Exploratory Data Analysis
 _____| ____/ _/    _\ 

Type help or ? to list commands.

(g2) 
```

Next type "help" to see the available commands ...

```console
(g2) help

Documented commands (type help <topic>):
========================================
auditSummary        dataSourceSummary    get   quickLook  search          why
compare             entitySizeBreakdown  help  score      setColorScheme
crossSourceSummary  export               load  scroll     versions      
```

Type "help" on a specific command to find out how to use it ...

```console
(poc) help search

Searches for entities by their attributes.

Syntax:
    search Joe Smith (without a json structure performs a search on name alone)
    search {"name_full": "Joe Smith"}
    search {"name_org": "ABC Company"}
    search {"name_last": "Smith", "name_first": "Joe", "date_of_birth": "1992-12-10"}
    search {"name_org": "ABC Company", "addr_full": "111 First St, Anytown, USA 11111"}

Notes: 
    Searching by name alone may not locate a specific entity.
    Try adding a date of birth, address, or phone number if not found by name alone.
```

Next thing to do is to just start exploring your database!  Here are a few tips ...

**adhoc flow ...**
1. search joe smith *(where "joe smith" is the name of an entity you want to lookup)*
2. get 123 *(where 123 is one of the entity_ids returned by the search)*
3. why 123 *(if entity 123 consists of multiple records and you want to know why they resolved)*
4. compare 123,145 *(where 123 and 145 are two entity_ids you want to compare)*
5. why 123,145 *(where 123 and 145 are two entity_ids you want to see why they did not resolve)*

*Notes:* 
- Be sure to type "help why" to understand what the colors and symbols mean.
- Use "scroll" immediately after any table that is cut off as screen wrapping has been turned off. This will allow you to see the entire table and pan left and right, up and down.

**browsing statistics and examples ...**
1. load /project/snapshots/snapshot1.json *(where snapshot1.json is a file created by G2Snapshot.py)*
2. dataSourceSummary *(when a list of the data sources and their statistics are desired)* 
3. dataSourceSummary customer duplicates *(to view a list of duplicate records in the customer data source)*
4. Select (N)ext or (P)revious to move through the examples.
5. Select (W)hy on any example you want more explanation for.
6. Select (S)croll on any table that is cut off as screen wrapping has been turned off.
7. Select (Q)uit to get out of the report.

Thats it, happy exploring!
