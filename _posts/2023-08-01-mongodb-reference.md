---
layout: post
title: Mongodb reference
date: 2023-08-01 00:01
category: Mongodb 
author: swapan chakrabarty
tags: [Mongodb]
summary: Mongodb reference
---

``atlas setup --clusterName myAtlasClusterEDU --provider AWS --currentIp --skipSampleData --username myAtlasDBUser --password myatlas-001 | tee atlas_cluster_details.txt``

The setup process will leave you connected in mongosh to your cluster. Click the Check button to continue to the next lab.

mongosh -u myAtlasDBUser -p myatlas-001 mongodb+srv://myatlasclusteredu.ogyr1ni.mongodb.ne

atlas clusters loadSampleData myAtlasClusterEDU

Mongodb datatypes:
json datatypes:
* string
* object
* array
* boolean
* null

bson datatypes
dates
numbers
object id(primary key)

every doc requires _id field which is the primary key
mongodb will auto add it, if its not included in the insert

document can contain different fields
fields may contain differnt types

documents displayed in json but stored as bson

optional validatin 

_id is a required field in every MongoDB document, but it's not a data type. If an inserted document does not include an _id field, MongoDB will automatically create the _id field and populate it with a value of type ObjectId.


**Mongo db data relationaships:**
data that is accessed together should be stored together 

* one-to-one
* one-to-many
* Many-to-many

**Ways to model relationships**
* Embedding
* Referencing

one to many:
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/fdfd5320-5382-4314-8125-b9639bab2fea)

Embedding and Referencing example:
actor document (cast) is embedded within movie document
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/257c93a8-7198-4d0d-a638-0c493a773864)

Modeling exercise:   
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/27e1a162-26ad-4155-ae0c-3c7cddf18c15)   

right now phone numbers is a 1-1 relationship but should be 1 to many   
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/749f349b-bc90-4944-8b55-87eb3240116d)   

better design:   
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/fab90ccc-65be-4eb8-8ac9-0671e1d35511)   

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/ddb17d0e-4895-479b-bba8-c3bf1640a978)   

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/05aa7888-c6ee-4186-b8b8-cc9030152b24)

# Embedded Documents
For one to many and many to many relationships
embedding: 
* avoids application joins
* proves better performance for read operations 
* update related data in a single write operation

issue:
* can create large documents
  * large documents need to be read into memory resulting in bad performance
* continuously adding data without limit creates unbounded documents
* BSON documents can only be 16MB

# Reference
when we store data in different collections we use references to join them
References save the _id field of one document in another document as a link between the two.

references example:   
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/e610db2c-3e4c-409b-8ff7-f44d7f7d47c5)   

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/32ad7649-97b2-4fec-9a2b-41d04fe10375)   

# scaling a data model
* we want to avoid documents that are unbounded (grow infinitely)

Example: this document is unbounded, you can have  1 comment or thousands
* this affects reads as writes    
* for writes, each write reads the entire document into memory   
* will be hard to paginate, all comments will have to be read and filtered in the app   
* may cause storage issue as the doc gets bigger (16mb)   

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/aa3fe2ba-365c-4224-977b-6fafcd039513)   

better design:   
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/65301ec7-7f80-4033-ba3b-d71c2f332f0e)   

# Schema anti patterns   
* massive arrays
* massive number of collections
* bloated documents
* unnecessary indexes
* queries without indexes
* data that is accessed together but stored in different collections    