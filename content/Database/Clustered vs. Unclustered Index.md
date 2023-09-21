#TODO 

> 聚集索引 vs. 非聚集索引

![[Database/attachments/CleanShot 2023-02-06 at 12.45.08@2x.png]]

One table can only have one clustered index. The speed of clustered index is faster than the unclustered index, and clustered index is efficient for **range searches**.

To build a clustered index, first sort the heap file

- Leave some free space on each block for future inserts
- Index entries direct search for data entries

When inserting lots of data into a clustered index, data may be inserted into the **blocks at end of the file**. So the order of data is "**close to**", but not identical to, the sort order.