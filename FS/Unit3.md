# UNIT III – Introduction to MongoDB and Mongoose
<img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/ac6f2a61-ba46-447c-b8f0-97c50f0f1179" />

## Complete Notes for Full Stack Development

---

## Table of Contents

1. [Introduction to NoSQL](#1-introduction-to-nosql)
2. [Introduction to MongoDB](#2-introduction-to-mongodb)
3. [MongoDB CRUD Operations](#3-mongodb-crud-operations)
4. [Introduction to Mongoose](#4-introduction-to-mongoose)
5. [Mongoose CRUD Operations](#5-mongoose-crud-operations)
6. [Extending Mongoose Schemas](#6-extending-mongoose-schemas)
7. [Important Exam Questions](#7-important-exam-questions)

---

## 1. Introduction to NoSQL

### 1.1 The Relational Database Era

**Standard Approach:**
- Web applications traditionally used relational databases (SQL) for persistent data storage
- Data stored in normalized tables
- Tables linked by primary and foreign keys
- Developers used SQL and ORMs (Object-Relational Mappers) to manage data

**Problems with Relational Databases:**
- Scaling problems as the web grew
- Difficulty in horizontal scaling
- Complex queries for hierarchical data
- Performance issues with large datasets

### 1.2 The Rise of NoSQL

**Why NoSQL?**

NoSQL databases emerged to address relational database limitations:

| Feature | Relational DB | NoSQL DB |
|---------|--------------|----------|
| **Scaling** | Vertical (more powerful hardware) | Horizontal (more servers) |
| **Availability** | Limited by single server | High availability through distribution |
| **Query Complexity** | Complex joins required | Simple querying |
| **Schema** | Rigid, predefined schema | Flexible, dynamic schema |
| **Data Model** | Tables with rows/columns | Documents, key-value, column, object |

**NoSQL Design Patterns:**

1. **Key-Value Stores** - Simple key-value pairs - redis
2. **Column Stores** - Data stored in columns -  cassandra
3. **Object Stores** - Object-oriented data storage - s3
4. **Document Stores** - Most popular, stores hierarchical documents - Mongodb

### 1.3 Relational vs Document Databases

```mermaid
graph TB
    subgraph Relational["Relational Database"]
        A[Users Table] --> B[Posts Table]
        B --> C[Comments Table]
    end
    
    subgraph Document["Document Database"]
        D[Single Document] --> E[User + Posts + Comments]
    end
```

**Relational Database Example:**

```
Users Table:
| user_id | name  | email          |
|---------|-------|----------------|
| 1       | Alice | alice@mail.com |

Posts Table:
| post_id | user_id | title       |
|---------|---------|-------------|
| 101     | 1       | First Post  |

Comments Table:
| comment_id | post_id | text        |
|------------|---------|-------------|
| 501        | 101     | Great post! |
```

**Document Database Example:**

```json
{
  "_id": ObjectId("52d02240e4b01d67d71ad577"),
  "name": "Alice",
  "email": "alice@mail.com",
  "posts": [
    {
      "post_id": 101,
      "title": "First Post",
      "comments": [
        {
          "comment_id": 501,
          "text": "Great post!"
        }
      ]
    }
  ]
}
```

**Key Differences:**

| Aspect | Relational | Document |
|--------|-----------|----------|
| **Data Storage** | Separate connected tables | Hierarchical documents (JSON/XML) |
| **Data Retrieval** | Multiple table joins required | Single document retrieval |
| **Object Reconstruction** | Application reconstructs from multiple tables | Entire object in single document |
| **Flexibility** | Rigid schema | Dynamic schema |

### 1.4 NoSQL Use Cases

**Best suited for:**
- Real-time web applications
- Big data and analytics
- Content management systems
- Mobile applications
- IoT data storage
- Social networks
- E-commerce product catalogs

---

## 2. Introduction to MongoDB

### 2.1 History and Background

**Formation (2007):**
- Founded by Dwight Merriman and Eliot Horowitz
- Company: 10gen
- Goal: Platform for hosting web applications with simplified hardware management

**MongoDB Origin:**
- Name derived from "humongous"
- Reflects ability to support complex data storage
- Released as open-source project
- Became one of fastest-growing databases

**Industry Adoption:**
- Major companies: eBay, The New York Times
- Proved viability beyond simple storage layer
- Extensive driver support for popular programming languages
- Rich ecosystem of administration tools

### 2.2 What is MongoDB?

**Definition:**
- MongoDB is a document-based NoSQL database
- Part of the MERN stack (MongoDB, Express, React, Node.js)
- Database layer of modern web applications

**Document Database:**
- Equivalent of a record is a document (object)
- Entire object written as single document
- No need to split data across multiple tables

**Comparison:**

| Database Type | Data Organization |
|--------------|-------------------|
| **Relational** | Rows and columns in tables |
| **Document (MongoDB)** | Objects as documents |

### 2.3 MongoDB Core Concepts

#### 2.3.1 MongoDB Documents

**Definition:**
- Data structure composed of field and value pairs
- Similar to JSON objects
- Easy to think of as JavaScript objects

**Document Structure:**

```json
{
  "_id": ObjectId("52d02240e4b01d67d71ad577"),
  "title": "First Blog Post",
  "author": "John Doe",
  "content": "This is my first blog post",
  "tags": ["mongodb", "nosql", "database"],
  "comments": [
    {
      "user": "Alice",
      "text": "Great post!",
      "date": ISODate("2024-01-15")
    }
  ],
  "createdAt": ISODate("2024-01-10"),
  "likesCount": 25
}
```

**Supported Data Types:**

| Data Type | Description | Example |
|-----------|-------------|---------|
| **Boolean** | true/false values | `isActive: true` |
| **Number** | Integer, float, double | `age: 25, price: 99.99` |
| **String** | Text data | `name: "John"` |
| **Date** | Date and time | `ISODate("2024-01-15")` |
| **Timestamp** | Special timestamp type | `Timestamp(1699564800, 1)` |
| **Regular Expression** | Pattern matching | `/^test/i` |
| **Binary Data** | Binary data storage | Images, files |
| **Array** | List of values | `[1, 2, 3]` |
| **Object/Embedded Document** | Nested objects | `{city: "Bengaluru"}` |

#### 2.3.2 MongoDB Collections

**Definition:**
- Collection is like a table in relational database
- Set of documents
- Can have primary key and indexes

**Key Features:**

| Feature | Description |
|---------|-------------|
| **Primary Key** | Mandatory field: `_id` |
| **Auto-indexing** | `_id` field automatically indexed |
| **Dynamic Schema** | Documents in same collection can have different fields |
| **No Joins** | Data embedded in documents |

**Example Collection:**

```javascript
// posts collection
[
  {
    "_id": ObjectId("...1"),
    "title": "First Post",
    "author": "John"
  },
  {
    "_id": ObjectId("...2"),
    "title": "Second Post",
    "author": "Alice",
    "category": "Tech" // Additional field
  }
]
```

#### 2.3.3 MongoDB Databases

**Definition:**
- Logical grouping of many collections
- Each database has its own set of files
- Single MongoDB server can have multiple databases

**Example Structure:**

```
MongoDB Server
├── Database: blog
│   ├── Collection: posts
│   ├── Collection: users
│   └── Collection: comments
├── Database: ecommerce
│   ├── Collection: products
│   ├── Collection: orders
│   └── Collection: customers
```

#### 2.3.4 MongoDB Query Language

**Definition:**
- Made up of methods to achieve various operations
- JSON-like syntax for queries
- No SQL statements required

**Example Query:**

```javascript
// Find documents where invoiceNumber > 1000
{ "invoiceNumber": { $gt: 1000 } }

// Find posts by specific user
{ "author": "John Doe" }

// Complex query with multiple conditions
{ 
  "status": "published",
  "views": { $gte: 100 },
  "tags": { $in: ["mongodb", "database"] }
}
```

### 2.4 MongoDB Shell Basics

#### 2.4.1 Starting MongoDB Shell

**Installation Check:**

```bash
$ mongo
MongoDB shell version v4.0.2
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 4.0.2
>
```

**The Mongo Shell:**
- Interactive JavaScript shell
- Similar to Node.js shell
- Directly execute JavaScript code
- Test queries and operations

#### 2.4.2 Basic Shell Commands

**Show Databases:**

```javascript
> show databases
admin    0.000GB
config   0.000GB
local    0.000GB
blog     0.005GB
```

**Check Current Database:**

```javascript
> db
test  // Default database
```

**Switch Database:**

```javascript
> use blog
switched to db blog
```

**Show Collections:**

```javascript
> show collections
posts
users
comments
```

### 2.5 Key Features of MongoDB

#### 2.5.1 BSON Format

**Binary JSON (BSON):**
- Binary-encoded serialization of JSON-like documents
- More efficient in size and speed
- Enables MongoDB's high read/write throughput

**BSON vs JSON:**

| Feature | JSON | BSON |
|---------|------|------|
| **Format** | Text-based | Binary-encoded |
| **Size** | Larger | Smaller, more efficient |
| **Speed** | Slower parsing | Faster parsing |
| **Data Types** | Limited (string, number, boolean, null, array, object) | Extended (Date, Binary, ObjectId, etc.) |

**BSON Document Example:**

```javascript
{
  "_id": ObjectId("52d02240e4b01d67d71ad577"), // Unique identifier
  "title": "First Blog Post",
  "comments": [ ... ]
}
```

**ObjectId Structure:**

The `_id` field uses ObjectId which consists of:

```mermaid
graph LR
    A[ObjectId - 12 bytes] --> B[4 bytes: Timestamp]
    A --> C[3 bytes: Machine ID]
    A --> D[2 bytes: Process ID]
    A --> E[3 bytes: Counter]
```

| Component | Size | Description |
|-----------|------|-------------|
| **Timestamp** | 4 bytes | Seconds since Unix epoch |
| **Machine Identifier** | 3 bytes | Unique machine identifier |
| **Process ID** | 2 bytes | Process identifier |
| **Counter** | 3 bytes | Random starting value counter |

**Benefits of ObjectId:**
- Globally unique across distributed systems
- Contains timestamp (creation time)
- No central coordination needed
- Sortable by creation time

#### 2.5.2 Ad Hoc Queries

**Definition:**
- Database responds to dynamically structured queries
- No need to predefine each query
- Achieved through indexing BSON documents
- Uses unique query language

**SQL vs MongoDB Comparison:**

**SQL Example:**

```sql
SELECT * FROM Posts WHERE Title LIKE '%mongo%';
```

**MongoDB Equivalent:**

```javascript
db.posts.find({ title: /mongo/ });
```

**More Query Examples:**

| Operation | SQL | MongoDB |
|-----------|-----|---------|
| **Equality** | `WHERE name = 'John'` | `{ name: 'John' }` |
| **Greater Than** | `WHERE age > 25` | `{ age: { $gt: 25 } }` |
| **IN Operator** | `WHERE status IN ('active', 'pending')` | `{ status: { $in: ['active', 'pending'] } }` |
| **AND Condition** | `WHERE age > 25 AND city = 'NYC'` | `{ age: { $gt: 25 }, city: 'NYC' }` |
| **OR Condition** | `WHERE age > 25 OR city = 'NYC'` | `{ $or: [{ age: { $gt: 25 } }, { city: 'NYC' }] }` |

**Complex Query Example:**

```javascript
// Find published posts with more than 100 views by specific authors
db.posts.find({
  status: "published",
  views: { $gte: 100 },
  author: { $in: ["John", "Alice"] },
  tags: { $all: ["mongodb", "tutorial"] }
});
```

#### 2.5.3 Indexing

**Definition:**
- Unique data structures enabling efficient query resolution
- Maps document fields for faster searches
- Reduces need to scan entire collection

**Without Index:**

```mermaid
graph LR
    A[Query] --> B[Scan Doc 1]
    B --> C[Scan Doc 2]
    C --> D[Scan Doc 3]
    D --> E[... Scan All Docs]
    E --> F[Return Results]
```

**With Index:**

```mermaid
graph LR
    A[Query] --> B[Check Index]
    B --> C[Direct Access to Matching Docs]
    C --> D[Return Results]
```

**Example Scenario:**

**Document Structure:**

```javascript
{
  "_id": ObjectId("52d02240e4b01d67d71ad577"),
  "title": "First Blog Post",
  "comments": [ ... ],
  "commentsCount": 12
}
```

**Query Without Index:**

```javascript
// Scans all documents
db.posts.find({ commentsCount: { $gt: 10 } });
// Slow for large collections
```

**Creating an Index:**

```javascript
// Create index on commentsCount field
db.posts.createIndex({ commentsCount: 1 }); // 1 = ascending
```

**Query With Index:**

```javascript
// Uses index for fast lookup
db.posts.find({ commentsCount: { $gt: 10 } });
// Much faster execution
```

**Index Types:**

| Index Type | Description | Example |
|-----------|-------------|---------|
| **Single Field** | Index on single field | `{ age: 1 }` |
| **Compound** | Index on multiple fields | `{ age: 1, name: 1 }` |
| **Multikey** | Index on array fields | `{ tags: 1 }` |
| **Text** | Full-text search | `{ content: "text" }` |
| **Geospatial** | Location-based queries | `{ location: "2dsphere" }` |

**Index Performance Impact:**

```
Without Index:
- Query Time: O(n) - scans all documents
- For 1 million docs: ~1000ms

With Index:
- Query Time: O(log n) - uses B-tree structure
- For 1 million docs: ~10ms
```

#### 2.5.4 Replica Set

**Definition:**
- Architecture for data redundancy and improved availability
- Set of MongoDB services hosting same dataset
- Protects data from hardware failure
- Increases read capacity

**Replica Set Components:**

| Component | Role | Capabilities |
|-----------|------|--------------|
| **Primary** | Main write node | Handles all write operations, reads |
| **Secondary** | Replica nodes | Handles read operations, replicates data |
| **Arbiter** | Voting node | No data, participates in elections only |

**Basic Replica Set Architecture:**

```mermaid
graph TD
    A[Client Application] -->|Writes| B[Primary]
    A -->|Reads| B
    A -->|Reads| C[Secondary 1]
    A -->|Reads| D[Secondary 2]
    
    B -->|Replicates| C
    B -->|Replicates| D
    
    C -.Heartbeat.-> B
    D -.Heartbeat.-> B
```

**Replica Set Workflow:**

1. **Write Operation:**
   - Client sends write to Primary
   - Primary writes to its dataset
   - Primary sends operation to Secondaries
   - Secondaries apply the operation

2. **Read Operation:**
   - Can read from Primary or Secondary
   - Configured through read preferences

3. **Automatic Failover:**
   - If Primary fails, Secondaries elect new Primary
   - System remains available

**Replica Set with Arbiter:**

```mermaid
graph TD
    A[Primary] -.Replication.-> B[Secondary]
    A -.Replication.-> C[Arbiter]
    
    B -.Election Vote.-> C
    C -.Election Vote.-> B
```

**Arbiter Characteristics:**
- Does not maintain any data
- Participates in Primary election
- Maintains quorum in replica set
- Cannot become Primary
- Cannot function as Secondary
- Lightweight, minimal resources

**Benefits of Replica Sets:**

| Benefit | Description |
|---------|-------------|
| **High Availability** | Automatic failover if Primary fails |
| **Data Redundancy** | Multiple copies of data |
| **Read Scaling** | Distribute reads across Secondaries |
| **Disaster Recovery** | Backup copies in different locations |
| **Zero Downtime** | Maintenance without service interruption |

#### 2.5.5 MongoDB Sharding

**Definition:**
- Process of splitting data between different machines (shards)
- Enables horizontal scaling
- Each shard holds portion of data
- Collection of shards forms single logical database

**Scaling Approaches:**

| Approach | Description | Implementation |
|----------|-------------|----------------|
| **Vertical Scaling** | Add more power to single server | Upgrade CPU, RAM, Storage |
| **Horizontal Scaling** | Add more servers | Sharding across multiple machines |

**Sharding Architecture:**

```mermaid
graph TB
    A[Application] --> B[Query Router - mongos]
    B --> C[Config Servers]
    
    C -.Metadata.-> B
    
    B --> D[Shard 1]
    B --> E[Shard 2]
    B --> F[Shard 3]
    
    D --> G[Replica Set 1]
    E --> H[Replica Set 2]
    F --> I[Replica Set 3]
```

**Sharding Components:**

| Component | Role | Description |
|-----------|------|-------------|
| **Shard** | Data partition | Each holds subset of data, functions as separate database |
| **Query Router (mongos)** | Request handler | Routes queries to appropriate shards |
| **Config Servers** | Metadata storage | Store cluster metadata and configuration |

**Sharding Process:**

1. **Data Distribution:**
   - Data split based on shard key
   - Each shard contains unique subset

2. **Query Routing:**
   - Application sends query to mongos
   - mongos consults config servers
   - Query routed to relevant shard(s)

3. **Result Aggregation:**
   - mongos collects results from shards
   - Returns combined results to application

**Shard Key Selection:**

```javascript
// Example: Sharding by user region
sh.shardCollection("app.users", { region: 1 });

// Documents distributed based on region
// Shard 1: { region: "Asia" }
// Shard 2: { region: "Europe" }
// Shard 3: { region: "Americas" }
```

**Benefits of Sharding:**

| Benefit | Description |
|---------|-------------|
| **Horizontal Scalability** | Add more shards as data grows |
| **Improved Performance** | Parallel query execution |
| **No Single Point of Failure** | Data distributed across multiple servers |
| **Geographic Distribution** | Shards in different regions |
| **Workload Distribution** | Balance load across shards |

**When to Use Sharding:**

- Dataset exceeds storage capacity of single server
- Working set exceeds RAM of single server
- Write throughput too high for single server
- Need to distribute data geographically

---

## 3. MongoDB CRUD Operations

### 3.1 Overview of CRUD

**CRUD Operations:**
- **C**reate - Insert new documents
- **R**ead - Query and retrieve documents
- **U**pdate - Modify existing documents
- **D**elete - Remove documents

**Basic Syntax Pattern:**

```javascript
db.collectionName.operation(query, data, options);
```

### 3.2 Creating Documents

#### 3.2.1 Creating with insert()

**Syntax:**

```javascript
db.collection.insert(document)
db.collection.insert([document1, document2, ...]) // Batch insert
```

**Single Document Insert:**

```javascript
db.posts.insert({
  "title": "Second Post",
  "user": "alice"
});

// Output: WriteResult({ "nInserted" : 1 })
```

**Multiple Documents Insert:**

```javascript
db.posts.insert([
  {
    "title": "First Post",
    "user": "alice",
    "content": "Hello MongoDB"
  },
  {
    "title": "Second Post",
    "user": "bob",
    "content": "Learning CRUD"
  }
]);

// Output: BulkWriteResult({ "nInserted" : 2 })
```

**With Specific _id:**

```javascript
db.posts.insert({
  "_id": "custom-id-123",
  "title": "Custom ID Post",
  "user": "alice"
});
```

**Key Points:**
- Automatically generates `_id` if not provided
- Returns WriteResult with operation status
- Throws error if `_id` already exists

#### 3.2.2 Creating with update()

**Using upsert Flag:**

**Syntax:**

```javascript
db.collection.update(
  query,           // Selection criteria
  update,          // Document or update operators
  { upsert: true } // Create if not exists
);
```

**Example:**

```javascript
db.posts.update(
  { "user": "alice" },                        // Query
  { "title": "Second Post", "user": "alice" }, // New document
  { upsert: true }                             // Create if not found
);

// If document exists: updates it
// If document doesn't exist: creates new document
```

**With Update Operators:**

```javascript
db.posts.update(
  { "user": "alice" },
  { 
    $set: { "title": "Updated Title" },
    $inc: { "views": 1 }
  },
  { upsert: true }
);
```

**Key Points:**
- `upsert: true` creates document if query matches nothing
- `upsert: false` (default) only updates existing documents
- Useful for "create or update" logic

#### 3.2.3 Creating with save()

**Syntax:**

```javascript
db.collection.save(document)
```

**Example:**

```javascript
db.posts.save({
  "title": "Second Post",
  "user": "alice"
});

// Creates new document if _id not present
// Updates existing document if _id matches
```

**Behavior Comparison:**

| Method | Behavior |
|--------|----------|
| **insert()** | Always creates new document, error if `_id` exists |
| **save()** | Creates if no `_id`, updates if `_id` exists |
| **update() with upsert** | Updates if query matches, creates otherwise |

**Example with _id:**

```javascript
// First save - creates document
db.posts.save({
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "title": "Original Title"
});

// Second save - updates existing document
db.posts.save({
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "title": "Updated Title"
});
```

### 3.3 Reading Documents

#### 3.3.1 Basic find() Operations

**Find All Documents:**

```javascript
db.posts.find()
// OR
db.posts.find({})

// Returns all documents in collection
```

**Output Format:**

```javascript
{ "_id": ObjectId("..."), "title": "First Post", "user": "alice" }
{ "_id": ObjectId("..."), "title": "Second Post", "user": "bob" }
```

#### 3.3.2 Querying with Conditions

**Equality Query:**

```javascript
// Find posts by specific user
db.posts.find({ "user": "alice" });

// Multiple conditions (AND)
db.posts.find({ 
  "user": "alice",
  "status": "published"
});
```

**Using Query Operators:**

```javascript
// Find users in list
db.posts.find({ 
  "user": { $in: ["alice", "bob"] } 
});

// Greater than
db.posts.find({ 
  "commentsCount": { $gt: 10 } 
});

// Less than or equal
db.posts.find({ 
  "views": { $lte: 100 } 
});

// Not equal
db.posts.find({ 
  "status": { $ne: "draft" } 
});
```

**Common Query Operators:**

| Operator | Description | Example |
|----------|-------------|---------|
| **$eq** | Equal to | `{ age: { $eq: 25 } }` |
| **$ne** | Not equal | `{ status: { $ne: "draft" } }` |
| **$gt** | Greater than | `{ age: { $gt: 18 } }` |
| **$gte** | Greater than or equal | `{ score: { $gte: 90 } }` |
| **$lt** | Less than | `{ age: { $lt: 30 } }` |
| **$lte** | Less than or equal | `{ price: { $lte: 100 } }` |
| **$in** | In array | `{ status: { $in: ["active", "pending"] } }` |
| **$nin** | Not in array | `{ role: { $nin: ["admin", "moderator"] } }` |

#### 3.3.3 Building AND/OR Queries

**AND Query (Implicit):**

```javascript
// All conditions must match
db.posts.find({ 
  "user": "alice", 
  "commentsCount": { $gt: 10 },
  "status": "published"
});
```

**OR Query:**

```javascript
// At least one condition must match
db.posts.find({ 
  $or: [
    { "user": "alice" }, 
    { "user": "bob" }
  ] 
});
```

**Complex Query (AND + OR):**

```javascript
db.posts.find({
  "status": "published",  // AND
  $or: [
    { "user": "alice" },
    { "commentsCount": { $gt: 10 } }
  ]
});

// Finds: published posts by alice OR with >10 comments
```

**NOR Query:**

```javascript
db.posts.find({
  $nor: [
    { "status": "draft" },
    { "views": { $lt: 10 } }
  ]
});

// Finds: documents that are NOT draft AND NOT low views
```

#### 3.3.4 Projection and Options

**Field Projection:**

```javascript
// Include specific fields
db.posts.find(
  { "user": "alice" },
  { "title": 1, "createdAt": 1 }  // 1 = include
);

// Exclude specific fields
db.posts.find(
  { "user": "alice" },
  { "content": 0, "comments": 0 }  // 0 = exclude
);

// Note: Cannot mix inclusion and exclusion (except _id)
```

**Query Options:**

```javascript
db.posts.find(
  { "status": "published" },
  { "title": 1, "user": 1 }
).limit(10)         // Return max 10 documents
 .skip(20)          // Skip first 20 documents
 .sort({ "createdAt": -1 }); // Sort descending
```

**Counting Documents:**

```javascript
db.posts.find({ "user": "alice" }).count();

// Alternative
db.posts.count({ "user": "alice" });
```

### 3.4 Updating Documents

#### 3.4.1 Update with update()

**Syntax:**

```javascript
db.collection.update(
  query,       // Selection criteria
  update,      // Modifications
  options      // Additional options
);
```

**Update Single Document:**

```javascript
db.posts.update(
  { "user": "alice" },
  { $set: { "title": "Updated Title" } }
);

// Updates only first matching document
```

**Update Multiple Documents:**

```javascript
db.posts.update(
  { "user": "alice" },
  { $set: { "status": "archived" } },
  { multi: true }  // Update all matching documents
);
```

**Update Operators:**

| Operator | Description | Example |
|----------|-------------|---------|
| **$set** | Set field value | `{ $set: { title: "New Title" } }` |
| **$unset** | Remove field | `{ $unset: { draft: "" } }` |
| **$inc** | Increment value | `{ $inc: { views: 1 } }` |
| **$mul** | Multiply value | `{ $mul: { price: 1.1 } }` |
| **$rename** | Rename field | `{ $rename: { "name": "fullName" } }` |
| **$min** | Update if less than | `{ $min: { score: 50 } }` |
| **$max** | Update if greater than | `{ $max: { score: 100 } }` |
| **$currentDate** | Set current date | `{ $currentDate: { lastModified: true } }` |

**Array Update Operators:**

| Operator | Description | Example |
|----------|-------------|---------|
| **$push** | Add element to array | `{ $push: { tags: "mongodb" } }` |
| **$pull** | Remove element from array | `{ $pull: { tags: "old" } }` |
| **$addToSet** | Add if not exists | `{ $addToSet: { tags: "unique" } }` |
| **$pop** | Remove first/last element | `{ $pop: { tags: 1 } }` |

**Complex Update Example:**

```javascript
db.posts.update(
  { "_id": ObjectId("...") },
  {
    $set: { "title": "New Title" },
    $inc: { "views": 1 },
    $push: { "tags": "tutorial" },
    $currentDate: { "lastModified": true }
  }
);
```

#### 3.4.2 Update with save()

**Syntax:**

```javascript
db.collection.save(document)
```

**Example:**

```javascript
// Requires complete document with _id
db.posts.save({
  "_id": ObjectId("50691737d386d8fadbd6b01d"),
  "title": "Second Post",
  "user": "alice",
  "content": "Updated content"
});

// Replaces entire document
```

**Key Points:**
- Must include `_id` for update
- Replaces entire document (not partial update)
- Creates new document if `_id` doesn't exist
- Less flexible than `update()` with operators

#### 3.4.3 findOneAndUpdate()

**Modern Update Method:**

```javascript
db.posts.findOneAndUpdate(
  { "user": "alice" },           // Query
  { $set: { "status": "active" } }, // Update
  { returnNewDocument: true }    // Return updated doc
);

// Returns the updated document
```

### 3.5 Deleting Documents

#### 3.5.1 Delete with remove()

**Delete All Documents:**

```javascript
db.posts.remove({});

// Removes all documents from collection
// Collection structure remains
```

**Delete Matching Documents:**

```javascript
db.posts.remove({ "user": "alice" });

// Removes all documents where user is alice
```

**Delete Single Document:**

```javascript
db.posts.remove({ "user": "alice" }, true);
// OR
db.posts.remove({ "user": "alice" }, { justOne: true });

// Removes only first matching document
```

#### 3.5.2 Modern Delete Methods

**deleteOne():**

```javascript
db.posts.deleteOne({ "user": "alice" });

// Deletes first matching document
// Returns: { acknowledged: true, deletedCount: 1 }
```

**deleteMany():**

```javascript
db.posts.deleteMany({ "status": "draft" });

// Deletes all matching documents
// Returns: { acknowledged: true, deletedCount: 5 }
```

**findOneAndDelete():**

```javascript
db.posts.findOneAndDelete({ "user": "alice" });

// Deletes and returns the deleted document
```

**Comparison:**

| Method | Behavior | Returns |
|--------|----------|---------|
| **remove()** | Deletes matching docs | WriteResult |
| **deleteOne()** | Deletes first match | DeleteResult with count |
| **deleteMany()** | Deletes all matches | DeleteResult with count |
| **findOneAndDelete()** | Deletes and returns doc | Deleted document |

### 3.6 CRUD Operations Summary

**Quick Reference Table:**

| Operation | Method | Example |
|-----------|--------|---------|
| **Create** | `insert()` | `db.posts.insert({ title: "..." })` |
| | `save()` | `db.posts.save({ title: "..." })` |
| | `update()` with upsert | `db.posts.update({...}, {...}, {upsert:true})` |
| **Read** | `find()` | `db.posts.find({ user: "alice" })` |
| | `findOne()` | `db.posts.findOne({ _id: "..." })` |
| **Update** | `update()` | `db.posts.update({...}, {$set: {...}})` |
| | `save()` | `db.posts.save({ _id: "...", ... })` |
| | `findOneAndUpdate()` | `db.posts.findOneAndUpdate({...}, {...})` |
| **Delete** | `remove()` | `db.posts.remove({ user: "alice" })` |
| | `deleteOne()` | `db.posts.deleteOne({ user: "alice" })` |
| | `deleteMany()` | `db.posts.deleteMany({ status: "draft" })` |

---

## 4. Introduction to Mongoose

### 4.1 What is Mongoose?

**Definition:**
- Object Data Modeling (ODM) library for MongoDB and Node.js
- Provides schema-based solution to model application data
- Built on top of MongoDB native driver
- Simplifies interactions with MongoDB

**Why Use Mongoose?**

| Feature | Benefit |
|---------|---------|
| **Schema Definition** | Structure and validate data |
| **Type Casting** | Automatic type conversion |
| **Validation** | Built-in and custom validators |
| **Middleware** | Pre/post hooks for operations |
| **Query Building** | Simplified query construction |
| **Population** | Reference relationships between documents |

**Mongoose vs Native MongoDB Driver:**

```mermaid
graph TB
    A[Node.js Application] --> B[Mongoose ODM]
    A --> C[MongoDB Native Driver]
    
    B --> D[Schema Validation]
    B --> E[Middleware]
    B --> F[Population]
    B --> G[Type Casting]
    
    B --> H[MongoDB Driver]
    C --> H
    
    H --> I[MongoDB Database]
```

### 4.2 Installing Mongoose

**Step 1: Initialize Node.js Project**

```bash
mkdir new_node_test
cd new_node_test
npm init -y
```

**Generated package.json:**

```json
{
  "name": "new_node_test",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": ""
}
```

**Step 2: Install Mongoose**

```bash
npm install mongoose
```

**Step 3: Verify Installation**

```bash
npm list mongoose
# Output: mongoose@8.x.x
```

### 4.3 Connecting to MongoDB

#### 4.3.1 MongoDB Connection URI

**URI Format:**

```
mongodb://username:password@hostname:port/database
```

**Examples:**

```javascript
// Local MongoDB without authentication
const uri = 'mongodb://localhost/mean-book';

// Local MongoDB with authentication
const uri = 'mongodb://prashanth:root@127.0.0.1/posts';

// MongoDB Atlas (Cloud)
const uri = 'mongodb+srv://username:password@cluster.mongodb.net/database';

// Local with default port
const uri = 'mongodb://localhost:27017/myapp';
```

#### 4.3.2 Basic Connection

**Simple Connection:**

```javascript
const mongoose = require('mongoose');

const uri = 'mongodb://localhost/mean-book';
mongoose.connect(uri);

// Modern syntax with options
mongoose.connect(uri, {
  useNewUrlParser: true,
  useUnifiedTopology: true
});
```

**Connection with Error Handling:**

```javascript
const mongoose = require('mongoose');

const uri = 'mongodb://localhost/mean-book';

mongoose.connect(uri)
  .then(() => {
    console.log('Connected to MongoDB successfully');
  })
  .catch((error) => {
    console.error('MongoDB connection error:', error);
  });
```

**Connection Events:**

```javascript
const mongoose = require('mongoose');

// Connection successful
mongoose.connection.on('connected', () => {
  console.log('Mongoose connected to MongoDB');
});

// Connection error
mongoose.connection.on('error', (err) => {
  console.error('Mongoose connection error:', err);
});

// Connection disconnected
mongoose.connection.on('disconnected', () => {
  console.log('Mongoose disconnected');
});

// Close connection on app termination
process.on('SIGINT', async () => {
  await mongoose.connection.close();
  process.exit(0);
});
```

#### 4.3.3 Complete Server Setup

**server.js Example:**

```javascript
const mongoose = require('mongoose');
const express = require('express');

// MongoDB connection
const uri = 'mongodb://prashanth:root@127.0.0.1/posts';
mongoose.connect(uri);

// Load models after connection
require('./model');

// Express setup
const app = express();

app.listen(8080, () => {
  console.log('Connected to http://localhost:8080');
});

module.exports = app;
```

**Key Points:**
- Load models after establishing connection
- Connection should be established before any database operations
- Use environment variables for connection strings in production

### 4.4 Understanding Mongoose Schemas

#### 4.4.1 What is a Schema?

**Definition:**
- Blueprint for MongoDB documents
- Defines structure of documents within a collection
- Specifies fields, types, validators, defaults

**Schema vs Model:**

| Concept | Purpose |
|---------|---------|
| **Schema** | Defines document structure |
| **Model** | Constructor compiled from schema, creates documents |

#### 4.4.2 Creating a Schema

**Basic Schema Definition:**

```javascript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const UserSchema = new Schema({
  firstName: String,
  lastName: String,
  email: String,
  username: String,
  password: String
});
```

**Schema with Type Definitions:**

```javascript
const UserSchema = new Schema({
  firstName: { type: String },
  lastName: { type: String },
  email: { type: String },
  username: { type: String },
  password: { type: String }
});
```

**Supported Schema Types:**

| Type | Description | Example |
|------|-------------|---------|
| **String** | Text data | `name: String` |
| **Number** | Numeric data | `age: Number` |
| **Date** | Date and time | `createdAt: Date` |
| **Buffer** | Binary data | `image: Buffer` |
| **Boolean** | True/false | `isActive: Boolean` |
| **Mixed** | Any data type | `data: Schema.Types.Mixed` |
| **ObjectId** | MongoDB ObjectId | `userId: Schema.Types.ObjectId` |
| **Array** | Array of values | `tags: [String]` |
| **Decimal128** | High precision decimals | `price: Schema.Types.Decimal128` |
| **Map** | Map data structure | `metadata: Map` |

**Complex Schema Example:**

```javascript
const PostSchema = new Schema({
  title: String,
  content: String,
  author: {
    type: Schema.Types.ObjectId,
    ref: 'User'  // Reference to User model
  },
  tags: [String],  // Array of strings
  metadata: {
    views: Number,
    likes: Number
  },
  comments: [{
    user: String,
    text: String,
    date: Date
  }],
  createdAt: Date,
  updatedAt: Date
});
```

#### 4.4.3 Schema Options

**Timestamps:**

```javascript
const UserSchema = new Schema({
  firstName: String,
  lastName: String
}, {
  timestamps: true  // Adds createdAt and updatedAt
});
```

**Collection Name:**

```javascript
const UserSchema = new Schema({
  firstName: String
}, {
  collection: 'users'  // Explicit collection name
});
```

**Other Options:**

```javascript
const UserSchema = new Schema({
  firstName: String
}, {
  timestamps: true,
  collection: 'users',
  versionKey: false,  // Remove __v field
  strict: true,       // Only save defined schema fields
  toJSON: { virtuals: true },  // Include virtuals in JSON
  toObject: { virtuals: true }
});
```

### 4.5 Creating and Registering Models

#### 4.5.1 Creating a Model

**Basic Model Creation:**

```javascript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

// Define schema
const UserSchema = new Schema({
  firstName: String,
  lastName: String,
  email: String,
  username: String,
  password: String
});

// Create model from schema
mongoose.model('User', UserSchema);
```

**Model Creation Flow:**

```mermaid
graph LR
    A[Define Schema] --> B[Create Model]
    B --> C[Use Model for CRUD]
    
    C --> D[Create Documents]
    C --> E[Query Documents]
    C --> F[Update Documents]
    C --> G[Delete Documents]
```

#### 4.5.2 Registering the User Model

**Step 1: Create Model File (model.js or user.server.model.js):**

```javascript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const UserSchema = new Schema({
  firstName: String,
  lastName: String,
  email: String,
  username: String,
  password: String,
  created: {
    type: Date,
    default: Date.now
  }
});

// Register model
mongoose.model('User', UserSchema);
```

**Step 2: Load Model in Server (server.js):**

```javascript
const mongoose = require('mongoose');
const express = require('express');

// Connect to MongoDB
const uri = 'mongodb://localhost/myapp';
mongoose.connect(uri);

// IMPORTANT: Load models after connection
require('./models/user.server.model');

// Get the model
const User = mongoose.model('User');

const app = express();

app.listen(8080, () => {
  console.log('Server running on port 8080');
});

module.exports = app;
```

**Key Points:**
- Model registration should happen after MongoDB connection
- Use `require()` to load model files
- Retrieve model using `mongoose.model('ModelName')`
- Models are globally available after registration

#### 4.5.3 Model File Organization

**Recommended Structure:**

```
project/
├── server.js
├── config/
│   └── mongoose.js
├── models/
│   ├── user.server.model.js
│   ├── post.server.model.js
│   └── comment.server.model.js
└── controllers/
    ├── user.server.controller.js
    └── post.server.controller.js
```

**config/mongoose.js:**

```javascript
const mongoose = require('mongoose');

module.exports = function() {
  const uri = 'mongodb://localhost/myapp';
  const db = mongoose.connect(uri);
  
  // Load all models
  require('../models/user.server.model');
  require('../models/post.server.model');
  require('../models/comment.server.model');
  
  return db;
};
```

**server.js:**

```javascript
const configDB = require('./config/mongoose');
const express = require('express');

// Initialize MongoDB connection and load models
const db = configDB();

const app = express();

app.listen(8080);
```

---

## 5. Mongoose CRUD Operations

### 5.1 Creating Documents with save()

**Using Model Constructor:**

```javascript
const User = mongoose.model('User');

// Create new user instance
const newUser = new User({
  firstName: 'John',
  lastName: 'Doe',
  email: 'john@example.com',
  username: 'johndoe',
  password: 'password123'
});

// Save to database
newUser.save()
  .then(user => {
    console.log('User created:', user);
  })
  .catch(error => {
    console.error('Error creating user:', error);
  });
```

**Async/Await Syntax:**

```javascript
async function createUser() {
  try {
    const User = mongoose.model('User');
    
    const newUser = new User({
      firstName: 'Alice',
      lastName: 'Smith',
      email: 'alice@example.com',
      username: 'alicesmith',
      password: 'securepass'
    });
    
    const savedUser = await newUser.save();
    console.log('User saved:', savedUser);
    return savedUser;
  } catch (error) {
    console.error('Save error:', error);
  }
}
```

**Using create() Method:**

```javascript
// Alternative: Direct creation
const User = mongoose.model('User');

User.create({
  firstName: 'Bob',
  lastName: 'Johnson',
  email: 'bob@example.com',
  username: 'bobjohnson',
  password: 'bobpass'
})
.then(user => {
  console.log('User created:', user);
})
.catch(error => {
  console.error('Error:', error);
});
```

**Batch Creation:**

```javascript
const User = mongoose.model('User');

const users = [
  { firstName: 'User1', username: 'user1', email: 'user1@example.com' },
  { firstName: 'User2', username: 'user2', email: 'user2@example.com' },
  { firstName: 'User3', username: 'user3', email: 'user3@example.com' }
];

User.create(users)
  .then(createdUsers => {
    console.log('Created users:', createdUsers.length);
  })
  .catch(error => {
    console.error('Batch creation error:', error);
  });
```

### 5.2 Finding Multiple Documents with find()

#### 5.2.1 Basic find() Usage

**Find All Documents:**

```javascript
const User = mongoose.model('User');

User.find()
  .then(users => {
    console.log('All users:', users);
  })
  .catch(error => {
    console.error('Find error:', error);
  });
```

**Find with Conditions:**

```javascript
// Find by username
User.find({ username: 'johndoe' })
  .then(users => {
    console.log('Found users:', users);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

**Multiple Conditions:**

```javascript
// Find active users created after specific date
User.find({
  isActive: true,
  createdAt: { $gte: new Date('2024-01-01') }
})
.then(users => {
  console.log('Active users:', users);
});
```

#### 5.2.2 Advanced find() with Projection

**Field Projection:**

```javascript
// Include specific fields
User.find({}, 'firstName lastName email')
  .then(users => {
    console.log('User names and emails:', users);
  });

// Exclude specific fields
User.find({}, '-password -__v')
  .then(users => {
    console.log('Users without sensitive data:', users);
  });
```

**Projection Object:**

```javascript
User.find({}, {
  firstName: 1,
  lastName: 1,
  email: 1,
  _id: 0  // Exclude _id
})
.then(users => {
  console.log('Projected users:', users);
});
```

#### 5.2.3 Query Options

**Limit, Skip, Sort:**

```javascript
User.find({}, 'firstName lastName', {
  limit: 10,        // Return max 10 documents
  skip: 0,          // Skip first 0 documents (pagination)
  sort: { firstName: 1 }  // Sort ascending by firstName
})
.then(users => {
  console.log('Paginated users:', users);
});
```

**Chained Query Methods:**

```javascript
User.find({ isActive: true })
  .select('firstName lastName email')
  .limit(10)
  .skip(20)
  .sort({ createdAt: -1 })  // -1 = descending
  .exec()
  .then(users => {
    console.log('Query result:', users);
  });
```

**Sort Options:**

```javascript
// Ascending sort
User.find().sort({ firstName: 1 });

// Descending sort
User.find().sort({ createdAt: -1 });

// Multiple fields
User.find().sort({ lastName: 1, firstName: 1 });

// String syntax
User.find().sort('firstName -createdAt');  // firstName asc, createdAt desc
```

**Pagination Example:**

```javascript
async function getPaginatedUsers(page = 1, pageSize = 10) {
  const User = mongoose.model('User');
  
  const skip = (page - 1) * pageSize;
  
  const users = await User.find()
    .select('firstName lastName email')
    .limit(pageSize)
    .skip(skip)
    .sort({ createdAt: -1 });
  
  const total = await User.countDocuments();
  
  return {
    users,
    page,
    pageSize,
    totalPages: Math.ceil(total / pageSize),
    totalUsers: total
  };
}

// Usage
getPaginatedUsers(2, 10).then(result => {
  console.log('Page 2 users:', result);
});
```

#### 5.2.4 Query Operators

**Comparison Operators:**

```javascript
// Greater than
User.find({ age: { $gt: 18 } });

// Less than or equal
User.find({ age: { $lte: 65 } });

// In array
User.find({ status: { $in: ['active', 'pending'] } });

// Not in array
User.find({ role: { $nin: ['admin', 'moderator'] } });
```

**Logical Operators:**

```javascript
// AND (implicit)
User.find({
  age: { $gte: 18 },
  isActive: true
});

// OR
User.find({
  $or: [
    { username: 'johndoe' },
    { email: 'john@example.com' }
  ]
});

// Complex query
User.find({
  $and: [
    { age: { $gte: 18 } },
    {
      $or: [
        { role: 'admin' },
        { verified: true }
      ]
    }
  ]
});
```

**String Operators:**

```javascript
// Regular expression
User.find({ username: /^john/i });  // Starts with "john", case-insensitive

// Contains
User.find({ email: /@gmail\.com$/ });  // Ends with @gmail.com
```

### 5.3 Reading Single Document with findOne()

**Basic findOne():**

```javascript
const User = mongoose.model('User');

User.findOne({ username: 'johndoe' })
  .then(user => {
    if (user) {
      console.log('Found user:', user);
    } else {
      console.log('User not found');
    }
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

**findOne() with Callback (Legacy):**

```javascript
User.findOne({ _id: userId }, (err, user) => {
  if (err) {
    console.error('Error finding user:', err);
  } else {
    console.log('Found user:', user);
  }
});
```

**findOne() with Projection:**

```javascript
User.findOne(
  { username: 'johndoe' },
  'firstName lastName email'
)
.then(user => {
  console.log('User data:', user);
});
```

**findById() - Shorthand:**

```javascript
// Find by _id
User.findById('507f1f77bcf86cd799439011')
  .then(user => {
    console.log('User:', user);
  });

// Equivalent to:
User.findOne({ _id: '507f1f77bcf86cd799439011' });
```

**Comparison:**

| Method | Use Case |
|--------|----------|
| **find()** | Returns array (even if one match) |
| **findOne()** | Returns single document or null |
| **findById()** | Shorthand for finding by _id |

### 5.4 Updating Documents

#### 5.4.1 findOneAndUpdate()

**Basic Update:**

```javascript
const User = mongoose.model('User');

User.findOneAndUpdate(
  { _id: userId },                    // Query
  { email: 'newemail@example.com' },  // Update
  { new: true }                        // Return updated document
)
.then(user => {
  console.log('Updated user:', user);
})
.catch(error => {
  console.error('Update error:', error);
});
```

**With Update Operators:**

```javascript
User.findOneAndUpdate(
  { username: 'johndoe' },
  {
    $set: { email: 'john.new@example.com' },
    $inc: { loginCount: 1 },
    $push: { tags: 'premium' }
  },
  { new: true }
)
.then(user => {
  console.log('Updated:', user);
});
```

**Options:**

| Option | Description | Default |
|--------|-------------|---------|
| **new** | Return updated document | false (returns original) |
| **upsert** | Create if not exists | false |
| **runValidators** | Run schema validators | false |
| **select** | Field projection | null |

**Example with Options:**

```javascript
User.findOneAndUpdate(
  { username: 'newuser' },
  { $set: { isActive: true } },
  {
    new: true,
    upsert: true,
    runValidators: true,
    select: 'username isActive'
  }
)
.then(user => {
  console.log('User:', user);
});
```

#### 5.4.2 findByIdAndUpdate()

**Shorthand for Updating by ID:**

```javascript
User.findByIdAndUpdate(
  userId,
  { $set: { firstName: 'UpdatedName' } },
  { new: true }
)
.then(user => {
  console.log('Updated user:', user);
});
```

#### 5.4.3 update() and updateMany()

**Update Multiple Documents:**

```javascript
// Update all matching documents
User.updateMany(
  { isActive: false },
  { $set: { status: 'inactive' } }
)
.then(result => {
  console.log('Modified count:', result.modifiedCount);
});
```

**updateOne():**

```javascript
// Update first matching document
User.updateOne(
  { username: 'johndoe' },
  { $set: { lastLogin: new Date() } }
)
.then(result => {
  console.log('Update result:', result);
});
```

#### 5.4.4 Using save() for Updates

**Load, Modify, Save Pattern:**

```javascript
async function updateUser(userId, updates) {
  try {
    // Find document
    const user = await User.findById(userId);
    
    if (!user) {
      throw new Error('User not found');
    }
    
    // Modify fields
    user.firstName = updates.firstName || user.firstName;
    user.lastName = updates.lastName || user.lastName;
    user.email = updates.email || user.email;
    
    // Save (triggers validators and middleware)
    const updatedUser = await user.save();
    
    return updatedUser;
  } catch (error) {
    console.error('Update error:', error);
  }
}
```

**save() vs findOneAndUpdate():**

| Method | Validators | Middleware | Performance |
|--------|------------|------------|-------------|
| **save()** | Always runs | Pre/post save hooks | Slower (2 operations) |
| **findOneAndUpdate()** | Optional | Update middleware | Faster (1 operation) |

### 5.5 Deleting Documents

#### 5.5.1 deleteOne()

**Delete First Matching Document:**

```javascript
const User = mongoose.model('User');

User.deleteOne({ username: 'johndoe' })
  .then(result => {
    console.log('Delete result:', result);
    // { acknowledged: true, deletedCount: 1 }
  })
  .catch(error => {
    console.error('Delete error:', error);
  });
```

#### 5.5.2 deleteMany()

**Delete All Matching Documents:**

```javascript
// Delete inactive users
User.deleteMany({ isActive: false })
  .then(result => {
    console.log('Deleted count:', result.deletedCount);
  });

// Delete all documents
User.deleteMany({})
  .then(result => {
    console.log('Deleted all users:', result.deletedCount);
  });
```

#### 5.5.3 findOneAndDelete()

**Delete and Return Document:**

```javascript
User.findOneAndDelete({ _id: userId })
  .then(deletedUser => {
    if (deletedUser) {
      console.log('Deleted user:', deletedUser);
    } else {
      console.log('User not found');
    }
  });
```

#### 5.5.4 findByIdAndDelete()

**Shorthand for Deleting by ID:**

```javascript
User.findByIdAndDelete(userId)
  .then(deletedUser => {
    console.log('Deleted:', deletedUser);
  });
```

**Comparison:**

| Method | Returns | Use Case |
|--------|---------|----------|
| **deleteOne()** | DeleteResult | Don't need deleted document |
| **deleteMany()** | DeleteResult | Bulk deletion |
| **findOneAndDelete()** | Deleted document | Need document data |
| **findByIdAndDelete()** | Deleted document | Delete by ID with data |

### 5.6 CRUD Operations Complete Example

```javascript
const mongoose = require('mongoose');

// Connect to MongoDB
mongoose.connect('mongodb://localhost/userapp');

// Define Schema
const UserSchema = new mongoose.Schema({
  firstName: String,
  lastName: String,
  email: String,
  username: String,
  password: String,
  createdAt: { type: Date, default: Date.now }
});

// Create Model
const User = mongoose.model('User', UserSchema);

// CREATE
async function createUser() {
  const user = new User({
    firstName: 'John',
    lastName: 'Doe',
    email: 'john@example.com',
    username: 'johndoe',
    password: 'password123'
  });
  
  const savedUser = await user.save();
  console.log('Created:', savedUser);
  return savedUser;
}

// READ - Multiple
async function findUsers() {
  const users = await User.find({ isActive: true })
    .select('firstName lastName email')
    .limit(10)
    .sort({ createdAt: -1 });
  
  console.log('Found users:', users);
  return users;
}

// READ - Single
async function findUserByUsername(username) {
  const user = await User.findOne({ username });
  console.log('Found user:', user);
  return user;
}

// UPDATE
async function updateUser(userId, updates) {
  const user = await User.findByIdAndUpdate(
    userId,
    { $set: updates },
    { new: true, runValidators: true }
  );
  
  console.log('Updated user:', user);
  return user;
}

// DELETE
async function deleteUser(userId) {
  const result = await User.findByIdAndDelete(userId);
  console.log('Deleted user:', result);
  return result;
}

// Execute CRUD operations
async function runCRUD() {
  try {
    // Create
    const newUser = await createUser();
    
    // Read
    await findUsers();
    await findUserByUsername('johndoe');
    
    // Update
    await updateUser(newUser._id, { email: 'newemail@example.com' });
    
    // Delete
    await deleteUser(newUser._id);
    
  } catch (error) {
    console.error('CRUD error:', error);
  } finally {
    await mongoose.connection.close();
  }
}

runCRUD();
```

---

## 6. Extending Mongoose Schemas

### 6.1 Defining Default Values

**Purpose:**
- Provide automatic values for fields when not specified
- Ensure data consistency
- Simplify document creation

**Basic Default Values:**

```javascript
const UserSchema = new Schema({
  firstName: String,
  lastName: String,
  email: String,
  username: String,
  password: String,
  created: {
    type: Date,
    default: Date.now  // Function reference
  },
  isActive: {
    type: Boolean,
    default: true  // Static value
  },
  role: {
    type: String,
    default: 'user'
  }
});
```

**Default with Functions:**

```javascript
const PostSchema = new Schema({
  title: String,
  slug: {
    type: String,
    default: function() {
      return this.title.toLowerCase().replace(/\s+/g, '-');
    }
  },
  viewCount: {
    type: Number,
    default: 0
  },
  tags: {
    type: [String],
    default: []  // Empty array
  },
  metadata: {
    type: Object,
    default: function() {
      return {
        views: 0,
        likes: 0,
        shares: 0
      };
    }
  },
  publishedAt: {
    type: Date,
    default: Date.now
  }
});
```

**Using Arrow Functions (Not Recommended):**

```javascript
// DON'T use arrow functions - 'this' won't work correctly
created: {
  type: Date,
  default: () => Date.now()  // Avoid this
}

// DO use regular functions or function references
created: {
  type: Date,
  default: Date.now  // Correct - function reference
}

created: {
  type: Date,
  default: function() {  // Correct - regular function
    return Date.now();
  }
}
```

**Example Usage:**

```javascript
// Without default values
const user1 = new User({
  firstName: 'John',
  lastName: 'Doe',
  email: 'john@example.com'
});
// created and isActive automatically set

await user1.save();
console.log(user1.created);   // Current timestamp
console.log(user1.isActive);  // true
console.log(user1.role);      // 'user'
```

### 6.2 Using Schema Modifiers

**Definition:**
- Modifiers transform field values before saving or retrieving
- Two types: Getters (retrieve) and Setters (save)

#### 6.2.1 Predefined Modifiers

**String Modifiers:**

```javascript
const UserSchema = new Schema({
  username: {
    type: String,
    trim: true,      // Remove leading/trailing whitespace
    lowercase: true  // Convert to lowercase
  },
  firstName: {
    type: String,
    trim: true
  },
  lastName: {
    type: String,
    trim: true
  },
  email: {
    type: String,
    trim: true,
    lowercase: true
  },
  role: {
    type: String,
    uppercase: true,  // Convert to uppercase
    enum: ['USER', 'ADMIN', 'MODERATOR']
  }
});
```

**Available String Modifiers:**

| Modifier | Description | Example |
|----------|-------------|---------|
| **lowercase** | Convert to lowercase | `'JOHN'` → `'john'` |
| **uppercase** | Convert to uppercase | `'john'` → `'JOHN'` |
| **trim** | Remove whitespace | `' john '` → `'john'` |

**Example Usage:**

```javascript
const user = new User({
  username: '  JohnDoe  ',  // Saved as 'johndoe'
  email: ' John@Example.COM ',  // Saved as 'john@example.com'
  role: 'admin'  // Saved as 'ADMIN'
});

await user.save();
console.log(user.username);  // 'johndoe'
console.log(user.email);     // 'john@example.com'
console.log(user.role);      // 'ADMIN'
```

**Number Modifiers:**

```javascript
const ProductSchema = new Schema({
  price: {
    type: Number,
    min: 0,           // Minimum value
    max: 1000000      // Maximum value
  },
  quantity: {
    type: Number,
    min: [0, 'Quantity cannot be negative'],
    get: function(v) {
      return Math.round(v);  // Always return integer
    }
  },
  discount: {
    type: Number,
    min: 0,
    max: 100,
    default: 0
  }
});
```

### 6.3 Custom Setter Modifiers

**Definition:**
- Custom functions that transform values before saving to database
- Execute when setting field value

**Basic Custom Setter:**

```javascript
const UserSchema = new Schema({
  website: {
    type: String,
    set: function(url) {
      if (!url) {
        return url;
      }
      
      // Add http:// if no protocol specified
      if (url.indexOf('http://') !== 0 && url.indexOf('https://') !== 0) {
        url = 'http://' + url;
      }
      
      return url;
    }
  }
});
```

**Usage:**

```javascript
const user = new User({
  website: 'example.com'
});

await user.save();
console.log(user.website);  // 'http://example.com'

// With protocol
const user2 = new User({
  website: 'https://example.com'
});
console.log(user2.website);  // 'https://example.com'
```

**Email Normalization Setter:**

```javascript
const UserSchema = new Schema({
  email: {
    type: String,
    set: function(email) {
      if (!email) return email;
      
      // Trim, lowercase, and remove special characters
      return email.trim().toLowerCase();
    },
    validate: {
      validator: function(email) {
        return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
      },
      message: 'Invalid email format'
    }
  }
});
```

**Password Hashing Setter:**

```javascript
const bcrypt = require('bcrypt');

const UserSchema = new Schema({
  password: {
    type: String,
    set: function(password) {
      // Hash password before saving
      return bcrypt.hashSync(password, 10);
    }
  }
});

// Usage
const user = new User({
  username: 'john',
  password: 'plaintext123'
});

await user.save();
// password stored as hashed value
```

**Phone Number Formatting:**

```javascript
const UserSchema = new Schema({
  phone: {
    type: String,
    set: function(phone) {
      if (!phone) return phone;
      
      // Remove all non-digit characters
      const cleaned = phone.replace(/\D/g, '');
      
      // Format as (XXX) XXX-XXXX
      if (cleaned.length === 10) {
        return `(${cleaned.slice(0,3)}) ${cleaned.slice(3,6)}-${cleaned.slice(6)}`;
      }
      
      return phone;
    }
  }
});

// Usage
const user = new User({
  phone: '1234567890'
});
console.log(user.phone);  // '(123) 456-7890'
```

**Multiple Setters Example:**

```javascript
const ProductSchema = new Schema({
  name: {
    type: String,
    trim: true,  // Predefined modifier
    set: function(name) {  // Custom setter
      // Capitalize first letter of each word
      return name.replace(/\w\S*/g, function(txt) {
        return txt.charAt(0).toUpperCase() + txt.substr(1).toLowerCase();
      });
    }
  },
  sku: {
    type: String,
    uppercase: true,  // Predefined modifier
    set: function(sku) {  // Custom setter
      // Remove spaces and special characters
      return sku.replace(/[^a-zA-Z0-9]/g, '').toUpperCase();
    }
  }
});

// Usage
const product = new Product({
  name: '  wireless mouse  ',  // Saved as 'Wireless Mouse'
  sku: 'wm-123-x'              // Saved as 'WM123X'
});
```

### 6.4 Custom Getter Modifiers

**Definition:**
- Transform values when retrieving from database
- Do not modify stored data
- Execute when accessing field value

**Basic Custom Getter:**

```javascript
const UserSchema = new Schema({
  firstName: String,
  lastName: String,
  fullName: {
    type: String,
    get: function() {
      return `${this.firstName} ${this.lastName}`;
    }
  }
});

// Enable getters in JSON output
UserSchema.set('toJSON', { getters: true });
UserSchema.set('toObject', { getters: true });
```

**Usage:**

```javascript
const user = new User({
  firstName: 'John',
  lastName: 'Doe'
});

console.log(user.fullName);  // 'John Doe' (not stored in DB)
```

**URL Formatting Getter:**

```javascript
const UserSchema = new Schema({
  website: {
    type: String,
    get: function(url) {
      if (!url) return url;
      
      // Ensure protocol is present
      if (url.indexOf('http://') !== 0 && url.indexOf('https://') !== 0) {
        return 'http://' + url;
      }
      
      return url;
    }
  }
});

UserSchema.set('toJSON', { getters: true });
```

**Date Formatting Getter:**

```javascript
const PostSchema = new Schema({
  createdAt: {
    type: Date,
    default: Date.now,
    get: function(date) {
      if (!date) return date;
      
      // Format as 'YYYY-MM-DD'
      return date.toISOString().split('T')[0];
    }
  }
});

PostSchema.set('toJSON', { getters: true });

// Usage
const post = new Post({ title: 'My Post' });
console.log(post.createdAt);  // '2024-12-21'
```

**Currency Formatting Getter:**

```javascript
const ProductSchema = new Schema({
  price: {
    type: Number,
    get: function(price) {
      if (!price) return price;
      
      // Format as currency with 2 decimal places
      return ' + price.toFixed(2);
    }
  }
});

ProductSchema.set('toJSON', { getters: true });

// Usage
const product = new Product({ price: 29.99 });
console.log(product.price);  // '$29.99'
```

**Virtual Properties with Getters:**

```javascript
const UserSchema = new Schema({
  firstName: String,
  lastName: String
});

// Virtual property (not stored in DB)
UserSchema.virtual('fullName').get(function() {
  return `${this.firstName} ${this.lastName}`;
});

// Another virtual
UserSchema.virtual('initials').get(function() {
  return `${this.firstName[0]}${this.lastName[0]}`.toUpperCase();
});

UserSchema.set('toJSON', { virtuals: true });
UserSchema.set('toObject', { virtuals: true });

// Usage
const user = new User({
  firstName: 'John',
  lastName: 'Doe'
});

console.log(user.fullName);  // 'John Doe'
console.log(user.initials);  // 'JD'
console.log(user.toJSON());  // Includes virtual properties
```

**Image URL Getter:**

```javascript
const UserSchema = new Schema({
  avatar: {
    type: String,
    get: function(avatar) {
      if (!avatar) return '/images/default-avatar.png';
      
      // Add CDN base URL if relative path
      if (avatar.startsWith('/')) {
        return `https://cdn.example.com${avatar}`;
      }
      
      return avatar;
    }
  }
});

UserSchema.set('toJSON', { getters: true });

// Usage
const user1 = new User({ avatar: '/avatars/user123.jpg' });
console.log(user1.avatar);  // 'https://cdn.example.com/avatars/user123.jpg'

const user2 = new User({});
console.log(user2.avatar);  // '/images/default-avatar.png'
```

**Enabling Getters Globally:**

```javascript
const UserSchema = new Schema({
  // ... fields with getters
}, {
  toJSON: { getters: true, virtuals: true },
  toObject: { getters: true, virtuals: true }
});
```

### 6.5 Schema Modifiers Complete Example

```javascript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const UserSchema = new Schema({
  // Basic fields with predefined modifiers
  username: {
    type: String,
    required: true,
    unique: true,
    trim: true,
    lowercase: true
  },
  
  email: {
    type: String,
    required: true,
    unique: true,
    trim: true,
    lowercase: true,
    validate: {
      validator: function(email) {
        return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
      },
      message: 'Invalid email format'
    }
  },
  
  // Custom setter - add protocol to URL
  website: {
    type: String,
    set: function(url) {
      if (!url) return url;
      if (url.indexOf('http://') !== 0 && url.indexOf('https://') !== 0) {
        return 'http://' + url;
      }
      return url;
    }
  },
  
  // Custom setter - format phone number
  phone: {
    type: String,
    set: function(phone) {
      if (!phone) return phone;
      const cleaned = phone.replace(/\D/g, '');
      if (cleaned.length === 10) {
        return `(${cleaned.slice(0,3)}) ${cleaned.slice(3,6)}-${cleaned.slice(6)}`;
      }
      return phone;
    }
  },
  
  firstName: {
    type: String,
    required: true,
    trim: true
  },
  
  lastName: {
    type: String,
    required: true,
    trim: true
  },
  
  // Custom getter - format date
  createdAt: {
    type: Date,
    default: Date.now,
    get: function(date) {
      if (!date) return date;
      return date.toISOString().split('T')[0];
    }
  },
  
  // Default values
  role: {
    type: String,
    default: 'user',
    enum: ['user', 'admin', 'moderator']
  },
  
  isActive: {
    type: Boolean,
    default: true
  },
  
  // Custom getter - avatar with CDN
  avatar: {
    type: String,
    get: function(avatar) {
      if (!avatar) return '/images/default-avatar.png';
      if (avatar.startsWith('/')) {
        return `https://cdn.example.com${avatar}`;
      }
      return avatar;
    }
  }
});

// Virtual property - full name
UserSchema.virtual('fullName').get(function() {
  return `${this.firstName} ${this.lastName}`;
});

// Virtual property - initials
UserSchema.virtual('initials').get(function() {
  return `${this.firstName[0]}${this.lastName[0]}`.toUpperCase();
});

// Enable getters and virtuals
UserSchema.set('toJSON', { getters: true, virtuals: true });
UserSchema.set('toObject', { getters: true, virtuals: true });

// Create model
const User = mongoose.model('User', UserSchema);

// Usage example
async function demonstrateModifiers() {
  const user = new User({
    username: '  JohnDoe  ',
    email: ' John@Example.COM ',
    website: 'example.com',
    phone: '1234567890',
    firstName: 'John',
    lastName: 'Doe',
    avatar: '/avatars/john.jpg'
  });
  
  await user.save();
  
  console.log('Username:', user.username);   // 'johndoe' (trim, lowercase)
  console.log('Email:', user.email);         // 'john@example.com' (trim, lowercase)
  console.log('Website:', user.website);     // 'http://example.com' (custom setter)
  console.log('Phone:', user.phone);         // '(123) 456-7890' (custom setter)
  console.log('Full Name:', user.fullName);  // 'John Doe' (virtual getter)
  console.log('Initials:', user.initials);   // 'JD' (virtual getter)
  console.log('Avatar:', user.avatar);       // CDN URL (custom getter)
  console.log('Created:', user.createdAt);   // Formatted date (custom getter)
  console.log('Role:', user.role);           // 'user' (default value)
  console.log('Active:', user.isActive);     // true (default value)
}
```

### 6.6 Schema Options Summary

**Common Schema Options:**

```javascript
const UserSchema = new Schema({
  // Field definitions
}, {
  // Schema options
  timestamps: true,           // Add createdAt, updatedAt
  collection: 'users',        // Collection name
  versionKey: false,          // Remove __v field
  strict: true,               // Only save schema fields
  strictQuery: true,          // Apply strict to queries
  toJSON: {
    getters: true,            // Apply getters
    virtuals: true,           // Include virtuals
    transform: function(doc, ret) {
      delete ret.password;    // Remove password from JSON
      return ret;
    }
  },
  toObject: {
    getters: true,
    virtuals: true
  }
});
```

**Schema Options Reference:**

| Option | Description | Default |
|--------|-------------|---------|
| **timestamps** | Auto-add createdAt/updatedAt | false |
| **collection** | Explicit collection name | Pluralized model name |
| **versionKey** | Enable __v versioning | true ('__v') |
| **strict** | Only save defined fields | true |
| **strictQuery** | Apply strict mode to queries | false |
| **toJSON** | Options for toJSON() | {} |
| **toObject** | Options for toObject() | {} |
| **validateBeforeSave** | Validate before saving | true |
| **versionKey** | Version key name | '__v' |

---

## 7. Important Exam Questions

### Short Answer Questions (2-5 marks)

1. **What is NoSQL? Explain why NoSQL databases emerged.**
   - Define NoSQL
   - Problems with relational databases (scaling, complexity)
   - Benefits: horizontal scaling, simple queries, high availability

2. **Differentiate between relational and document databases with an example.**
   - Relational: data in tables with rows/columns
   - Document: hierarchical documents (JSON/XML)
   - Example: blog post storage in both

3. **What is BSON? Explain its advantages over JSON.**
   - Binary-encoded JSON
   - More efficient size and speed
   - Extended data types (Date, ObjectId, Binary)
   - Structure of ObjectId (12 bytes breakdown)

4. **What are MongoDB collections and databases?**
   - Collection: set of documents (like table)
   - Database: logical grouping of collections
   - Primary key (_id) mandatory
   - Example structure

5. **Explain ad hoc queries in MongoDB.**
   - Dynamic query response without predefining
   - Uses indexing on BSON documents
   - Example: SQL vs MongoDB query comparison

6. **What is indexing in MongoDB? Why is it important?**
   - Data structures for efficient queries
   - Without index: full collection scan
   - With index: direct document access
   - Performance comparison

7. **What is a replica set in MongoDB?**
   - Architecture for data redundancy
   - Primary: handles writes
   - Secondary: handles reads, replicates data
   - Automatic failover mechanism

8. **Explain the role of arbiter in replica set.**
   - No data maintenance
   - Participates in primary election
   - Maintains quorum
   - Cannot become primary/secondary

9. **What is MongoDB sharding?**
   - Horizontal scaling technique
   - Splits data across multiple machines
   - Components: shards, query router (mongos), config servers
   - Use cases

10. **Differentiate between insert(), update() with upsert, and save() in MongoDB.**
    - Table comparing behavior
    - When to use each
    - Examples

11. **What is Mongoose? Why is it used with MongoDB?**
    - ODM (Object Data Modeling) library
    - Schema-based modeling
    - Benefits: validation, type casting, middleware, population

12. **Explain Mongoose schema and model.**
    - Schema: blueprint for documents
    - Model: constructor compiled from schema
    - Relationship diagram

13. **What are default values in Mongoose schemas?**
    - Automatic values when field not specified
    - Static values vs functions
    - Example with Date.now

14. **What are predefined modifiers in Mongoose?**
    - String: trim, lowercase, uppercase
    - Number: min, max
    - Examples

15. **Differentiate between custom getter and setter modifiers.**
    - Table comparison
    - When each executes
    - Examples (URL formatting, password hashing)

### Long Answer Questions (10-15 marks)

16. **Explain MongoDB architecture in detail with diagram.**
    - Documents, Collections, Databases
    - BSON format and ObjectId structure
    - Query language
    - Indexing mechanism
    - Replica sets architecture
    - Sharding architecture
    - All diagrams

17. **Explain all MongoDB CRUD operations with syntax and examples.**
    - Creating: insert(), update() with upsert, save()
    - Reading: find(), findOne() with operators
    - Updating: update(), save() with operators
    - Deleting: remove(), deleteOne(), deleteMany()
    - Complete examples for each

18. **Explain Mongoose CRUD operations with complete code examples.**
    - Creating: save(), create()
    - Reading: find(), findOne(), findById() with options
    - Updating: findOneAndUpdate(), findByIdAndUpdate(), save()
    - Deleting: deleteOne(), deleteMany(), findOneAndDelete()
    - Complete working example

19. **Explain schema modifiers in Mongoose with examples.**
    - Default values (static and function-based)
    - Predefined modifiers (trim, lowercase, uppercase)
    - Custom setter modifiers (3-4 examples)
    - Custom getter modifiers (3-4 examples)
    - Virtual properties
    - Complete schema example demonstrating all

20. **Compare and contrast SQL and MongoDB with examples.**
    - Data model differences
    - Query language comparison
    - Scaling approaches
    - Schema flexibility
    - Use cases for each
    - Example: e-commerce application in both

### Diagram-Based Questions (5-10 marks)

21. **Draw and explain MongoDB replica set architecture.**
    - Primary, Secondary, Arbiter roles
    - Data replication flow
    - Automatic failover process
    - Read/write operations handling

22. **Draw and explain MongoDB sharding architecture.**
    - Shards, Query Router, Config Servers
    - Query routing mechanism
    - Data distribution
    - Benefits

23. **Draw the flow diagram for Mongoose schema to document creation.**
    - Schema definition
    - Model creation
    - Document instantiation
    - Validation
    - Middleware execution
    - Database save

24. **Explain indexing with diagram showing with and without index.**
    - Query execution without index (full scan)
    - Query execution with index (direct access)
    - Performance comparison
    - B-tree structure concept

### Code-Based Questions (10 marks)

25. **Write a complete Mongoose schema for a Blog application with:**
    - User schema (with modifiers and defaults)
    - Post schema (with references)
    - Comment schema (embedded)
    - All CRUD operations
    - Custom validators

26. **Write code to demonstrate all Mongoose CRUD operations on a User model.**
    - Schema definition with validation
    - Create operations (save and create)
    - Read operations (find variations)
    - Update operations (multiple methods)
    - Delete operations
    - Error handling

27. **Create a Mongoose schema with:**
    - At least 2 default values
    - 2 predefined modifiers
    - 1 custom setter
    - 1 custom getter
    - 1 virtual property
    - Complete working example

28. **Write MongoDB shell commands for:**
    - Creating documents (3 methods)
    - Complex queries (AND/OR, operators)
    - Update operations with operators
    - Delete operations
    - Proper syntax and comments

### Practical/Application Questions (10-15 marks)

29. **Design a complete MongoDB database for an e-commerce application.**
    - Collections needed
    - Document structure for each
    - Relationships between collections
    - Indexes required
    - Sample queries

30. **Explain how you would implement user authentication using Mongoose.**
    - User schema design
    - Password hashing (setter)
    - Email validation
    - Login method
    - Token storage
    - Complete code

31. **You have 10 million user records. Explain:**
    - Why use MongoDB sharding
    - How to design shard key
    - Query routing mechanism
    - Performance benefits

32. **Design a blog application backend using Mongoose:**
    - All schemas (User, Post, Comment)
    - Relationships
    - CRUD APIs structure
    - Validation and error handling
    - Code snippets

### Comparison Questions (5-10 marks)

33. **Compare:**
    - find() vs findOne()
    - save() vs create()
    - update() vs findOneAndUpdate()
    - deleteOne() vs findOneAndDelete()

34. **Vertical Scaling vs Horizontal Scaling in context of MongoDB**
    - Definitions
    - Implementation
    - When to use each
    - MongoDB sharding as horizontal scaling

35. **Relational Database vs NoSQL Database**
    - Data model
    - Scalability
    - Schema flexibility
    - Query language
    - Use cases
    - Performance characteristics

---

## Important Topics for Quick Revision

### Must-Know Concepts

1. **NoSQL Introduction**
   - Why NoSQL emerged
   - Types of NoSQL databases
   - Document database concept

2. **MongoDB Basics**
   - BSON format and ObjectId
   - Collections and Databases
   - Query language basics

3. **MongoDB Features**
   - Ad hoc queries
   - Indexing (with/without comparison)
   - Replica sets (Primary, Secondary, Arbiter)
   - Sharding (architecture and benefits)

4. **MongoDB CRUD**
   - All three create methods
   - find() with operators
   - update() with operators ($set, $inc, etc.)
   - delete() methods

5. **Mongoose Basics**
   - What and why
   - Connection to MongoDB
   - Schema vs Model

6. **Mongoose CRUD**
   - save() and create()
   - find(), findOne(), findById()
   - findOneAndUpdate(), findByIdAndUpdate()
   - deleteOne(), deleteMany(), findOneAndDelete()

7. **Schema Extensions**
   - Default values
   - Predefined modifiers (trim, lowercase, uppercase)
   - Custom setters (at least 2 examples)
   - Custom getters (at least 2 examples)
   - Virtual properties

### Key Code Snippets to Remember

1. MongoDB connection URI format
2. Basic Mongoose schema definition
3. Model creation and registration
4. Document creation with save()
5. find() with projection and options
6. update() with $set, $inc operators
7. Schema with default values
8. Schema with custom setter/getter
9. Virtual property definition

### Important Diagrams

1. Relational vs Document database comparison
2. MongoDB Replica Set architecture
3. MongoDB Sharding architecture
4. Indexing performance comparison
5. Mongoose Schema → Model → Document flow

---

## Tips for Exam Preparation

1. **Focus on practical code**: Understand syntax thoroughly
2. **Draw diagrams clearly**: Replica sets and sharding are important
3. **Memorize CRUD operations**: All variations and options
4. **Understand modifiers**: Know difference between getter and setter
5. **Practice schema design**: Real-world scenarios
6. **Know comparisons**: SQL vs MongoDB, find() vs findOne()
7. **Time management**: Allocate time based on marks
8. **Code comments**: Add brief comments to show understanding

---

**End of UNIT III Notes**


