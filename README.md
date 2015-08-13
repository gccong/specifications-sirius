# Omics Discovery Index

[![Join the chat at https://gitter.im/PSI-PROXI/datadiscoveryindex](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/PSI-PROXI/datadiscoveryindex?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

OmicsDI
=========

Contents
----------

1. [Essentials](#1-essentials)  
 1.1. [Objectives](#11-objectives)    
 1.2. [What is OmicsDI](#12-what-is-omicsdi)  
* [Databases](#2-databases)  
  2.1. [Major Partners?](#21-major-partners)  
  2.2. [Which databases are part of OmicsDI](#22-which-databases-are-part-of-omicsdi)  
  2.3. [OmicsDI Architecture?](#23-Omicsdi-architecture)  
  2.4. [OmicsDI XML](#24-omicsDI-xml)  
  2.5. [How to build a BioDocker container](#25-how-to-build-a-biodocker-container)  
* [Developing containers](#3-developing-containers)  
  3.1. [What do I need to develop?](#31-what-do-i-need-to-develop)  
  3.2. [How to create a container?](#32-how-to-create-a-container)  
* [Support](#4-support)  
  4.1  [Get involved](#41-get-involved)  
  4.2. [Contact](#42-contact)   
* [License](#5-license)  

1. Essentials
--------------

### 1.1. Objectives

* Provide data discovery index for ‘omics’ datasets (genomics, proteomics and metabolomics).
* Integrate databases and repositories from the US and Europe.
* Provide the infrastructure to add new databases/repositories and to search, navigate, and find “high-quality”, relevant datasets.


### 1.2. What is OmicsDI?

In the age of systems biology and data integration, omics data (proteomics/genomics/metabolomics) represents an essential 
component to understand the “whole picture” of life. Access to this data is now a crucial component in biomedical research and
requires substantial investment and infrastructure. Several public repositories have been developed, each with different
purposes in mind such as: [PRIDE] (http://www.ebi.ac.uk/pride/archive/), [PeptideAtlas] (http://www.peptideatlas.org/),
[MassIVE](http://massive.ucsd.edu/ProteoSAFe/), [Metaboligths](http://www.ebi.ac.uk/metabolights/), [Metabolomics Workbench](http://www.metabolomicsworkbench.org/),
[EGA](https://www.ebi.ac.uk/ega/). While there is often integration between repositories for the same data type, discovery
of datasets across multiple omics datatypes remains a tedious task requiring different query strategies across multiple
resources. The aim of OmicsDI is to address these issues by providing a central infrastructure for searching, storing
and visualizing omics datasets.


2. Databases
-------------

## 2.1. Major Partners


- **ProteomeXchange**: The ProteomeXchange Consortium is a collaboration of currently three major mass spectrometry proteomics
data repositories, PRIDE at EMBL-EBI in Cambridge  (UK), PeptideAtlas at ISB in Seattle (US), and MassIVE at UCSD (US), 
offering a unified data deposition and discovery strategy across all three repositories. ProteomeXchange is a distributed
database infrastructure; the potentially very large raw data component of the data is only held at the original submission 
database, while the searchable metadata is centrally collected and indexed. All ProteomeXchange data is fully open after 
release of the associated publication.

- **MetabolomeXchange**: MetabolomeXchange is a collaboration of 4 major metabolomics repositories, with a total of 10 partners
contributing. MetabolomeXchange was inspired by and is implementing similar coordination strategies to ProteomeXchange. 
The founding partners are MetaboLights at EMBL-EBI(UK),Metabolomics Repository Bordeaux(FR), Golm Metabolome Database and 
the Metabolomics Workbench (US). The Metabolomics Workbench is a NIH funded collaboration of 6 Regional Comprehensive 
Metabolomics Resource Cores. MetabolomeXchange started accepting metadata submissions in summer 2014, and reached 200 public
datasets in March 2015.

- **The European Genome-Phenome Archive**: The European Genome-Phenome Archive (EGA) provides a service for the permanent 
archiving and distribution of personally identifiable genetic and phenotypic data resulting from biomedical research projects. 
Data at EGA was collected from individuals whose consent agreements authorise data release only for specific research use by 
researchers. Strict protocols govern how information is managed, stored and distributed by the EGA project.
The EGA comprises a public metadata section, allowing searching and identifying relevant studies, and the controlled access
data section. Access to the data section for a particular study is only granted after validation of a research proposal through
the relevant ethics approval.

### 2.2. Which databases are part of OmicsDI?

Currently six different databases in two continents are part of the OmicsDI consortium: 
 
  - [PRIDE] (http://www.ebi.ac.uk/pride/archive/): The **PR**oteomics **IDE**ntification database. (EMBL-EBI, UK)(Proteomics)
  - [PeptideAtlas] (http://www.peptideatlas.org/): A multi-organism, publicly accessible compendium of peptides identified in a large set of tandem mass spectrometry proteomics experiments. (System Biology Inst, Seattle, US)(Proteomics)
  - [MassIVE](http://massive.ucsd.edu/ProteoSAFe/): The **Mass** spectrometry **I**nteractive **V**irtual **E**nvironment, is a community resource to promote the global, free exchange of mass spectrometry data. (UCSD, US)(Proteomics)
  - [Metaboligths](http://www.ebi.ac.uk/metabolights/): The Metabolomics Database. (EMBL-EBI, UK)(Metabolomics)
  - [Metabolomics Workbench](http://www.metabolomicsworkbench.org/): UCSD Metabolomics Workbench, a resource sponsored by the Common Fund of the National Institutes of Health. (UCSD, US) (Metabolomics)
  - [EGA](https://www.ebi.ac.uk/ega/): The European Genome-phenome Archive. (EMBL-EBI, UK)(Genomics)
  

### 2.3. OmicsDI Architecture?

![Omics Discovery Index Architecture](https://raw.githubusercontent.com/BD2K-DDI/specifications/master/docs/architecture/architecture.png)

The architecture of the Omics Discovery Index started with an XML [see here](#24-omicsDI-xml) files that contains 
the information from each dataset in the database. Each file is retrieved from the providers every night and if a new dataset was 
added the system will add it. 

Each file is indexed using the EBI Search Indexing System and the final information is exposed using web-services. The EBI Search System also 
contain the index of other major databases such as Uniprot, ENSEMBL, Pubmed allowing the user cross-link the biological information with 
those resources [see here](#24-omicsDI-xml) . 

### 2.4. OmicsDI XML?

The OmicsDI XML is used to export every database (including all the datasets) to a common structure. The XML structure (in short)
is the following:

```xml
<database>
    <name>Database Name</name>
    <description>Database Description</description>
    <release>Number of the release</release>
    <release_date>Release Date</release_date>
    <entry_count>Number of entries</entry_count>
    <entries>      
        <entry id="Dataset_ID_1">
            <name>Name of the Dataset</name>
            <description>Description of the dataset</description>
            <cross_references>
                <ref dbkey="CHEBI:16551" dbname="ChEBI"/>
                <ref dbkey="MTBLC16551" dbname="MetaboLights"/>
                <ref dbkey="CHEBI:16810" dbname="ChEBI"/>
                <ref dbkey="MTBLC16810" dbname="MetaboLights"/>
                <ref dbkey="CHEBI:30031" dbname="ChEBI"/>
            </cross_references>
            <dates>
                <date type="submission" value="2013-11-19"/>
                <date type="publication" value="2013-11-26"/>
            </dates>
            <additional_fields>
                <field name="repository">Repository</field>
                <field name="omics_type">Omics Type</field>
            </additional_fields>    
        </entry>
    </entries>
</database>         
```

- Cross reference to other database entities is used to link for example external sources in the dataset. The **dbkey** correspond
 with the entity in the database and the **dbname** correspond with the database id. Some examples:
   
   
  - If the dataset is from a Human sample, the cross reference should be: <ref dbkey="9606" dbname="TAXONOMY"/>
   
  - If the proteomics experiment identified/quantified a UNIPROT id P31946, the cross reference should be: <ref dbkey="P31946" dbname="Uniprot"/>

  - If the dataset was published in a scientific journal and is indexed in pubmed, the cross reference should be: <ref dbkey="26013411" dbname="pubmed"/>

  A complete list of all databases databases for reference can be found in [this site](http://www.ebi.ac.uk/ebisearch/).
  
### 2.5. Cross References to other databases

There are two different ways to run a container.

* Go to the GitHub reposiry with the recipie of the software you want, clone it, and build it yourself on your machine.
* Use the docker daemon to search for a ready-to-use version of the containerized software you want.

Inside the central repository there is a list of softwares with docker recipies, there you can find more information about how to work with them.


3. Developing containers
-----------------------

### 3.1. What do I need to develop?

Docker containers are based on Linux systems, so you will need a computer with Linux installed, you also will need the docker daemon and the software you want to containerize.

### 3.2. How to create a container?

Having all in hands now you need to create a Dockerfile. Dockerfiles are simple recipies to instruct the daemon on how to set an appropriate OS and how to download, manage, install and
give access to the software inside.

You can check the [Docker](https://docs.docker.com/reference/builder/) documentation for more information.

Once the contaienr is ready you can get in touch with us so we can make the appropriate arrangements to make your container availabe to everyone in the community by giving an automated build system.


4. Support
----------

### 4.1. Get involved

Whether you want to make your own software available to others as a container, to just usem them on your pipelines and analysis or just give opinions, you are most welcome. This is a community-driven project, that means everyone has a voice.

Here are some general ideas:

* Browse our list of containers
* Propose your own ideas or software
* Interact with other if you think there is something missing.

### 4.2. Contact

visit the [BioDocker](http://biodocker.github.io/) website.

visit the [GitHub](https://github.com/BioDocker/biodocker) page for the source code.

visit the [Specification](https://github.com/BioDocker/specifications) page to make comments and requests.


5. License
----------

[Apache 2](http://www.apache.org/licenses/LICENSE-2.0)