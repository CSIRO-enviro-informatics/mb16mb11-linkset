# Mesh Blocks 2011 to Mesh Blocks 2016 Linkset
This code repository contains a Linkset - a specialised Dataset linking objects in two other Datasets.

This Linkset contains spatial associations between [`Mesh Blocks`](http://linked.data.gov.au/def/asgs#MeshBlock) class objects in [2011](http://linked.data.gov.au/dataset/asgs2011) and [2016](http://linked.data.gov.au/dataset/asgs2016) version of the Australian Statistical Geography Standard (ASGS 2016).

Mesh Blocks, in both versions of the ASGS are represented spatially as polygons and, from one to the other ASGS version, Mesh Blocks may be identical, overlap or have no correlate. This latter situation occurs when Mesh Blocks have either been created or destroyed.

The Mesh Block-to-Mesh Block relations come from the [Australian Bureau of Statistics](https://www.abs.gov.au) but are not the [ASGS Geographic Correspondences as published on data.gov.au](https://data.gov.au/dataset/ds-dga-23fe168c-09a7-42d2-a2f9-fd08fbd0a4ce/details) as those are population reapportionments. Instead, these relations are area-based correspondences obtained directly from the ABS.

The formal definition of what a Linkset is, is provided by the Location Index (LocI) project within its project ontology, see:
http://linked.data.gov.au/def/loci 


## Repository Contents  
* [data.ttl.gz](data.ttl.gz) - this Linkset’s main data file. It is a compressed RDF turtle files
* [header.ttl](header.ttl) - this Linkset’s data.ttl header information, stored separately for ease of access
* [example-data.ttl](example-data.ttl) - 10 Statements from the Linkset for ease of access, in RDF (turtle) format, as per the main data file
* [README.md](README.md) - this file
* [LICENSE](LICENSE) - the data license assigned to this Linkset’s content
* [methods/](methods/) - a folder containing information (prose and code) about how this Linkset was generated


## Purpose
This repository contains a Linkset. [Linkset](http://linked.data.gov.au/def/loci#Linkset)s are specialised Linked Data datasets that link objects, such as Addresses or Catchments, in one Linked Data dataset to objects in another.

Publishing relationships between Datasets as distinct Linksets allows for the independent management of Dataset-to-Dataset relationships.

### Linksets for Spatial Relationships
Where LocI objects across multiple datasets have spatial relationships that we wish to represent, we create Linksets with spatial (topological) relationships such as touches, within, overlaps etc. using terms formalised in the (GeoSPARQL Standard](https://www.opengeospatial.org/standards/geosparql).

### Linksets for Dataset versions
Some LocI Datasets, such as the ASGS, have multiple, independently delivered versions (the ASGS is released as a Linked Data Datasets in both [2011](http://linked.data.gov.au/dataset/asgs2011) and [2016](http://linked.data.gov.au/dataset/asgs2016) versions). Linksets can be used to link between these versions of a Dataset too. This allows for information such as correspondence tables (links between ASGS versions, published by the Australian Bureau of Statistics) to be published as Linked Data independently of any other Dataset.

### This Linkset
This Linkset - Mesh Blocks 2011 to Mesh Blocks 2016 Linkset - is a spatial relations Linkset linking Mesh Blocks in the 2011 ASGS to Mesh Blcoks in the 2016 ASG (both polygons). Many Mesh Blocks are essentially the same in both datasets but there are other proportional overlaps too. Proportional overlaps between Mesh Blocks are described by both the From and To Mesh Blocks *containing* an intermediate polygon that is their intersection. Since the areas of all Mesh Blocks are known from their Datasets (ASGS 2011 & 2016), only the area of the intersecting polygon is recorded in this Linkset.

The link for *Mesh Block 2011:80056497700* -> *Mesh Block 2016:80056497700* reads in 3 parts like this:

**i1** is an intersection polygon with area 53.19m^2  

Mesh Block **2011:80056497700**  
*contains*  
Intersection polygon **i1**  

Mesh Block **2016:80056497700**  
*contains*  
Intersection polygon **i1**  

Thus **2011:80056497700** overlaps **2016:80056497700** by 53.19m^2  which, by using the areas of each Mesh Block, can be expressed as a relation in either direction.


## How is a Linkset’s data organised?
Linksets include the main facts of relations between objects in two datasets - what the IDs of two objects are and how they are related - and they also include information about how links were created, such as what spatial intersection method was used to establish a topological relation. Linkset generation might have employed multiple methods to make all the object-to-object links within it so a Linkset may relate multiple methods and give the particular method used for each link.

Other per-link information may be recorded too: if the links within a Linkset are generated over a significant period of time then the each link may have a created time; if different people/organisations contributed different links then each link may reference their specific contributor.

### Linkset content sections
Linksets use a highly condensed, but still (sort of) human-readable data format to include many (millions) of links. Linkset data files contain:

* **A header**
  * Basic information about the Linksets - what, who when
  * Links to methods used in the generation of the Linkset
* **A (long) set of Statements**
  * One Statement per link
  * Link type - how the two objects relate, spatially or otherwise
  * Link metadata - time of creation (if important), method used and who created it (if known) etc.

Linksets include all their information in one potentially very large file but they also include the header information in a stand-alone text file - header.ttl.

They also include a few (perhaps 10) example Statements in a stand-alone text file - example-data-… .ttl (numbered as there may be many).

### Linkset files
In addition to the main Linkset data file and the header.ttl and example-data.ttl files, there are usually several other files within a Linkset, including this README file. General Linkset files include:

* **data.ttl** - the main Linkset data file. 
  * since this could be very large (1.5GB+), it is often compressed (*data.ttl.gz*) and sometimes split into parts (*data01.ttl.gz*, *data02.ttl.gz*...)
* **header.ttl** - the Linkset’s data.ttl header information, stored separately for ease of access
* **example-data.ttl** - a few statements (perhaps 10) from the Linkset for ease of access
* **README.md** - this file: a description of this Linkset and general Linkset information
* **LICENSE** - the data license assigned to this Linkset’s content

This specific Linkset’s files are listed in above in Repository Content.

### Linkset data format
In its long list of statements, this Linkset expresses each overlap link in 3 parts like this for **2011:80056497700**  -> **2016:80056497700**:  

**i1** is an intersection polygon with area 53.19m^2  

Mesh Block **2011:80056497700**  
*contains*  
Intersection polygon **i1**  

Mesh Block **2016:80056497700**  
*contains*  
Intersection polygon **i1**  

In RDF code  this link is expressed as:

```
:i1 a geo:Feature ;
    dbp:area [ 
        qudt:numericValue: 53.19 ;
        qudt:unit: qudt:SquareMeter 
    ] 
.

:m11o1 
    a rdfs:Statement ;
    rdf:subject               mb11:80056497700 ;
    rdf:predicate             c: ;
    rdf:object                :i1 ;
    loci:hadGenerationMethod  :m1  # a described method
.

:m16o1 
    a rdfs:Statement ;
    rdf:subject               mb16:80056497700 ;
    rdf:predicate             c: ;
    rdf:object                :i1 ;
    loci:hadGenerationMethod  :m1  # a described method
.
```

With contractions used to save data volumes resulting in:

```
:i1 a f: ;
    dbp:area [ 
        nv: 53.19137367788374 ;
        qu: m2: ] .

:m11o1 
    s: mb11:80056497700 ;
    p: c: ;
    o: :i1 ;
    m: :m1 .

:m16o1 
    s: mb16:80056497700 ;
    p: c: ;
    o: :i1 ;
    m: :m1 .
```

See the file [example-data.ttl](example-data.ttl) for the first 10 Statements of the Linkset expressed like this and see the [header.ttl](header.ttl) file to explain all the contractions.


## Linkset metrics
Linksets always contain similar information - links between objects in datasets - and a standard set of metrics can be calculated for any Linkset. These metrics, set by the LocI project, are:

* Number of links
* Number of items in Dataset A (from) not linked
* Number of items in Dataset B (to) not linked
* Number of link creation methods used
* Numbers of uses of each link-creation method

### Metric calculation
A series of queries to calculate Linkset metrics is being prepared here: <https://github.com/CSIRO-enviro-informatics/linkset-metrics>

### This Linkset’s metrics
**Metric** | **Value**
-- | --
Number of links | *not yet calculated*
Number of items in Dataset A (from) not linked | *not yet calculated* 
Number of items in Dataset B (to) not linked | *not yet calculated*
Number of link creation methods used | 1
Numbers of uses of each link-creation method | m1: *not yet calculated*


## Rights & License
The content of this API is licensed for use under the [Creative Commons 4.0 License](https://creativecommons.org/licenses/by/4.0/). See the [license deed](LICENSE) all details.


## Contacts
*LocI Project technical owner*:  
**Nicholas Car**  
CSIRO Land & Water, Environmental Informatics Group  
<nicholas.car@csiro.au>  

*Linkset creator*:  
**Shane Seaton**  
CSIRO Land & Water, Environmental Informatics Group  
<shane.seaton@csiro.au>  

*Linkset correspondences obtained from the Australian Bureau of Statistics' 'geography' unit: <https://www.abs.gov.au/geography>
