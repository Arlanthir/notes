# DynamoDB

*(Mostly summarized from The DynamoDB Book by Alex DeBrie, author of the [DynamoDB Guide](https://www.dynamodbguide.com/))*

- NoSQL Database
  - Key-value (hash table) or wide-column store ("shelf of phone books", hash table of B-trees)
- Managed by Amazon in their cloud environment (AWS)
- HTTP API
- Authentication/permissions via IAM (AWS Identity and Access Management)
  - Automatically managed when accessed via other AWS applications (e.g. EC2 virtual machines or ECS containers)

## Local configuration

To follow the examples and try out DynamoDB on your machine, it is recommended that you use [LocalStack](https://github.com/localstack/localstack).

Make sure to install the awslocal wrapper: `pip3 install awscli-local`.

Verify that it's running with `localstack status services` (should display all the available services). The running port should be 4566.

You can also install [Dynobase](https://dynobase.dev/) to see an interactive visual representation of the DynamoDB state: `brew install --cask dynobase`. The free trial is about one week.

If you do, remember to configure the following settings:

- Offline settings > DynamoDB Local Port: `4566`
- Offline settings > DynamoDB Local Regions: `eu-central-1,localhost,local`

You may have to set your default Local region to `eu-central-1` as well:

Add to `~/.aws/config`:
```
[profile Local]
region=eu-central-1
```

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
  - **Scalar**: `string` (S), `number` (N), `binary` (B), `boolean` (BOOL), `null` (NULL).
  - **Document**: Groups of arbitrary nested attributes. `list` (L) and `map` (M).
  - **Set**: Multiple unique values. Sets of `string`, `number` and `binary`.
- **Primary key**: Mandatory unique key for each item.
  - **Simple**: A single element, called a **partition key** ("hash"). Enables operations on individual items.
  - **Composite**: Two elements: **partition key** ("hash") and **sort key** ("range"). Enables "fetch many" access patterns and helps managing relations between items.
- **Secondary index**: Optional reorganization of the data to enable more access patterns.
  - **Local**: Same **partition key** as the original data, different **sort key**. Can use *strong consistency* or *eventual consistency*. Must be created with the table.
  - **Global**: Any attribute as **partition key** and **sort key**. More flexible and frequently used. Can only use *eventual consistency*. Can be created or deleted after table creation.
- **Item collection**: Group of items that share the same partition key. When using a local secondary index, max 10GB per collection (includes main table and local secondary index).

### Advanced concepts

- **Streams**: Allows creating an event-driven log of DB writes that can be processed in a Lambda.
- **TTL**: Allows specifying a `number` attribute as a Unix timestamp after which the item will be automatically deleted (usually within 48h of being expired, so double-checking is needed).
- **Partition**: The database *shard* where data is stored. Items with the same partition key (item collections) are guaranteed to be stored in the same partition.

### Replication and consistency

DynamoDB has 3 nodes for each partition. The write operations always write to the primary node, which replicates it synchronously to a secondary node and asynchronously to another secondary node (not always the same one). Read operations can be eventually consistent (and thus directed to any node) or strongly consistent (directed to the primary node, at the cost of more billable throughput).

### Overloading keys

Typically, tables will have multiple entities. Therefore, partition and sort key names will frequently be the generic `PK` and `SK`. The values for the keys can be prefixed with the entity abbreviation, to help disambiguate and prevent collisions (e.g. `USER#a1b2c3`).
A table can therefore store Organization entities mapped to `PK:ORG#a1b2c3 SK:ORG#a1b2c3` (notice the repeating value as both PK and SK) and their users mapped to `PK:ORG#a1b2c3 SK:USER#d1e2f3`.

## Creating tables

DynamoDB is *schemaless*. This means that, other than the primary key attributes, you don't have to define any attributes or data types when you create tables. By comparison, relational databases require you to define the names and data types of each column when you create a table.

To create a table that organizes Actors and Movies, you can do the following:

```bash
awslocal dynamodb create-table \
    --table-name MoviesAndActors \
    --attribute-definitions \
        AttributeName=Actor,AttributeType=S \
        AttributeName=Movie,AttributeType=S \
    --key-schema \
        AttributeName=Actor,KeyType=HASH \
        AttributeName=Movie,KeyType=RANGE \
    --global-secondary-indexes \
        IndexName=MoviesIndex,KeySchema="[\
            {AttributeName=Movie,KeyType=HASH},\
            {AttributeName=Actor,KeyType=RANGE}\
        ]",Projection={ProjectionType=ALL} \
    --billing-mode=PAY_PER_REQUEST \
    --endpoint-url http://localhost:4566 \
    --region eu-central-1
```

To delete the above table:

```bash
awslocal dynamodb delete-table --table-name MoviesAndActors --region eu-central-1
```

The equivalent JSON payload would be:

```javascript
{
  "TableName": "MoviesAndActors",
  "KeySchema": [
    {
      "KeyType": "HASH",
      "AttributeName": "Actor"
    },
    {
      "KeyType": "RANGE",
      "AttributeName": "Movie"
    }
  ],
  "AttributeDefinitions": [
    {
      "AttributeName": "Actor",
      "AttributeType": "S"
    },
    {
      "AttributeName": "Movie",
      "AttributeType": "S"
    }
  ],
  "BillingMode": "PAY_PER_REQUEST",
  "GlobalSecondaryIndexes": [
    {
      "IndexName": "MoviesIndex",
      "Projection": {
        "ProjectionType": "ALL"
      },
      "KeySchema": [
        {
          "AttributeName": "Movie",
          "KeyType": "HASH"
        },
        {
          "AttributeName": "Actor",
          "KeyType": "RANGE"
        }
      ]
    }
  ]
}
```

## API

Contrary to SQL, where interactions with the database are made via textual SQL queries, interactions with Dynamo are made using libraries for the specific language of the client app (Go, JavaScript, etc).

### Item-based actions

Used to operate on a single item. The full primary key must be provided. Only available on the main table (not on secondary indexes). All write operations are item-based.

Operations:

- **GetItem**
- **PutItem** (will overwrite if the item already exists)
- **UpdateItem** (add/remove/alter properties on existing items. Can create item if it doesn't exist)
- **DeleteItem**

To save round trip requests, sequential item-based operations can be grouped in **batch actions** (can individually fail) and **transaction actions** (one fail rolls back the entire state).

#### PutItem

```typescript
import { DynamoDBClient, PutItemCommand } from '@aws-sdk/client-dynamodb'

const ddbClient = new DynamoDBClient({
  region: 'eu-central-1',
  endpoint: 'http://localhost:4566',
})

const command = new PutItemCommand({
  TableName: 'MoviesAndActors',
  Item: {
    Actor: { S: 'Tom Hanks' },
    Movie: { S: 'Toy Story' },
    Role: { S: 'Woody' },
    Year: { N: '1995' },
    Genre: { S: 'Drama' },
  },
})

try {
  await ddbClient.send(command)
} catch (e) {
  console.log(`DynamoDB Error: ${e}`)
}
```


### Query

Used to operate on item collections (same partition key). Available on main table and secondary indexes. Max 1MB before filter expressions, the rest will be paginated.

Can be used to fetch all related objects in one-to-many or many-to-many relationships.

Example query to get all movies (SK) in which an actor (PK) participates:

```python
items = client.query(
  TableName='MoviesAndActors',
  KeyConditionExpression='#actor = :actor',
  ExpressionAttributeNames={
    '#actor': 'Actor'
  },
  ExpressionAttributeValues={
    ':actor': { 'S': 'Tom Hanks' }
  }
)
```

We can further specify conditions over the SK to return a partial collection, provided the items are contiguous (i.e. `>=`, `<=`, `begins_with`, `between` are valid, but not `ends_with`). Example for movies with names beginning with letters between A and M:

```python
items = client.query(
  TableName='MoviesAndActors',
  KeyConditionExpression='#actor = :actor AND #movie BETWEEN :a AND :m',
  ExpressionAttributeNames={
    '#actor': 'Actor',
    '#movie': 'Movie'
  },
  ExpressionAttributeValues={
    ':actor': { 'S': 'Tom Hanks' },
    ':a': { 'S': 'A' },
    ':m': { 'S': 'M' }
  }
)
```

To query all actors of a given movie, we would have to create a global secondary index with the PK and SK flipped, and we could then do:

```python
items = client.query(
  TableName='MoviesAndActors',
  IndexName='MoviesIndex',
  KeyConditionExpression='#movie = :movie',
  ExpressionAttributeNames={
    '#movie': 'Movie'
  },
  ExpressionAttributeValues={
    ':movie': { 'S': 'Toy Story' }
  }
)
```


### Scan

Used to operate on the whole table. It's best to avoid, except on tables/indexes specifically created to be scanned or when migrating data to a different system. Max 1MB before filter expressions, the rest will be paginated.

### Optional request properties

- ConsistentRead `False` by default. Not available in global secondary indexes.
- ScanIndexForward: When `False`, treats the index in `descending` order (e.g. to find most recent timestamps). `True` by default. Available on Query operations.
- ReturnValues: Return item data when changing it (`ALL_OLD`, `UPDATED_OLD`, `ALL_NEW`, `UPDATED_NEW`). `NONE` by default. Available on write operations.
- ReturnConsumedCapacity: Return consumed capacity data (`TOTAL` or more detailed `INDEXES`).
- ReturnItemCollectionMetrics: Return the collection size on write operations to see how close you are to the 10GB limit.
