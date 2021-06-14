# Description

The archive file ontology-experiment.zip contains software, required run the experiments described in the article "Influence of event representations on natural language querying interfaces to ontologies":
generate ontology test individuals, run experiment of querying and data import performance.

Contents of the archive file:
```bash
generated-data - folder to store .zip files with generated ontology individuals
generator-resources - folder to store resources, used by ontology generator
import-data - folder to store .zip files with generated ontology individuals for data import experiment
queries - folder to store experimental SPARQL queries
graphdb_start.bat - file to start GraphDB database
graphdb_stop.bat - file to stop GraphDB database
ont-expt.jar - program to generate data and run experiments
```

## Requirements

The software runs on a computer that has these minimum software requirements: Windows 8.1, Java JRE 1.8.0, GraphDB 9.3.0 or later.

## Instalation and preparation for experiments

Copy the contents of the archive to any directory on your computer. In graphdb_start.bat file, specify GraphDB.exe file path. In In graphdb_stop.bat file, specify path to Java folder and .exe file of the browser where GraphDB Workbench tool is running.

Create 5 different repositories for each kind of repository (i.e. R1, R2, R3, and R4) to store ontologies of different sizes.
The size of the ontology is determined by the number of individuals of class Talking: 2, 4, 6, 8, and 10 million.
We used a naming convention where the type of repository is specified at the beginning, followed by the keyword talkings, and a number at the end that describes
the size of the ontology in millions of individuals. For example, R1-talkings-2 denotes an R1 repository kind, containing 2 million of talking class individuals.
However, the naming convention can be used differently because the names of the repositories are passed to the program as parameters during the experiments.

When creating a repository, it is important to choose the right ruleset - RDFS-Plus (Optimized) is used for R1, R3, R4, and OWL-Horst (Optimized) is used for R2.
Schema files from the corresponding schema-variants directories must be imported into each repository.

4 additional repositories are also required for data import experiments (e.g., (R1|R2|R3|R4)-talkings-import-test).
The data of the main repository of a certain size (e.g., R1-talkings-2) are copied to these repositories before running data import experiments.


# Usage

The program is launched from the command line. The program works in the following modes: test data generation, experiment of SPARQL query performance, experiment of data import performance.

## Test data generation

The program generates .zip files with ontology individuals to be imported into the appropriate repositories using the GraphDB Workbench tool. The generation program allows to choose
how many instances to generate. To avoid very large files, up to 100,000 individuals of talking class are generated in a single .zip file.

Example of running the program when 5,000
instances are generated for an R1 repository kind:

```bash
java -jar ont-expt.jar generate R1 5000
```

Description of arguments:
```bash
repository_kind (R1 | R2 | R3 | R4) - kind of repository for which to generate data
count_of_talkings - number of talking class individuals to generate
```

The generated files are saved in the generated-data directory.


## Experiment of SPARQL query performance

Before running this experiment, the required amounts of data must be imported into the repositories. The program automatically launches the GraphDB instance and executes queries
the required number of times.

Example of running the program:

```bash
java -jar ont-expt.jar query-test http://localhost:7200 R1-talkings-2 query_1.1.txt
```

Description of arguments:
```bash
database_url - base URL of GraphDB Workbench
repository_name - name of repository where the queries are executed
query_file - SPARQL query file from queries folder
```

The program prints an average execution time on the screen.

All required SPARQL queries are stored in the queries directory. Queries allow to answer the sample questions discussed in the article. Note that different queries can be used
in different types of repositories to answer the same question as presented bellow:
```bash
Q1 (R1, R2, R3, R4) - query_1.1.txt
Q2 (R1, R2, R3, R4) - query_2.1.txt
Q3 (R1) - query_3.1.txt
Q3 (R2, R3) - query_3.2.txt
Q3 (R4) - query_3.3.txt
Q4 (R1) - query_4.1.txt
Q4 (R2, R3) - query_4.2.txt
Q4 (R4) - query_4.3.txt
Q5 (R1) - query_5.1.txt
Q5 (R2, R3) - query_5.2.txt
Q5 (R4) - query_5.3.txt
Q6 (R1) - query_6.1.txt
Q6 (R2, R3) - query_6.2.txt
Q6 (R4) - query_6.3.txt
```

## Experiment of data import performance

Before running this experiment, you need to generate a .zip file with 100,000 individuals, as described in test data generation paragraph, and copy this file to the import-data directory.
To prepare the repository for experiment (e.g., R1-talkings-import-test), copy contents of required repository's (e.g., R1-talkings-2) Storage folder to Storage folder of test repository.
Storage folder path can be seen in repository's edit window in GraphDB Workbench.

Example of running the program:

```bash
java -jar ont-expt.jar import-test http://localhost:7200 R1-talkings-import-test ont-1.zip
```

Description of arguments:
```bash
database_url - base URL of GraphDB Workbench
repository_name - name of repository to import data to
import_data_file - import data .zip file
```

The program prints data import time on the screen.