---
layout: post
title:  "Redshift: Choosing the right column encodings"
date:   2016-09-16 11:10:00 -0700
categories: redshift aws optimizations
permalink: /redshift-encodings
---

This post will teach you how to choose encoding for your tables


# Adding columns encodings to Redshift tables

## What are encodings
Redshift provides the ability to add compression for data stored in Redshift. Using correct compression can speed up your ETL jobs by as much as 2-10x. Especially if you have VARCHAR and CHAR types used heavily, you could see a bigger boost in your table performance

## Example of adding encodings

Column compression can be enabled by adding encodings as part of create statement for a table. The following example demonstrates how to add encoding to your table

```

CREATE TABLE public.encoded_table_about_users (
    id              BIGINT                      ENCODE delta IDENTITY(1,1),
    user_id         VARCHAR(256)                ENCODE runlength,
    user_name       VARCHAR(256)                ENCODE lzo,
    user_zipcode    VARCHAR(256)                ENCODE bytedict,
    user_address    VARCHAR(1024)               ENCODE lzo,
    user_city       VARCHAR(256)                ENCODE lzo,
    created_at      TIMESTAMP WITHOUT TIME ZONE ENCODE lzo
)
DISTKEY(user_id)
SORTKEY(id);

```

# Choosing Encoding Types

### VARCHAR and CHAR

Note: Consider using VARCHAR(255) instead of VARCHAR(256) for your table definition since it supports TEXT255 encoding which is superior to BYTEDICT for VARCHAR

By Default, LZO Encoding for all VARCHAR, CHAR columns

* For VARCHAR(255) or less, if you expect less than 245 unique values then use TEXT255
* For VARCHAR(256) or more, if you expect less than 255 unique values use BYTEDICT
* For VARCHAR(any size), if you expect large number of unique values, use LZO

So, character dimension columns like ride_type, driver_mode_type, region that have fixed number of small values can use TEXT255 and BYTEDICT, every other VARCHAR can benefit from LZO by default.


### Numeric Types

* By default, no encoding is safe option.
* If you expect chains of same value occurring consecutively, then use RUNLENGTH. e.g. if a column contains values like 2, 2, 2, 2, 5, 5, 3, 3, 3, 1, 2, 4, 4, 4
* If you expect incrementing values like auto increment columns etc, DELTA encoding works great at compressing it. e.g. 1000, 1001, 1001, 1002, 1004, 1007
* Advanced: If you have defined a column as BIGINT but most values are INT size or less, you can use the MOSTLY32 type which will save half the space in storing that data. For understanding MOSTLY encoding, please refer to [Redshift MOSTLY encoding page](http://docs.aws.amazon.com/redshift/latest/dg/c_MostlyN_encoding.html)


### Timestamps

* For increasing timestamps like occurred_at, DELTA encoding works great since it stores the difference between current value and previous value. 

## Reference
[Redshift Encoding Guide](http://docs.aws.amazon.com/redshift/latest/dg/c_Compression_encodings.html)

