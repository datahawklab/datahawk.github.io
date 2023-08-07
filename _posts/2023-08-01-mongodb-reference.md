---
layout: post
title: Mongodb course notes
date: 2023-08-07 00:01    
category: Mongodb course notes
author: swapan chakrabarty
tags: [Mongodb]
summary: Mongodb reference
---

### Mongodb document fundamentals

These are my notes from: <https://learn.mongodb.com/courses/m320-mongodb-data-modeling>

* documents displayed in json but stored as bson
* a document is like a joined row
* Documents are like a map or an associative array
* by default, documents in a collection dont have to have the same schema
* by default, fields in a document dont have to have the same data type

### Mongodb anti patterns

* massive arrays
* massive number of collections
* bloated documents
* unnecessary indexes
* queries without indexes
* data that is accessed together but stored in different collections

### Mongodb patterns

* keep frequently documents in RAM
* keep indexes in RAM
* solid state drives or hard disk
* infrequently used data can use hard disks
* **You either model for simplicity or performance**

### Mongodb datatypes

**json datatypes**

* string
* object
* array
* boolean
* null

**bson datatypes**

* dates
* numbers
* object id(primary key)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/9b329e69-b368-49d6-859e-225bd9f77eb4)

every doc requires _id field which is the primary key
mongodb will auto add it, if its not included in the insert

_id is a required field in every MongoDB document, but it's not a data type. If an inserted document does not include an_id field, MongoDB will automatically create the_id field and populate it with a value of type ObjectId.

### Mongodb to RDBMS translation

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/d1e07104-e064-480f-bc36-a97a0d38d251)

### Relational vs mongodb data modeling

**example relational data model:**

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/df433e94-98d6-4721-a72f-1c2ad3e8c3dc)

**Modeling in Mongodb:**

