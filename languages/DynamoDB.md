# DynamoDB

*(Mostly summarized from The DynamoDB Book by Alex DeBrie, author of the [DynamoDB Guide](https://www.dynamodbguide.com/))*

- NoSQL Database
  - Key-value (hash table) or wide-column store ("shelf of phone books", hash table of B-trees)
- Managed by Amazon in their cloud environment (AWS)
- HTTP API
- Authentication/permissions via IAM (AWS Identity and Access Management)
  - Automatically managed when accessed via other AWS applications (e.g. EC2 virtual machines or ECS containers)

## When to use

DynamoDB reimagines databases in a world where storage is cheaper and there is increased need of performance.

It optimizes the operations on a single record (in detriment of *joins*) and relaxes the relational constraints (consistency).

Most useful for:

1. Hyper-scale applications
2. Hyper-ephemeral compute (serverless / lambdas)

## Concepts

- **Table**: A collection of data items using multiple data entities (as opposed to single entity relational tables).
- **Item**: A single record (equivalent to a *row* in relational tables). Max 400KB per item.
- **Attribute**: A single piece of data within an item. The equivalent of a column in a relational row. Not all pieces of data need Attributes.
  - **Scalar**: `string`, `number`, `binary`, `boolean`, `null`.
  - **Complex**: Groups of arbitrary nested attributes. `list` and `map`.
  - **Set**: Multiple unique values. Sets of `string`, `number` and `binary`.
- **Primary key**: Mandatory unique key for each item.
  - **Simple**: A single element, called a **partition key**. Enables operations on individual items.
  - **Composite**: Two elements: **partition key** and **sort key**. Enables "fetch many" access patterns and helps managing relations between items.
- **Secondary index**: Optional reorganization of the data to enable more access patterns.
  - **Local**: Same **partition key** as the original data, different **sort key**. Can use *strong consistency* or *eventual consistency*. Must be created with the table.
  - **Global**: Any attribute as **partition key** and **sort key**. More flexible and frequently used. Can only use *eventual consistency*. Can be created or deleted after table creation.
- **Item collection**: Group of items that share the same partition key. Max 10GB per collection.

### Advanced concepts

- **Streams**: Allows creating an event-driven log of DB writes that can be processed in a Lambda.
- **TTL**: Allows specifying a `number` attribute as a Unix timestamp after which the item will be automatically deleted (usually within 48h of being expired, so double-checking is needed).
- **Partition**: The database *shard* where data is stored. Items with the same partition key (item collections) are guaranteed to be stored in the same partition.

### Replication and consistency

DynamoDB has 3 nodes for each partition. The write operations always write to the primary node, which replicates it synchronously to a secondary node and asynchronously to another secondary node (not always the same one). Read operations can be eventually consistent (and thus directed to any node) or strongly consistent (directed to the primary node, at the cost of more billable throughput).

### Overloading keys

Typically, tables will have multiple entities. Therefore, partition and sort key names will frequently be the generic `PK` and `SK`. The values for the keys can be prefixed with the entity abbreviation, to help disambiguate and prevent collisions (e.g. `USER#a1b2c3`).

## API

### Query

Max 1MB before filter expressions.

### Scan

Max 1MB before filter expressions.
