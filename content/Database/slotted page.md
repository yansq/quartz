#TODO 

https://zhuanlan.zhihu.com/p/433654215

![[Database/attachments/Pasted image 20230130130031.png]]

Slotted-page is a common way to store data in databases. 

A slotted page has 3 parts:

- header
- slot array
- tuples(data)

The header stores metadata of the page, including the number of tuples, DBMS version number, size of the page, etc.

The slot array stores the offset(distance to the head of the page) and length of each tuple. The common size of each array's item is 4 bytes.

In a slot page, tuple stores from tail to head, while slot array stores from head to tail. When these two parts of data meet, means this page is already full and can't store more data. It's a smart design to solve the problem of storing tuples with dynamic length fields.