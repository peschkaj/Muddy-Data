# Modeling Muddy Data #

## Before We Get Started ##

Before getting started breaking all of the rules, it's important that we know what the rules actually are. This is where database normalization comes in to play.

## Normalization ##

Normalization is the process of organizing data and reducing data duplication.

### Why Normalize? ###

#### Remove Anomalies ####

We want to free the database of modification anomalies. If my home address is 742 Evergreen Terrace, it should always be 742 Evergreen Terrace and there should be no way for it to be anything else. 

#### Create Extensible Design ####

By properly normalizing the database we make it possible to quickly and easily extend the database as we add new features and track additional data. Not only will a normalized schema make it easy to add new features, but adding new features will not break existing functionality or cause problems for existing data.

#### Easy Mode Engage ####

A normalized database should effectively model real world concepts, how they relate, and will make this apparent to the user through the schema. 

#### Query, Query, Query #### 

The reason to store data is to query it. If we always knew how people were going to be querying our data, it would be very easy to store the data: we could aggregate it at the level and then persist to disk. One of the strong points of normalized relational databases is their support for flexible ad hoc querying. Even more important, well normalized databases avoid querying bias. That is: a well normalized database makes it possible to run queries that the database designer could never have dreamed up.

### The Normal Forms ###

Understanding how data should be properly normalized is important for several reasons. It gives us a baseline for data quality. We know that we have a well thought out design when it is fully normalized. Once we have a solid understanding of normalization, it's easier to find places where that normalization isn't as necessary as we once thought. You have to know the rules in order to break them. 

#### First Normal Form ####

First normal form essentially has three rules:

1. All attributes are atomic. Columns contain one and only one value.
2. All rows are unique.
3. Every row has the same number of columns.

#### Second Normal Form ####

1. Everything conforms to first normal form.
2. Every row describes just one thing.

#### Third Normal Form ####

1. Everything conforms to first and second normal form.
2. Columns only describe the key, not other columns.

## What is Muddy Data? ##

This is data that doesn't fit into a normalized schema. You could also say that it's something that isn't easily stored **and** retrieved from third normal form. This might be complex attributes from an object model, user sessions, or user specified properties.

Even though there are many ways to do this, we're going to take a look at three different ways to model and store this muddy data.

### Entity Attribute Value ###

An Entity Attribute Value (EAV) database tracks relevant facts; we only record the facts that we need. This model is most effective when you have a large number of potential attributes that will not be recorded with each row. Data is recorded as three columns: 

  * *entity* - the described object
  * *attribute* - the attribute, or a foreign key into a table of attribute definitions
  * *value* - the value being tracked

An attribute table will contain meta data about the attribute - an ID, name, description, data type, and validation rules for the attribute value. The potential complexity of an attribute table is occasionally mentioned as a reason to avoid the EAV model.

Another potential problem with EAV schemas is called impedance mismatch. The logical and physical schemas are radically different. Even though objects are logically modeled as containing collections of typed objects, they are stored in the database as an association of loosely typed objects with rich meta data.

> Meta data is key to the successful operation of an EAV database. 

### Polymorphic Associations ###

Polymorphic associations are different enough from EAVs to warrant their own discussion. While an EAV database tracks properties of an object with meta data, polymorphic associations track the relationships between objects with meta data.

The most readily visible example of this is an application that supports rich tagging. In this hypothetical application we have many different objects - people, orders, physical facilities, and products. Each of these objects can be tagged with multiple tags. While it's possible to create single tag tables for each (e.g. PeopleTags and ProductTags), it is potentially better to create a single Taggables table that looks something like this:

    CREATE TABLE taggables (
        taggable_id BIGINT NOT NULL,
        object_id BIGINT NOT NULL,
        object_type VARCHAR(50) NOT NULL,
        tag VARCHAR(50) NOT NULL
    );



This Work is licensed under a [Creative Commons Attribution-ShareAlike 3.0 Unported License](http://creativecommons.org/licenses/by-sa/3.0/).
