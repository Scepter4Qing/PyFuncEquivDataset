# PyFuncEquivDataset

PyFuncEquivDataset is a dataset of functionally equivalent (in short, FE) method pairs. This dataset includes 130 FE method pairs in python that have been validated.

## Quick start

The size of this dataset is very large, approximately 3.14 GB.

### Tables in PyFuncEquivDataset.db

There are three tables `methods`, `pairs`, and `verifiedpairs` in this dataset.

```shell-session
sqlite> .table
methods        pairs          verifiedpairs
```

Of those three tables, table `verifiedpairs` includes information on FE method pairs.

### Table `methods`

Table `methods` includes various information related to methods.
The schema of table `methods` is as follows.

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
- `compilable` The method used to perform the mutual execution step is set to ‘1’, otherwise it is set to ‘0’.
- `Target_ESTest` is the set of test cases that Pynguin generated for the method.
- `Target_ESTest_scaffolding` is the set of test cases that are processed and used for mutual execution.
- `groupID` is the grouping result of 'groupID' after type inference.
- `id` represents the unique identifier of the method.

Of the above items `revision` are not used in this dataset.
So, all users of this dataset can ignore values in that items.


### Table `pairs`

Table `pairs` includes a list of method pairs that are candidates of FE method pairs.

### Table `verifiedpairs`

The schema of table `verifiedpairs` is as follows.

```shell-session
sqlite> .schema verifiedpairs
CREATE TABLE verifiedpairs (
    pairid    INTEGER
, reviewer INTEGER);
```

- `pairid` means the unique identifier for the method pair.
- `reviewera`, `reviewerb`, and `reviewerc` mean that they represent the judgement results that were individually confirmed by each reviewer. `1` means functionally equivalent and `0` means not functionally equivalent.
- `consensus` means the final decision result. If all three reviewers gave `1` or `0`, then `consensus` is equal to that value. If there was a difference between the three reviewers' judgements, they had a discussion about the method pair, and `consensus` represent the result of that discussion.

You can get the number of FE method pairs that have been validated by the three reviewers with the following command.

- `leftMethodID` and `rightMethodID` represent the identifiers of the two methods that form the pair. `leftMethodID/rightMethodID` are common to `id` in table `methods`.
- `id` is the unique identifier of this pair.