```json
{
    "_id": "5ad88534e3632e1a35a58d00",
    "customerID": 12345,
    "name": {
        "first": "John",
        "last": "Doe"
    },
    "address": [
        {
            "location": "work",
            "address": {
                "street": "16 Hatfields",
                "city": "London",
                "postal_code": "SE1 8DJ"
            },
            "country": "United Kingdom",
            "geo": {
                "type": "Point",
                "coord": [
                    51.5065752,
                    -0.109081
                ]
            }
        }
    ],
    "email": "john.doe@acme.com",`
    "phone": [
        {
            "location": "work",
            "number": "+44-1234567890"
        }
    ],
    "dob": "1977-04-01T05:00:00Z",
    "interests": [
        "devops",
        "data science"
    ],
    "annualSpend": 1292815.75
}
```

### Mongo db data relationships

data that is accessed together should be stored together

* one-to-one
* one-to-many
* Many-to-many

**Ways to model relationships**

* Embedding
* Referencing

Embedding and Referencing example:
actor document (cast) is embedded within movie document
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/257c93a8-7198-4d0d-a638-0c493a773864)

**NOT candidates for Embedding:**

A document is frequently read, but contains data that is
rarely accessed. Embedding this data only increases the
in-memory requirements

One part of a document is frequently updated and
constantly growing in size, while the remainder of the
document is relatively static

The combined document size would exceed MongoDB’s
16MB document limit

### Mongodb indexing

By default, MongoDB creates an index on the document’s _id primary key field
All user-defined indexes are secondary indexes. Indexes
can be created on any part of the JSON document –
including inside sub-documents and array elements

Index types:

* unique
* compound
* array
* TTL Indexes

In some cases data should expire automatically. Time to Live (TTL) indexes allow the user to specify a period of time after which the database will
automatically delete the data. A common use of TTL indexes is applications that maintain a rolling window of history (e.g., most recent 100 days) for
user actions such as clickstreams

**Wildcard Indexes**
For workloads with many ad-hoc query patterns or that handle highly polymorphic document structures, wildcard indexes give you a lot of extra flexibility. You can define a filter that automatically indexes all matching fields, subdocuments, and arrays in a collection. As with any index, they also need to be stored and maintained, so will add overhead to the database. If your application’s query patterns are known in advance, then you should use more selective indexes on the specific fields accessed by the queries

**Partial Indexes**
Partial Indexes can be viewed as a more flexible evolution of Sparse Indexes, where the DBA can specify an expression that will be checked to determine whether a document should be included in a particular index. e.g. for an "orders" collection, an index on state and delivery company might only be needed for active orders and so the index could be made conditional on {orderState: "active"} – thereby reducing the impact to memory, storage, and write performance while still optimizing searches over the active orders.

**Hash Indexes**
Hash indexes compute a hash of the value of a field and index the hashed value. The primary use of this index is to enable hash-based sharding, a simple and uniform distribution of documents across

**Text Search Indexes**
MongoDB provides a specialized index for text search that uses advanced, language-specific linguistic rules for stemming, tokenization and stop words shards

### Optimizing Performance

* MongoDB’s explain() method
* MongoDB Compass GUI
* Mongodb query profiler
* mongodb ops manager
MongoDB Atlas and Ops Manager eliminates this effort
with the Performance Advisor which monitors queries that
took more than 100ms to execute, and automatically
suggests new indexes to improve performance.

### MongoDB Aggregation Pipeline

MongoDB provides the Aggregation Pipeline natively within the database, which delivers similar functionality to the GROUP BY, JOIN, Materialized View, and related SQL features.

### MongoDB Charts

MongoDB Charts is the quickest and easiest way create and share visualizations of your MongoDB data in real time,

### Mongodb transactions

**Multi-Record ACID Transactional Model**
Because documents can bring together related data that
would otherwise be modelled across separate parent-child
tables in a tabular schema, MongoDB’s atomic
single-document operations provide transaction semantics
that meet the data integrity needs of the majority of
applications.

MongoDB 4.0 added support for multi-document ACID
transactions in the 4.0 release and extended them in 4.2
with Distributed Transactions that operate across
scaled-out, sharded clusters.

### Mongodb Consistency

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/7dca938b-e5ea-4dc0-af62-3a5ac98ed598)

MongoDB handles the complexity of maintaining multiple copies of data via replication. Read and write operations are directed to the primary replica by default for strong consistency, but users can choose to read from secondary replicas for reduced network latency, especially when users are geographically dispersed, or for isolating operational and analytical workloads

When reading data from any cluster member, users cantune MongoDB’s consistency model to match application requirements

**Foreign Keys**

add validation to embedded sub documents which would act like foreign keys in a relational model

**mongodb schema validation**

is very tunable, can be applied to sub documents only

**Example json schema validation:**

* Each document must contain a field named lineItems
* The document may optionally contain other fields
* lineItems must be an array where each element:
* Must contain a title (string), price (number no smallerthan 0)
* May optionally contain a boolean named purchased
* Must contain no further fields

```
db.createCollection("orders",
    {
        validator: {
            $jsonSchema: {
                properties: {
                    lineItems: {
                        type: "array",
                        items: {
                            properties: {
                                title: {
                                    type: "string"
                                },
                                price: {
                                    type: "number",
                                    minimum: 0.0
                                },
                                purchased: { type: "boolean" }
                            },
                            required: [
                                "_id",
                                "title",
                                "price"
                            ],
                            additionalProperties: false
                        }
                    }
                },
                required: ["lineItems"]
            }
        }
    }
)
```

**On-Demand Materialized Views**
Using the $merge stage, outputs from aggregation pipeline queries can now be merged with existing stored result sets whenever you run the pipeline, enabling you to create materialized views that are refreshed on-demand. Rather than a full stop replacement of the existing collection’s content, you can increment and enrich views of your result sets as new data is processed by the aggregation pipeline. With MongoDB’s Materialized Views you have the flexibility to output results to sharded collections – enabling you to scale-out your views as data volumes grow. You can also write the output to collections in different databases further isolating operational and analytical workloads from one another. As the materialized views are stored in a regular MongoDB collection, you can apply indexes to each view, enabling you to optimize query access patterns, and run deeper analysis against them using MongoDB Charts, or the BI and Apache Spark connectors.

## Data modeling for Mongodb

1. identify workloads
how your use your data
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/4f36c3da-4e9f-4b77-960b-28a296f5c715)
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/da0ed415-257c-4a21-b5d6-6668ebbb99d2)
2. document relationships
2. Embedding vs Reference

### How to identify workloads

3. apply design patterns

## Modeling for simplicity vs performance

**simplicity**

* fewer collections
* bigger embedded documents
* objects map well to documents
* fewer disc IO

**Performance**

* Sharding
* supports very fast reads or writes

Performance criteria
latency
operations per second
more collections

### Identify workloads

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/f9fc23ba-e3d7-4fa4-9413-c8383690b8fc)

### qualify and quantify operations

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/0978880e-2a5b-4f05-8b81-b512d87f5811)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/9b1069d9-a966-4a73-89ff-ed5fa084ff7d)

### Examples of types of data and their durability

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/253dc10c-76d6-476f-b246-7e747e99517e)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/b69abee7-7aec-45cf-a26c-4ec8deb9edfc)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/d7ffcd50-be45-4757-a6cd-dff63bb6beb1)

# Relationships and Cardinality

a mother and her children
embed as children should be small amount

a twitter user and his/er followers
reference as a user may have millions of followers

Better entity diagram:

* movie has between 1 and 1000 actors
* has 2 sets of financials
* has between 0 and 100 reviews
* with 30 reviews being the norm
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/9b7361ba-29a8-48f1-be3e-2f44a441474c)

## One to Many

embed on the side of the most queried collection

**One to many using references**

* array of references
* cascade deletes must be handled by application
* use referencing when the associated documents are not always needed with the most queried documents

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/fb1cf213-caba-4536-97b3-a052714f85ed)

**one to many exmaple**

here when i delete or update a store, no need to handle the zip codes since they're kept in the zips collection
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/0a8deea3-6403-43f1-ac0a-fc14ea083df3)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/445800b7-f1f9-4784-9d89-64ebba02fc0e)

**Join operations like SQL joins with $ operator**  

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/5ee1a32a-0071-4b98-8a87-f10db88a0afb)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/74bfe143-1042-42d9-91eb-1c94a5244492)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/f133dbff-25c5-4fc4-9f4a-6aa253a3f133)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/a5efea25-03d9-4aac-91cf-b9b00f6f4368)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/6a05045e-97ca-4288-a48d-053764d6a96d)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/ebd147ad-de99-42d1-bbab-f1014a1c3c81)

### One to Zillions

Document this relationship in the code as it requires special handling
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/586ec814-81c8-4a29-b5be-ae55ab15ea33)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/cb0af6ba-e5a3-4c7b-a182-a9d94049227b)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/ceb75d20-4261-4fa1-a1b3-9d4cebb2fcc3)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/08cb0f7d-0e8d-4289-a3c3-d39fd4f5968f)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/6d6c0171-ba7f-4de3-919f-a92a128f08fb)

# Mongodb Patterns

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/b5a7be52-aca0-4473-9cda-f897460dc740)

**embed address with orders as the shipping address does not changed for shipped and delivered orders so duplication ok**
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/cb381198-6ffe-4db3-857e-96545261bead)

**ok to embed actors with movies as its unlikely to change so duplication ok**
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/d95a0fc9-35d6-4460-9d7a-6aa72758c1da)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/96bd5ed3-eed5-4c9e-9a50-9a6463e239f8)

**how to maintain this sum? maybe data can be stale and some application process updates the gross sum**
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/4c78fea7-a010-4c90-9b08-9601333b5cbc)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/8e0c4c16-46fd-4fea-9ae9-9b5d797b1bd7)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/2a19c827-ecbc-4463-be98-cee966254ef1)
foreign keys and cascading deletes not supported

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/dc68b068-685a-4539-93f8-07c8b28ee7d6)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/898a75a2-8221-4405-9ae8-dd29ae2bf1fe)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/5d02fc9e-dac2-47f9-8648-4acb69bbf574)

Wildcard indexes in mongodb 4.2 may replace the need for indexes on the third section.
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/7a6a4e57-7d65-4f6a-812f-682c755d070c)

Note that wildcard indexes may remove the need to use the pattern below
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/b86645c3-3285-47af-ba67-4ae6cf267a6b)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/f744afed-37e1-4fdd-9a01-9026f5fdf550)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/e3e07ca6-bea7-4892-a3db-b6a6204379ff)
What if i wanted to know the releases between two dates?

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/f43df024-ad03-42ac-b82f-e377aac9205c)
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/e80b39cc-aa1f-4d08-bdc6-2c9bcc36bcc6)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/b14fa701-4f7c-4b0f-a4f2-62bd91fff4b1)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/066e9e4f-7dbc-4308-9ea6-4cb7ae880e21)

**Extended reference pattern**
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/d9d3f33c-0c3a-40fc-9f41-f28427223550)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/122d8033-8b52-4737-a944-b7ea49aa3445)

Here we query orders than we do orders per customer
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/40f5dc7d-652f-4c13-b1da-c059398d3130)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/1fcba275-3ad6-4099-9ac2-51cd5b237889)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/f903c7df-2cc9-41db-a726-a4c746675d61)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/607b897e-33b4-4795-9629-b266042489d9)

**Subset pattern**
![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/8a644bf0-e622-487f-a220-3d1e6e895171)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/c81d0f31-2473-46a6-8e6d-b6b04798e32c)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/a65b65fd-705f-47f6-a0c3-0e0faa80529d)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/f9f561c6-b7f7-4760-8422-b478c64ad542)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/b3c2bf8c-bee5-4ab6-810f-fba0483a4865)

![image](https://github.com/datahawklab/datahawklab.github.io/assets/91769455/a584ea30-db2d-4bd2-b54d-aa72ea785882)

**Lab data attribute pattern**

The purpose of this lab is complete the Pattern Attribute file in the Pattern-Attribute editor tab and then to run thevalidate_m320 in the tab mongodb to ensure this is the correct solution for the lab. The lab covers theAttribute pattern and converting our existing schema to apply this pattern.

User Story

The museum we work at has grown from a local attraction to one that is seen as saving very popular items. For this reason, other museums in the World have started exchanging pieces of art with our museum. Our database was tracking if our pieces are on display and where they are in the museum. To track the pieces we started exchanging with other museum, we added an array called events, in which we created an entry for each date a piece was loaned and the museum it was loaned to.

This schema has a problem where each time we start a exchange with a new museum, we must then add a new index. For example, when we started working with The Prado in Madrid, we added this index:
{ "events.prado" : 1 }
Please review the Problem Schema and the Problem Document editor tabs to help understand the old schema and how we need to change the schema to apply the pattern.

Tasks

To address this issue, you will need to change the schema to:

use a single index on all event dates.
transform the field that tracks the date when a piece was acquired, date_acquisition, so that it is also indexed with the values above.

Problem schema

```
{
  "_id": "<objectId>",
  "title": "<string>",
  "artist": "<string>",
  "date_acquisition": "<date>",
  "location": "<string>",
  "on_display": "<bool>",
  "in_house": "<bool>",
  "events": [{
    "moma": "<date>",
    "louvres": "<date>"
  }]
}
```

Problem document

```
{
  "_id": ObjectId("5c5348f5be09bedd4f196f18"),
  "title": "Cookies in the sky",
  "artist": "Michelle Vinci",
  "date_acquisition": ISODate("2017-12-25T00:00:00.000Z"),
  "location": "Blue Room, 20A",
  "on_display": false,
  "in_house": false,
  "events": [{
    "moma": ISODate("2019-01-31T00:00:00.000Z"),
    "louvres": ISODate("2020-01-01T00:00:00.000Z")
  }]
}
```

Answer schema

```
{
  "_id": "<objectId>",
  "title": "<string>",
  "artist": "<string>",
  "location": "<string>",
  "on_display": "<bool>",
  "in_house": "<bool>",
  "events": [{
    "k": "<string>",
    "v": "<date>"
  }]
}
```

**Subset pattern lab**

User Story

You are the lead developer for an online organic recycled clothing store.

Due to the growing number of environmentally-conscious consumers, our store's inventory has increased exponentially. We now also have an increasingly large pool of makers and suppliers.

We recently found that our shopping app is getting slower due to the fact that the frequently-used documents can no longer all fit in RAM. This is happening largely due to having all product reviews, questions, and specs stored in the same document, which grows in size as reviews and ratings keep coming in.

To resolve this issue, we want to reduce the amount of data immediately available to the user in the app and only load additional data when the user asks for it.

Problem schema

```
{
  "_id": "<objectId>",
  "item_code": "<string>",
  "name": "<string>",
  "maker_brand": "<string>",
  "price": "<decimal>",
  "description": "<string>",
  "materials": ["<string>"],
  "country": "<string>",
  "image": "<string>",
  "available_sizes": {
    "mens": ["<string>"],
    "womens": ["<string>"]
  },
  "package_weight_kg": "<decimal>",
  "average_rating": "<decimal>",
  "reviews": [{
    "author": "<string>",
    "text": "<string>",
    "rating": "<int>"
  }],
  "questions": [{
    "author": "<string>",
    "text": "<string>",
    "likes": "<int>"
  }],
  "stock_amount": "<int>",
  "maker_address": {
    "building_number": "<string>",
    "street_name": "<string>",
    "city": "<string>",
    "country": "<string>",
    "postal_code": "<string>"
  },
  "makers": ["<string>"]
}
```

Problem document

```
{
  "_id": ObjectId("5c9be463f752ec6b191c3c7e"),
  "item_code": "AS45OPD",
  "name": "Recycled Kicks",
  "maker_brand": "Shoes From The Gutter",
  "price": 100.00,
  "description": "These amazing Kicks are made from recycled plastics and
  fabrics.They come in a variety of sizes and are completely unisex in design.
  If your feet don't like them within the first 30 days, we'll return your
  money no questions asked.
  ",
  "materials": [
    "recycled cotton",
    "recycled plastic",
    "recycled food waste",
  ],
  "country": "Russia",
  "image": "https:///www.shoesfromthegutter.com/kicks/AS45OPD.img",
  "available_sizes": {
      "mens": ["5", "6", "8", "8W", "10", "10W", "11", "11W", "12", "12W"],
      "womens": ["5", "6", "7", "8", "9", "10", "11", "12"]
  },
  "package_weight_kg": 2.00,
  "average_rating": 4.8,
  "reviews": [{
      "author": "i_love_kicks",
      "text": "best shoes ever! comfortable, awesome colors and design!",
      "rating": 5
    },
    {
      "author": "i_know_everything",
      "text": "These shoes are no good because I ordered the wrong size.",
      "rating": 1
    },
    "..."
  ],
  "questions": [{
      "author": "i_love_kicks",
      "text": "Do you guys make baby shoes?",
      "likes": 1223
    },
    {
      "author": "i_know_everything",
      "text": "Why do you make shoes out of garbage?",
      "likes": 0
    },
    "..."
  ],
  "stock_amount": 10000,
  "maker_address": {
    "building_number": 7,
    "street_name": "Turku",
    "city": "Saint-Petersburg",
    "country": "RU",
    "postal_code": 172091
  },
  "makers": ["Ilya Muromets", "Alyosha Popovich", "Ivan Grozniy", "Chelovek Molekula"],
}
```

Answer document

```
{
  "_id": ObjectId("5c9be463f752ec6b191c3c7e"),
  "item_code": "AS45OPD",
  "name": "Recycled Kicks",
  "maker_brand": "Shoes From The Gutter",
  "price": 100.00,
  "description": "These amazing Kicks are made from recycled plastics and
  fabrics.They come in a variety of sizes and are completely unisex in design.
  If your feet don't like them within the first 30 days, we'll return your
  money no questions asked.
  ",
  "materials": [
    "recycled cotton",
    "recycled plastic",
    "recycled food waste",
  ],
  "country": "Russia",
  "image": "https:///www.shoesfromthegutter.com/kicks/AS45OPD.img",
  "available_sizes": {
      "mens": ["5", "6", "8", "8W", "10", "10W", "11", "11W", "12", "12W"],
      "womens": ["5", "6", "7", "8", "9", "10", "11", "12"]
  },
  "package_weight_kg": 2.00,
  "average_rating": 4.8,
  "top_five_reviews": [{
      "author": "i_love_kicks",
      "text": "best shoes ever! comfortable, awesome colors and design!",
      "rating": 5
    },
    {
      "author": "i_know_everything",
      "text": "These shoes are no good because I ordered the wrong size.",
      "rating": 1
    },
    "..."
  ],
  "top_five_questions": [{
      "author": "i_love_kicks",
      "text": "Do you guys make baby shoes?",
      "likes": 1223
    },
    {
      "author": "i_want_to_know_everything",
      "text": "How are these shoes made?",
      "likes": 1120
    },
    "..."
  ],
  "stock_amount": 10000,
}
```

Answer schema

```
{
  "_id": "<objectId>",
  "item_code": "<string>",
  "name": "<string>",
  "maker_brand": "<string>",
  "price": "<decimal>",
  "description": "<string>",
  "materials": ["<string>"],
  "country": "<string>",
  "image": "<string>",
  "available_sizes": {
    "mens": ["<string>"],
    "womens": ["<string>"]
  },
  "package_weight_kg": "<decimal>",
  "average_rating": "<decimal>",
  "top_five_reviews": [{
    "author": "<string>",
    "text": "<string>",
    "rating": "<int>"
  }],
  "top_five_questions": [{
    "author": "<string>",
    "text": "<string>",
    "likes":"<int>"
  }],
  "stock_amount": "<int>"
}
```   