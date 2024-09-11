# dataset

This is a dataset of functionally equivalent (in short, FE) method pairs. This dataset includes 132 FE method pairs that have been validated.

# Quick Start

### Tables in ijadataset.db

There are three tables `methods`, `pairs`, and `verifiedpairs` in this dataset.

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
