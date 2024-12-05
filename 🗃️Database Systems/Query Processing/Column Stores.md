A traditional transactional database (OLTP) is characterized by:

1. Lots of writes/updates.
2. Reads of individual records.

By contrast, an analytical / reporting database (OLAP) is characterized by:

1. Lots of reads of many records.
2. Bulk updates.
3. Typical query touches only a few columns.

Transactional databases do well with row stores, while analytical databases do well with column stores. For example, the Python data analysis library `pandas` stores its data via columns of the same type which are stored contiguously.

The translation from transactional to analytical is done via the ==extract, transform, load (ETL)== process (also called ELT for the slightly different order).

## Designs

Often, to power analytical workloads, you don't care so much about [[Database Normalization|removing redundancy]]. The data is static so you can guarantee correctness, and your goal is to just perform queries quickly.

> [!example] (Star schema)
> One method is via the ==star schema==, where you have a central ==fact table== that logs the central "unit" of data, e.g. a purchase in the store. Then, you have various ID's like `customer_id` among the columns of the fact table which can be joined to other surrounding tables with them as the primary key, in order to retrieve relevant data.

You can take this further to achieve a ==snowflake schema==, which allows further branching.

## Benefits

One of the main benefits is the performance increase when you don't have to read every column! The columns will be contiguous in memory so you can access a small fraction of the full table!

Further, you can have very wide tables where often rows are just sparse.

Another cool optimization is called ==late materialization==, where you have to perform multiple selections on the rows. In this case, you can first work with the bitmasks of the rows rom each selection in the columns, and then AND then together before actually doing the positional lookups.

## Compression

This is another very nice benefit of column stores. Here are some ideas:

* **Run-length encoding (RLE):** if values are repeated a lot in runs, store the value and its run length. Work well for mostly-sorted, few-valued columns. ^736daa
* **Delta encoding:** store the difference from the last entry. 
* **Lempel-Ziv encoding (LZ):** general-purpose lossless compression.
* **Bit packing:** each value becomes a length in bits followed by the value.
* **Bitmap encoding:** if you have only a few values, assign bits to them (e.g. Huffman coding). Bitmaps can be further compressed, e.g. using RLE.

Finally, you can make the operators compression-aware so that they are more efficient!

## Dealing with Inserts

This is the main problem with column stores. One idea is to have a fast row store where you take in inserts, and then have batched asynchronous data movement into the column stores. Of course, for queries, you will have to now touch the row store too.

## Parquet Format

You've seen this in compressing `pandas` data frames. It splits the data into row groups, where within each group everything is stored in column format. I guess the row groups allow better insertion properties. 