# PyFuncEquivDataset

PyFuncEquivDataset is a dataset of functionally equivalent (in short, FE) method pairs. This dataset includes 130 FE method pairs in python that have been validated.

## Quick start

The size of this dataset is very large, approximately 3.14 GB.

Then, please make sure that SQLite is installed in your environment.

```shell-session
$sqlite3 PyFuncEquivDataset.db
SQLite version 3.43.2 2023-10-10 13:08:14
Enter ".help" for usage hints.
sqlite>
```

### Tables in PyFuncEquivDataset.db

There are three tables `methods`, `pairs`, and `verifiedpairs` in this dataset.

```shell-session
sqlite> .table
methods        pairs          verifiedpairs
```

Of those three tables, table `verifiedpairs` includes information on FE method pairs.


### Table `verifiedpairs`

The schema of table `verifiedpairs` is as follows.

```shell-session
sqlite> .schema verifiedpairs
CREATE TABLE verifiedpairs (
    pairid    INTEGER
, reviewer INTEGER);
```

- `pairid` means the unique identifier for the method pair.
- `reviewer` represent the results of manual checking by the reviewer, where `1` indicates functional equivalence and `0` indicates functional non-equivalence.


You can get the number of FE method pairs that have been validated by the reviewer with the following command.

```shell-session
sqlite> select count(*) from verifiedpairs where reviewer = 1;
130
```

The following command enables you to see the source code of FE method pairs that have been validated by the reviewer.

```shell-session
sqlite> select (select rtext from methods where id = (select leftMethodID from pairs P where P.id = V.pairid)), (select rtext from methods where id = (select rightMethodID from pairs P where p.id = V.pairid)) from verifiedpairs V where reviewer = 1;
```


## Detailed Information

### Table `methods`

Table `methods` includes various information related to methods.
The schema of table `methods` is as follows.
```shell-session
sqlite> .schema methods
CREATE TABLE methods (
    signature                 STRING,
    name                      STRING,
    rtext                     BLOB,
    ntext                     BLOB,
    size                      INT,
    branches                  INT,
    hash                      BLOB,
    path                      STRING,
    start                     INT,
    end                       INT,
    repo                      STRING,
    revision                  STRING,
    compilable                INT,
    tests                     INT,
    Target_ESTest             BLOB,
    Target_ESTest_scaffolding BLOB,
    groupID                   INT,
    id                        INTEGER PRIMARY KEY AUTOINCREMENT
);
```
- `signature` represents the declaration of the method.
- `name` represents the name of the method.
- `rtext` represents the raw text of the method.
- `ntext` represents the normalized text of the method.
- `size` represents the number of program statements included in the method.
- `branches` represents the number of branches included in the method.
- `hash` represents the MD5 hash value of the normalized text of the method.
- `path` represents the path to the file including the method.
- `start` and `end` represent the start/end line of the method in the file.
- `repo` represent which open-source project this method comes from.
- `revision` is not used in this dataset.
- `compilable` The method used to perform the mutual execution step is set to `1`, otherwise it is set to `0`.
- `tests` is not used in this dataset.
- `Target_ESTest` is the set of test cases that Pynguin generated for the method.
- `Target_ESTest_scaffolding` is the set of test cases that are processed and used for mutual execution.
- `groupID` is the grouping result of 'groupID' after type inference.
- `id` represents the unique identifier of the method.

Of the above items `revision` and `tests` are not used in this dataset.
So, all users of this dataset can ignore values in that items.


### Table `pairs`

Table `pairs` includes a list of candidates FE method pairs.
The schema of table `pairs` is as follows.
```shell-session
sqlite> .schema pairs
CREATE TABLE pairs (
    leftMethodID  INT,
    rightMethodID INT,
    id            INTEGER PRIMARY KEY AUTOINCREMENT
);
```

- `leftMethodID` and `rightMethodID` represent the identifiers of the two methods that form the pair. `leftMethodID/rightMethodID` are common to `id` in table `methods`.
- `id` is the unique identifier of this pair.

For example, you can obtain the raw code of method pairs that are candidates of functionally equivalent ones with the following command.

```shell-session
sqlite> select (select M1.rtext from methods M1 where M1.id = p.leftMethodID), (select M2.rtext from methods M2 where M2.id = p.rightMethodID) from pairs P;
```

Herein, each candidate of FE method pairs satisfies the following conditions.
- Let `Method-A` and `Method-B` be the two methods that forms the pair. `Method-A` passes all test cases generated from `Method-B` and `Method-B` passes all test cases generated from `Method-A`.
- The test cases generated by all method in pairs achieve 100% branch coverage.








