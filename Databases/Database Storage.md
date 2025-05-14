#database-storage #files-and-pages

## Storage

We will focus on a "disk oriented" DBMS architecture that assumes the primary storage location of the database is non-volatile disk(s).

Non-volatile storage device is a type of storage device that does not require continuous power in order to retain its stored bits.

## Database System Design Goals

A high-level design goal of the DBMS is to support databases that exceed the amount of memory available. Since reading/writing to disk is expensive, disk use must be carefully managed. We do not want large stalls from fetching something from disk to slow down everything else. We want the DBMS to be able to process other queries while it is waiting to get the data from disk.

## Disk-Oriented DBMS Overview

The database is all on disk, and the data in database files is organised as files, which are  themselves organised into pages, with the first page being the [[Page Directory|directory page]]. To operate on the data, the DBMS needs to bring the data into memory. It does this by having a buffer pool that manages the data movement back and forth between disk and memory. The DBMS also has an execution engine that will execute queries. The execution engine will ask the buffer pool for a specific page, and the buffer pool will take care of bringing that page into memory and giving the execution engine a pointer to that page in memory. The buffer pool manager will ensure that the page is there while the execution engine operates on that part of memory.

## File Storage

The DBMS stores the database as files on disk. Some systems use a file hierarchy, others may use a single file. e.g. (SQLite)

The OS does not know anything about the contents of these files, since they are in their own proprietary format.

The Storage Manager is responsible for managing the database files, it represents files as a collection of pages. It also keeps track of what data has been read/written to pages as well as how much free space is there in these pages.

## Database Pages

The DBMS organises database across one or more files in fixed size blocks of data called pages. Pages can contain different types of data (tuples, indexes etc). Most systems will not mix different data types within a page, some systems require pages to be self-contained, meaning that all the information to interpret the data in the page is within that page itself.

Each page is given a unique identifier (page id). If the database is stored in a single file then the page id can just be the file offset, i.e offset = page_number * page_size. A page id can be unique per DBMS instance, per database or per table. Most systems usually have an indirection layer that maps page id to file path and offset. The upper levels of the system ask for a page id and indirection layer provides a specific file offset.

Most database systems will only use fixed length pages to not deal with the complexity overhead that comes with variable length pages.

There are three concepts of pages in DBMS:
- Hardware pages(4KB)
- OS Page (4KB)
- Database Page (512B - 16KB)

DBMS that specialise in read only work flows will have larger page sizes, this is because the storage device guarantees an atomic write of the size of the hardware page, meaning that if a system is writing to a 4KB hardware page the system guarantees that either all the data will be written or none of it will be written. Database Systems that use page sizes larger than that of the hardware page must have additional mechanisms to ensure the data gets written out safely. So if a database that primarily has read only operations uses smaller page sizes then it will have to perform more disk I/O's to do so, having a larger size reduced the disk I/O's since there are no atomicity concerns when reading data.

## Page Storage Architecture

Different databases store pages in a file in different ways: -
- Heap File Organisation
- Tree File Organisation
- Sequential / Sorted File Organisation (ISAM)
- Hashing File Organisation

### Heap File Organisation

There are a couple of ways to find the location of the page a DBMS wants on the disk, and heap file organisation is one of those ways.

A heap file is an unordered collection of pages where tuples are randomly stored in pages.

The DBMS can locate specific pages in a heap file by using two main strategies: -
- Linked List: The header page holds a pointer to a list of free pages and a list of data pages. However the DBMS has to do a sequential scan whenever it needs to find a specific page.
- Page Directory: The DBMS maintains a special page called a page directory, to keep track of the data pages, the amount of free space in each page, a list of free empty pages and the page type. These special pages have one entry for each database object. 

Page Directory is very widely used and is the technique we will focus on.

![[Page Directory.png]]

## Page Layout

Every page includes a header that contains the meta-data about the pages contents:
- Page size.
- Checksum.
- DBMS version.
- Transaction visibility.
- Self-containment. (Some systems like Oracle require this.)

The straw man approach is to keep track of the number of tuples in a page and then append any future tuples at the end of the page, this approach while simple introduces complexity when inserting tuples into pages after deletion of a tuple when working with variable length tuple. Since fixed length tuples only have limited uses we cannot use this approach for many applications.

There are two main approaches to laying out data in pages:
- Slotted Pages
- Log Structured

### Slotted Pages

This is the most common approach in today's DBMS. The page header keeps track of the number of used slots, the offset of the starting location of the last used slot and a slot array, which keeps track of the starting location of each tuple. 

To add a tuple the slot array will grow from beginning to the end while the tuples will grow from end to the beginning. The page is considered full when the slot array and tuple meet.

![[Slotted Array Page layout.png]]

![[Slotted Array Page Layout Slot and Tuple Growth.png]]
