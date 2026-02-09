# Unit III - MongoDB & Mongoose Q&A Guide (Simplified)

## Part 1: Introduction to MongoDB

### 1. Define NoSQL and differentiate with relational databases (4 marks)

**Answer:**

**NoSQL** = **Not Only SQL** - A database that stores data in formats other than tables.

**Relational Database (SQL):**
- Data stored in **tables** (rows and columns)
- Fixed structure (schema)
- Uses SQL language
- Examples: MySQL, PostgreSQL

**NoSQL Database:**
- Data stored in **documents/key-value/graphs**
- Flexible structure (no fixed schema)
- Different query languages
- Examples: MongoDB, Redis, Cassandra

**Comparison Table:**

| Feature | SQL (Relational) | NoSQL (MongoDB) |
|---------|-----------------|-----------------|
| **Structure** | Tables (rows & columns) | Documents (JSON-like) |
| **Schema** | Fixed (predefined) | Flexible (dynamic) |
| **Scalability** | Vertical (bigger server) | Horizontal (more servers) |
| **Relationships** | Foreign keys, JOINs | Embedded documents |
| **Best For** | Banking, accounting | Social media, big data |
| **Example** | MySQL, Oracle | MongoDB, Cassandra |

**Example Comparison:**

**SQL (MySQL):**
```sql
-- Fixed table structure
CREATE TABLE users (
  id INT PRIMARY KEY,
  name VARCHAR(50),
  email VARCHAR(100)
);

INSERT INTO users VALUES (1, 'John', 'john@example.com');
```

**NoSQL (MongoDB):**
```javascript
// Flexible document structure
db.users.insertOne({
  _id: 1,
  name: "John",
  email: "john@example.com",
  // Can add any field anytime!
  phone: "123-456-7890"  // New field, no problem!
});
```

**Key Differences:**
- ✅ SQL = Structured, fixed schema
- ✅ NoSQL = Flexible, dynamic schema
- ✅ SQL = Vertical scaling
- ✅ NoSQL = Horizontal scaling

**Remember:** SQL = Tables, NoSQL = Documents

---

### 2. Define MongoDB document and explain _id field (4 marks)

**Answer:**

**MongoDB Document** = A record stored in JSON-like format (actually BSON - Binary JSON).

**What is a Document?**
Think of it like a JavaScript object with key-value pairs.

**Simple Example:**
```javascript
{
  _id: 1,
  name: "John Doe",
  age: 25,
  email: "john@example.com"
}
```

**More Complex Example:**
```javascript
{
  _id: ObjectId("507f1f77bcf86cd799439011"),
  title: "My Blog Post",
  author: "Alice",
  content: "This is my first post",
  tags: ["mongodb", "database", "nosql"],
  comments: [
    { user: "Bob", text: "Great post!" },
    { user: "Charlie", text: "Very helpful" }
  ],
  createdAt: ISODate("2024-02-09T10:30:00Z")
}
```

**Purpose of `_id` Field:**

**1. Unique Identifier:**
- Every document MUST have an `_id`
- Acts like a primary key
- Ensures no duplicate documents

**2. Auto-generated:**
```javascript
// If you don't provide _id, MongoDB creates one
db.users.insertOne({
  name: "John"
  // MongoDB auto-creates: _id: ObjectId("...")
});
```

**3. Can Be Custom:**
```javascript
// You can set your own _id
db.users.insertOne({
  _id: 1,  // Custom number
  name: "John"
});

db.users.insertOne({
  _id: "user_john",  // Custom string
  name: "John"
});
```

**4. ObjectId Structure:**
```javascript
ObjectId("507f1f77bcf86cd799439011")
         └─────┬─────┘
              │
    12-byte hexadecimal value

Contains:
- Timestamp (when created)
- Machine identifier
- Process ID
- Random counter
```

**Why _id is Important:**
- ✅ Fast lookups (indexed by default)
- ✅ Guarantees uniqueness
- ✅ Helps in sorting (by creation time)
- ✅ Required for updates/deletes

**Examples:**
```javascript
// Find by _id
db.users.findOne({ _id: 1 });

// Update by _id
db.users.updateOne(
  { _id: 1 },
  { $set: { name: "Jane" } }
);

// Delete by _id
db.users.deleteOne({ _id: 1 });
```

**Remember:** `_id` = Unique identifier (like a student roll number)

---

### 3. Write MongoDB queries to find posts (4 marks)

**Answer:**

```javascript
// Sample posts collection:
// { _id: 1, title: "Post 1", invoiceNumber: 500 }
// { _id: 2, title: "Post 2", invoiceNumber: 1200 }
// { _id: 3, title: "Post 3", invoiceNumber: 1500 }

// Query 1: Find all posts where invoiceNumber > 1000
db.posts.find({ invoiceNumber: { $gt: 1000 } });
// Returns: Post 2 and Post 3

// Query 2: Find posts where invoiceNumber >= 1200
db.posts.find({ invoiceNumber: { $gte: 1200 } });
// Returns: Post 2 and Post 3

// Query 3: Find posts where invoiceNumber is between 1000 and 2000
db.posts.find({ 
  invoiceNumber: { 
    $gt: 1000, 
    $lt: 2000 
  } 
});
// Returns: Post 2 and Post 3

// Query 4: Find posts where invoiceNumber > 1000 and show only title
db.posts.find(
  { invoiceNumber: { $gt: 1000 } },
  { title: 1, invoiceNumber: 1, _id: 0 }
);
// Returns: { title: "Post 2", invoiceNumber: 1200 }
//          { title: "Post 3", invoiceNumber: 1500 }
```

**Comparison Operators:**
```javascript
$gt   // Greater than (>)
$gte  // Greater than or equal (>=)
$lt   // Less than (<)
$lte  // Less than or equal (<=)
$eq   // Equal (=)
$ne   // Not equal (!=)
```

**Complete Examples:**
```javascript
// Greater than
db.posts.find({ invoiceNumber: { $gt: 1000 } });

// Less than
db.posts.find({ invoiceNumber: { $lt: 1000 } });

// Between (range)
db.posts.find({ 
  invoiceNumber: { $gte: 1000, $lte: 2000 } 
});

// Not equal
db.posts.find({ invoiceNumber: { $ne: 1000 } });
```

**Remember:** `$gt` = greater than, `$lt` = less than

---

### 4. Explain collections in MongoDB with example (5 marks)

**Answer:**

**Collection** = A group of MongoDB documents (like a table in SQL).

**Simple Analogy:**
- SQL Table = MongoDB Collection
- SQL Row = MongoDB Document
- SQL Column = MongoDB Field

**Characteristics:**
- No fixed schema (flexible structure)
- Can have different fields in each document
- Stored in a database
- Indexed for fast queries

**Example:**

```javascript
// Collection: users
db.users.insertMany([
  { 
    _id: 1, 
    name: "Alice", 
    age: 25,
    email: "alice@example.com"
  },
  { 
    _id: 2, 
    name: "Bob", 
    age: 30,
    email: "bob@example.com",
    phone: "123-456"  // Extra field - no problem!
  },
  { 
    _id: 3, 
    name: "Charlie", 
    age: 28
    // Missing email - no problem!
  }
]);
```

**Flexible Schema Example:**
```javascript
// Collection: products
db.products.insertMany([
  // Laptop document
  {
    name: "Laptop",
    price: 50000,
    brand: "Dell",
    specs: {
      ram: "16GB",
      storage: "512GB SSD"
    }
  },
  // Book document (completely different structure)
  {
    name: "Harry Potter",
    price: 500,
    author: "J.K. Rowling",
    pages: 400
  }
]);
```

**Working with Collections:**

**1. Create Collection:**
```javascript
// Implicit creation (when you insert)
db.students.insertOne({ name: "John" });
// Collection 'students' created automatically

// Explicit creation
db.createCollection("teachers");
```

**2. View Collections:**
```javascript
// Show all collections in database
show collections;
```

**3. Query Collection:**
```javascript
// Find all documents
db.users.find();

// Find specific documents
db.users.find({ age: 25 });

// Count documents
db.users.countDocuments();
```

**4. Drop Collection:**
```javascript
db.users.drop();
```

**Real-World Example - Blog System:**
```javascript
// Collection: posts
{
  _id: 1,
  title: "My First Blog",
  author: "Alice",
  content: "Hello world!",
  tags: ["intro", "blog"],
  comments: [
    { user: "Bob", text: "Nice!" },
    { user: "Charlie", text: "Great start!" }
  ]
}

// Collection: users
{
  _id: "alice",
  name: "Alice Smith",
  email: "alice@example.com",
  posts: [1, 2, 3]  // References to posts
}
```

**Collection vs Table:**

| SQL Table | MongoDB Collection |
|-----------|-------------------|
| Fixed columns | Flexible fields |
| All rows same structure | Documents can differ |
| Schema changes need migration | Add fields anytime |

**Remember:** Collection = Group of documents (like a folder of files)

---

### 5. List five key features of MongoDB (5 marks)

**Answer:**

**1. Document-Oriented Storage**

Documents stored in JSON-like format (BSON).

```javascript
{
  _id: 1,
  name: "John",
  skills: ["React", "Node.js"],
  address: {
    city: "Bangalore",
    pin: 560001
  }
}
```

**Benefits:**
- Easy to read and understand
- Maps naturally to objects in code
- Can nest data (no JOINs needed)

---

**2. Flexible Schema**

No fixed structure - each document can be different.

```javascript
// Document 1
{ name: "Alice", age: 25 }

// Document 2 - different fields, no problem!
{ name: "Bob", age: 30, city: "Delhi", phone: "123-456" }
```

**Benefits:**
- Quick development
- Easy to evolve
- No database migrations

---

**3. Scalability (Horizontal Scaling)**

Add more servers instead of upgrading one big server.

```
One Server (Vertical Scaling - SQL):
Server: [All Data] → Upgrade RAM/CPU → Expensive!

Multiple Servers (Horizontal Scaling - MongoDB):
Server1: [Data Part 1]
Server2: [Data Part 2]  → Add more servers → Cheaper!
Server3: [Data Part 3]
```

**Benefits:**
- Handle massive data
- Better performance
- Cost-effective

---

**4. Rich Query Language**

Powerful queries for finding and updating data.

```javascript
// Find
db.users.find({ age: { $gte: 25 } });

// Update
db.users.updateMany(
  { city: "Bangalore" },
  { $set: { country: "India" } }
);

// Aggregation
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $group: { _id: "$user", total: { $sum: "$amount" } } }
]);
```

**Benefits:**
- Complex queries
- Aggregation pipelines
- Text search
- Geospatial queries

---

**5. High Availability (Replica Sets)**

Data copied to multiple servers automatically.

```
Primary Server:  [Data] ──┐
                          ├─→ Automatic Replication
Secondary Server: [Data] ─┤
Secondary Server: [Data] ─┘

If Primary fails → Secondary becomes Primary (Automatic!)
```

**Benefits:**
- No data loss
- Automatic failover
- Always available
- Read from secondaries

---

**Summary:**
1. **Document-Oriented** - JSON-like storage
2. **Flexible Schema** - No fixed structure
3. **Horizontal Scaling** - Add more servers
4. **Rich Queries** - Powerful search & aggregation
5. **High Availability** - Replica sets for backup

---

### 6. CRUD Operations on Posts Collection (5 marks)

**Answer:**

```javascript
// ========== CREATE ==========

// a) Insert a new post with title and user
db.posts.insertOne({
  title: "Introduction to MongoDB",
  user: "alice",
  content: "MongoDB is a NoSQL database...",
  createdAt: new Date()
});

// Insert multiple posts
db.posts.insertMany([
  { title: "React Tutorial", user: "alice", views: 100 },
  { title: "Node.js Guide", user: "bob", views: 150 }
]);


// ========== READ ==========

// Find all posts
db.posts.find();

// Find posts by specific user
db.posts.find({ user: "alice" });


// ========== UPDATE ==========

// b) Update the title of posts by user "alice"
db.posts.updateMany(
  { user: "alice" },  // Filter: find posts by alice
  { $set: { title: "Updated Title" } }  // Update: change title
);

// Update single post
db.posts.updateOne(
  { user: "alice" },
  { $set: { title: "New Title" } }
);


// c) Save a post with a given _id
// If _id exists, it updates; if not, it inserts
db.posts.save({
  _id: 1,  // Specific _id
  title: "Saved Post",
  user: "charlie",
  content: "This is saved content"
});

// Or use insertOne with custom _id
db.posts.insertOne({
  _id: 1,
  title: "My Post",
  user: "alice"
});


// ========== DELETE ==========

// d) Delete posts by user "bob"
db.posts.deleteMany({ user: "bob" });

// Delete single post
db.posts.deleteOne({ user: "bob" });

// Delete all posts (be careful!)
db.posts.deleteMany({});
```

**Complete Example with Output:**

```javascript
// 1. INSERT
db.posts.insertOne({
  title: "MongoDB Basics",
  user: "alice",
  tags: ["database", "nosql"],
  likes: 0
});
// Output: { acknowledged: true, insertedId: ObjectId("...") }

// 2. FIND
db.posts.find({ user: "alice" });
// Output: { _id: ..., title: "MongoDB Basics", user: "alice", ... }

// 3. UPDATE
db.posts.updateMany(
  { user: "alice" },
  { $set: { title: "MongoDB Advanced" } }
);
// Output: { acknowledged: true, matchedCount: 1, modifiedCount: 1 }

// 4. DELETE
db.posts.deleteMany({ user: "bob" });
// Output: { acknowledged: true, deletedCount: 2 }
```

**Update Operators:**
```javascript
// $set - Set field value
{ $set: { title: "New Title" } }

// $inc - Increment number
{ $inc: { likes: 1 } }  // Increase likes by 1

// $push - Add to array
{ $push: { tags: "mongodb" } }

// $unset - Remove field
{ $unset: { likes: "" } }
```

**Remember:**
- **C**reate = `insertOne()` / `insertMany()`
- **R**ead = `find()` / `findOne()`
- **U**pdate = `updateOne()` / `updateMany()`
- **D**elete = `deleteOne()` / `deleteMany()`

---

### 7. BSON Format in MongoDB (5 marks)

**Answer:**

**BSON** = **Binary JSON** - MongoDB's internal storage format.

**What is BSON?**
- JSON stored in binary format
- More data types than JSON
- Faster to parse and process

**JSON vs BSON:**

**JSON (Text Format):**
```json
{
  "name": "John",
  "age": 25,
  "active": true
}
```

**BSON (Binary Format):**
```
\x16\x00\x00\x00           // document length
\x02name\x00\x05\x00...    // string field
\x10age\x00\x19\x00...     // 32-bit integer
\x08active\x00\x01         // boolean
\x00                       // end of document
```

**Advantages of BSON over JSON:**

**1. More Data Types:**

| JSON | BSON |
|------|------|
| String | String |
| Number | Int32, Int64, Double, Decimal128 |
| Boolean | Boolean |
| ❌ No Date | ✅ Date |
| ❌ No Binary | ✅ Binary Data |
| ❌ No ObjectId | ✅ ObjectId |

**Examples:**
```javascript
// BSON supports more types
{
  _id: ObjectId("507f1f77bcf86cd799439011"),  // ObjectId
  name: "John",
  createdAt: ISODate("2024-02-09"),  // Date
  profilePic: BinData(0, "..."),  // Binary data
  age: NumberInt(25),  // Specific number types
  salary: NumberDecimal("50000.50")  // Decimal
}
```

**2. Faster Processing:**
- Binary format = faster to read/write
- Efficient traversal (knows field lengths)
- Better performance

**3. Better for Storage:**
- Compact binary representation
- Includes metadata (type, length)
- Efficient indexing

**4. Supports Complex Data:**
```javascript
{
  // Nested documents
  address: {
    street: "123 Main St",
    city: "Bangalore"
  },
  
  // Arrays
  hobbies: ["reading", "coding"],
  
  // Binary data (images, files)
  photo: BinData(0, "base64data..."),
  
  // Dates
  createdAt: ISODate("2024-02-09T10:30:00Z")
}
```

**5. Type Preservation:**
```javascript
// JSON loses type information
{ "age": 25 }  // Is it Int32 or Int64 or Double?

// BSON preserves exact type
{ "age": NumberInt(25) }  // Definitely Int32
```

**Real Example:**
```javascript
// Inserting with BSON types
db.users.insertOne({
  _id: ObjectId(),  // BSON ObjectId
  name: "Alice",
  age: NumberInt(25),  // 32-bit integer
  salary: NumberDecimal("75000.50"),  // Decimal (for money)
  joined: new Date(),  // BSON Date
  photo: BinData(0, "base64encodeddata"),  // Binary
  active: true
});
```

**Summary:**
- BSON = Binary JSON
- More data types (Date, ObjectId, Binary)
- Faster processing
- Better for databases
- Used internally by MongoDB

**Remember:** BSON = Better version of JSON for databases

---

### 8. MongoDB Queries on Courses Collection (5 marks)

**Answer:**

```javascript
// Sample courses collection:
db.courses.insertMany([
  { title: "MongoDB Basics", duration: 8, instructor: "Dr. Kumar" },
  { title: "React Advanced", duration: 10, instructor: "Dr. Meera" },
  { title: "Node.js", duration: 6, instructor: "Dr. Kumar" },
  { title: "Python", duration: 12, instructor: "Dr. Sharma" },
  { title: "JavaScript", duration: 5, instructor: "Dr. Meera" }
]);

// Query 1: Find courses with duration ≥ 6 weeks AND instructor = "Dr. Kumar"
db.courses.find({
  duration: { $gte: 6 },
  instructor: "Dr. Kumar"
});

// Output:
// { title: "MongoDB Basics", duration: 8, instructor: "Dr. Kumar" }
// { title: "Node.js", duration: 6, instructor: "Dr. Kumar" }


// Query 2: Find courses with duration > 8 weeks AND instructor = "Dr. Meera"
db.courses.find({
  duration: { $gt: 8 },
  instructor: "Dr. Meera"
});

// Output:
// { title: "React Advanced", duration: 10, instructor: "Dr. Meera" }
```

**With Projection (show only specific fields):**

```javascript
// Show only title and duration
db.courses.find(
  {
    duration: { $gte: 6 },
    instructor: "Dr. Kumar"
  },
  {
    title: 1,
    duration: 1,
    _id: 0  // Hide _id
  }
);

// Output:
// { title: "MongoDB Basics", duration: 8 }
// { title: "Node.js", duration: 6 }
```

**Multiple Conditions (OR):**

```javascript
// Find courses by Dr. Kumar OR Dr. Meera with duration ≥ 6
db.courses.find({
  duration: { $gte: 6 },
  $or: [
    { instructor: "Dr. Kumar" },
    { instructor: "Dr. Meera" }
  ]
});
```

**Sorting Results:**

```javascript
// Sort by duration (ascending)
db.courses.find({
  duration: { $gte: 6 },
  instructor: "Dr. Kumar"
}).sort({ duration: 1 });

// Sort by duration (descending)
db.courses.find({
  duration: { $gte: 6 }
}).sort({ duration: -1 });
```

**Count Results:**

```javascript
// Count courses matching criteria
db.courses.countDocuments({
  duration: { $gte: 6 },
  instructor: "Dr. Kumar"
});
// Output: 2
```

**Complete Example:**

```javascript
// Find, project, sort, and limit
db.courses.find(
  { 
    duration: { $gte: 6 },
    instructor: "Dr. Kumar"
  },
  {
    title: 1,
    duration: 1,
    _id: 0
  }
)
.sort({ duration: -1 })
.limit(5);
```

**Remember:** 
- `$gte` = greater than or equal (≥)
- `$gt` = greater than (>)
- Multiple conditions in same `{}` = AND
- `$or` operator = OR logic

---

## Part 2: Mongoose (ODM - Object Data Modeling)

### 1. Connect Mongoose to MongoDB (4 marks)

**Answer:**

**Mongoose** = Library to work with MongoDB in Node.js (makes it easier!).

**Basic Connection:**

```javascript
// Import mongoose
const mongoose = require('mongoose');

// Connect to database
mongoose.connect('mongodb://localhost:27017/studentDB');

// OR with async/await
async function connectDB() {
  await mongoose.connect('mongodb://localhost:27017/studentDB');
  console.log('Connected to MongoDB!');
}
connectDB();
```

**Complete Connection with Error Handling:**

```javascript
const mongoose = require('mongoose');

// Database connection string
const dbURL = 'mongodb://localhost:27017/studentDB';

// Connect
mongoose.connect(dbURL)
  .then(() => {
    console.log('✓ Connected to studentDB successfully!');
  })
  .catch((error) => {
    console.error('✗ Connection failed:', error);
  });

// Listen for connection events
mongoose.connection.on('connected', () => {
  console.log('Mongoose connected to studentDB');
});

mongoose.connection.on('error', (err) => {
  console.error('Mongoose connection error:', err);
});

mongoose.connection.on('disconnected', () => {
  console.log('Mongoose disconnected');
});
```

**With Options (Recommended):**

```javascript
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/studentDB', {
  useNewUrlParser: true,
  useUnifiedTopology: true
})
.then(() => console.log('Connected!'))
.catch((err) => console.error('Error:', err));
```

**MongoDB Atlas (Cloud) Connection:**

```javascript
const mongoose = require('mongoose');

// MongoDB Atlas connection string
const dbURL = 'mongodb+srv://username:password@cluster.mongodb.net/studentDB';

mongoose.connect(dbURL)
  .then(() => console.log('Connected to MongoDB Atlas!'))
  .catch((err) => console.error('Connection failed:', err));
```

**Complete Example:**

```javascript
// db.js
const mongoose = require('mongoose');

const connectDB = async () => {
  try {
    await mongoose.connect('mongodb://localhost:27017/studentDB');
    console.log('✓ MongoDB Connected Successfully!');
  } catch (error) {
    console.error('✗ MongoDB Connection Error:', error.message);
    process.exit(1); // Exit process with failure
  }
};

module.exports = connectDB;

// app.js
const connectDB = require('./db');

// Connect to database
connectDB();

// Now you can use Mongoose models
```

**Breakdown:**
```javascript
mongoose.connect('mongodb://localhost:27017/studentDB');
                  └──┬──┘  └─┬─┘ └─┬─┘ └────┬────┘
                     │       │     │        │
                 Protocol  Host  Port   Database Name
```

**Remember:** 
- `mongoose.connect()` = Connect to MongoDB
- Format: `mongodb://host:port/databaseName`
- Always handle errors!

---

### 2. Create User Schema and Model (5 marks)

**Answer:**

**Schema** = Blueprint/structure for your documents.
**Model** = Class to create and query documents.

**Step-by-Step:**

```javascript
// 1. Import mongoose
const mongoose = require('mongoose');

// 2. Define Schema
const userSchema = new mongoose.Schema({
  firstName: {
    type: String,
    required: true  // This field is required
  },
  lastName: {
    type: String,
    required: true
  },
  email: {
    type: String,
    required: true,
    unique: true  // No duplicate emails
  },
  username: {
    type: String,
    required: true,
    unique: true
  },
  password: {
    type: String,
    required: true,
    minlength: 6  // At least 6 characters
  }
});

// 3. Create Model
const User = mongoose.model('User', userSchema);

// 4. Export model
module.exports = User;
```

**Complete Example with All Features:**

```javascript
const mongoose = require('mongoose');

// Define User Schema
const userSchema = new mongoose.Schema({
  firstName: {
    type: String,
    required: [true, 'First name is required'],
    trim: true  // Remove whitespace
  },
  
  lastName: {
    type: String,
    required: [true, 'Last name is required'],
    trim: true
  },
  
  email: {
    type: String,
    required: [true, 'Email is required'],
    unique: true,
    lowercase: true,  // Convert to lowercase
    match: [/^\S+@\S+\.\S+$/, 'Invalid email format']  // Validation
  },
  
  username: {
    type: String,
    required: [true, 'Username is required'],
    unique: true,
    minlength: [3, 'Username must be at least 3 characters'],
    maxlength: [20, 'Username cannot exceed 20 characters']
  },
  
  password: {
    type: String,
    required: [true, 'Password is required'],
    minlength: [6, 'Password must be at least 6 characters']
  },
  
  createdAt: {
    type: Date,
    default: Date.now  // Automatically set creation date
  },
  
  isActive: {
    type: Boolean,
    default: true
  }
}, {
  timestamps: true  // Adds createdAt and updatedAt automatically
});

// Create Model
const User = mongoose.model('User', userSchema);

module.exports = User;
```

**Using the Model:**

```javascript
// Import the model
const User = require('./models/User');

// Create a new user
const newUser = new User({
  firstName: 'John',
  lastName: 'Doe',
  email: 'john@example.com',
  username: 'johndoe',
  password: 'secret123'
});

// Save to database
await newUser.save();
console.log('User created!');
```

**Data Types:**

```javascript
const schema = new mongoose.Schema({
  name: String,           // Text
  age: Number,           // Number
  isActive: Boolean,     // true/false
  joinDate: Date,        // Date
  skills: [String],      // Array of strings
  address: {             // Nested object
    street: String,
    city: String
  },
  userId: mongoose.Schema.Types.ObjectId  // Reference to another document
});
```

**Schema Validators:**

```javascript
{
  email: {
    type: String,
    required: true,        // Field is required
    unique: true,          // No duplicates
    lowercase: true,       // Convert to lowercase
    trim: true,            // Remove whitespace
    minlength: 5,          // Minimum length
    maxlength: 100,        // Maximum length
    match: /regex/,        // Pattern matching
    enum: ['admin', 'user'], // Only these values allowed
    default: 'user'        // Default value
  }
}
```

**Remember:**
- Schema = Structure/blueprint
- Model = Working class from schema
- `mongoose.model('Name', schema)` creates model

---

### 3. User CRUD Operations with Mongoose (10 marks)

**Answer:**

```javascript
const mongoose = require('mongoose');
const User = require('./models/User');  // Import User model

// Connect to database
mongoose.connect('mongodb://localhost:27017/studentDB');


// ========== CREATE ==========

// a) Insert a new user using save()
const createUser = async () => {
  try {
    // Method 1: Create and save
    const newUser = new User({
      firstName: 'John',
      lastName: 'Doe',
      email: 'john@example.com',
      username: 'johndoe',
      password: 'password123'
    });
    
    const savedUser = await newUser.save();
    console.log('User created:', savedUser);
    
    // Method 2: Create in one step
    const user2 = await User.create({
      firstName: 'Jane',
      lastName: 'Smith',
      email: 'jane@example.com',
      username: 'janesmith',
      password: 'password456'
    });
    console.log('User created:', user2);
    
  } catch (error) {
    console.error('Error creating user:', error.message);
  }
};


// ========== READ ==========

// b) Find users with username = "johndoe"
const findUsers = async () => {
  try {
    // Find all matching users
    const users = await User.find({ username: 'johndoe' });
    console.log('Found users:', users);
    
    // Find one user
    const user = await User.findOne({ username: 'johndoe' });
    console.log('Found user:', user);
    
    // Find by ID
    const userById = await User.findById('507f1f77bcf86cd799439011');
    console.log('User by ID:', userById);
    
  } catch (error) {
    console.error('Error finding users:', error.message);
  }
};


// ========== UPDATE ==========

// c) Update a user's email
const updateUser = async () => {
  try {
    // Method 1: Find and update
    const updatedUser = await User.findOneAndUpdate(
      { username: 'johndoe' },  // Find criteria
      { email: 'newemail@example.com' },  // Update
      { new: true }  // Return updated document
    );
    console.log('Updated user:', updatedUser);
    
    // Method 2: Update by ID
    const user = await User.findByIdAndUpdate(
      '507f1f77bcf86cd799439011',
      { email: 'updated@example.com' },
      { new: true }
    );
    console.log('Updated user:', user);
    
    // Method 3: Update multiple users
    const result = await User.updateMany(
      { lastName: 'Doe' },  // Find all Doe's
      { isActive: true }     // Set them active
    );
    console.log('Updated count:', result.modifiedCount);
    
  } catch (error) {
    console.error('Error updating user:', error.message);
  }
};


// ========== DELETE ==========

// d) Delete a user by _id
const deleteUser = async () => {
  try {
    // Method 1: Delete by ID
    const deletedUser = await User.findByIdAndDelete('507f1f77bcf86cd799439011');
    console.log('Deleted user:', deletedUser);
    
    // Method 2: Delete by criteria
    const result = await User.deleteOne({ username: 'johndoe' });
    console.log('Deleted:', result.deletedCount);
    
    // Method 3: Delete multiple
    const multiResult = await User.deleteMany({ isActive: false });
    console.log('Deleted count:', multiResult.deletedCount);
    
  } catch (error) {
    console.error('Error deleting user:', error.message);
  }
};


// ========== COMPLETE EXAMPLE ==========

const performCRUD = async () => {
  try {
    // CREATE
    const user = await User.create({
      firstName: 'Alice',
      lastName: 'Johnson',
      email: 'alice@example.com',
      username: 'alice',
      password: 'secret123'
    });
    console.log('Created:', user);
    
    // READ
    const foundUser = await User.findOne({ username: 'alice' });
    console.log('Found:', foundUser);
    
    // UPDATE
    foundUser.email = 'alice.new@example.com';
    await foundUser.save();
    console.log('Updated:', foundUser);
    
    // DELETE
    await User.deleteOne({ username: 'alice' });
    console.log('Deleted!');
    
  } catch (error) {
    console.error('Error:', error.message);
  }
};

// Run the functions
createUser();
findUsers();
updateUser();
deleteUser();
```

**Query Methods Summary:**

```javascript
// CREATE
User.create({ ... })           // Create and save
new User({ ... }).save()       // Create then save

// READ
User.find({ ... })             // Find all matching
User.findOne({ ... })          // Find first match
User.findById(id)              // Find by _id

// UPDATE
User.findByIdAndUpdate(id, update, options)
User.findOneAndUpdate(filter, update, options)
User.updateMany(filter, update)

// DELETE
User.findByIdAndDelete(id)
User.deleteOne(filter)
User.deleteMany(filter)
```

**Options:**

```javascript
// Return updated document
{ new: true }

// Run validators on update
{ runValidators: true }

// Create if not exists
{ upsert: true }
```

**Remember:**
- **C**reate: `save()`, `create()`
- **R**ead: `find()`, `findOne()`, `findById()`
- **U**pdate: `findByIdAndUpdate()`, `updateMany()`
- **D**elete: `findByIdAndDelete()`, `deleteMany()`

---

### 4. Student Schema with Custom Modifiers (10 marks)

**Answer:**

```javascript
const mongoose = require('mongoose');

// Define Student Schema
const studentSchema = new mongoose.Schema({
  // a) Define schema structure with appropriate data types
  name: {
    type: String,
    required: [true, 'Name is required'],
    trim: true
  },
  
  rollNo: {
    type: String,
    required: [true, 'Roll number is required'],
    unique: true,
    // c) Custom setter to prepend "Roll-" to rollNo
    set: function(value) {
      // If already has "Roll-", don't add again
      if (value.startsWith('Roll-')) {
        return value;
      }
      return 'Roll-' + value;
    }
  },
  
  course: {
    type: String,
    required: [true, 'Course is required'],
    enum: ['BCA', 'MCA', 'BSc', 'MSc']  // Only these values allowed
  },
  
  marks: {
    type: Number,
    required: [true, 'Marks are required'],
    min: [0, 'Marks cannot be negative'],
    max: [100, 'Marks cannot exceed 100'],
    // d) Custom getter to append " marks" to marks
    get: function(value) {
      return value + ' marks';
    }
  },
  
  // b) Default value for created as Date.now
  created: {
    type: Date,
    default: Date.now
  }
}, {
  // Enable getters when converting to JSON
  toJSON: { getters: true },
  toObject: { getters: true }
});

// Create Model
const Student = mongoose.model('Student', studentSchema);

// ========== USAGE EXAMPLES ==========

// Example 1: Create student
const createStudent = async () => {
  const student = await Student.create({
    name: 'John Doe',
    rollNo: '001',  // Will become "Roll-001"
    course: 'MCA',
    marks: 85  // Will display as "85 marks"
  });
  
  console.log('Created:', student);
  // Output:
  // {
  //   name: 'John Doe',
  //   rollNo: 'Roll-001',  ← Setter applied
  //   course: 'MCA',
  //   marks: '85 marks',   ← Getter applied
  //   created: 2024-02-09T10:30:00.000Z
  // }
};

// e) Query to increase marks by 5 for all MCA students
const increaseMarks = async () => {
  const result = await Student.updateMany(
    { course: 'MCA' },  // Filter: MCA students
    { $inc: { marks: 5 } }  // Increment marks by 5
  );
  
  console.log('Updated students:', result.modifiedCount);
};

// f) Find students with marks > 80, show only name and marks
const findTopStudents = async () => {
  const students = await Student.find(
    { marks: { $gt: 80 } },  // Filter: marks > 80
    { name: 1, marks: 1, _id: 0 }  // Projection: show name and marks only
  );
  
  console.log('Top students:', students);
  // Output:
  // [
  //   { name: 'John Doe', marks: '90 marks' },
  //   { name: 'Jane Smith', marks: '85 marks' }
  // ]
};

module.exports = Student;
```

**Complete Working Example:**

```javascript
const mongoose = require('mongoose');
const Student = require('./models/Student');

// Connect to database
mongoose.connect('mongodb://localhost:27017/studentDB');

const runExamples = async () => {
  try {
    // CREATE - Insert students
    await Student.create([
      { name: 'Alice', rollNo: '001', course: 'MCA', marks: 85 },
      { name: 'Bob', rollNo: '002', course: 'MCA', marks: 75 },
      { name: 'Charlie', rollNo: '003', course: 'BCA', marks: 90 },
      { name: 'David', rollNo: '004', course: 'MCA', marks: 65 }
    ]);
    console.log('✓ Students created');
    
    // READ - Find all students
    const allStudents = await Student.find();
    console.log('All students:', allStudents);
    
    // UPDATE - Increase marks by 5 for MCA students
    const updateResult = await Student.updateMany(
      { course: 'MCA' },
      { $inc: { marks: 5 } }
    );
    console.log(`✓ Updated ${updateResult.modifiedCount} students`);
    
    // READ - Find top students (marks > 80)
    const topStudents = await Student.find(
      { marks: { $gt: 80 } },
      { name: 1, marks: 1, rollNo: 1, _id: 0 }
    ).sort({ marks: -1 });  // Sort descending
    
    console.log('Top students:');
    topStudents.forEach(student => {
      console.log(`${student.rollNo} - ${student.name}: ${student.marks}`);
    });
    // Output:
    // Roll-003 - Charlie: 90 marks
    // Roll-001 - Alice: 90 marks (85 + 5)
    
  } catch (error) {
    console.error('Error:', error.message);
  } finally {
    mongoose.connection.close();
  }
};

runExamples();
```

**How Getters and Setters Work:**

```javascript
// SETTER (runs when saving)
rollNo: {
  set: function(value) {
    return 'Roll-' + value;
  }
}

// Input:  rollNo: '001'
// Stored: rollNo: 'Roll-001'

// GETTER (runs when retrieving)
marks: {
  get: function(value) {
    return value + ' marks';
  }
}

// Stored:   marks: 85
// Retrieved: marks: '85 marks'
```

**Enabling Getters:**

```javascript
// Method 1: In schema options
const schema = new mongoose.Schema({
  // fields...
}, {
  toJSON: { getters: true },
  toObject: { getters: true }
});

// Method 2: When querying
const student = await Student.findOne().lean({ getters: true });
```

**Summary:**
- ✅ Custom setter prepends "Roll-" to rollNo
- ✅ Custom getter appends " marks" to marks
- ✅ Default value for created field
- ✅ Query to increase marks with `$inc`
- ✅ Projection to show specific fields

---

### 5. Employee Schema with Salary Update (10 marks)

**Answer:**

```javascript
const mongoose = require('mongoose');

// a) Define Employee Schema
const employeeSchema = new mongoose.Schema({
  empName: {
    type: String,
    required: [true, 'Employee name is required'],
    trim: true
  },
  
  empId: {
    type: String,
    required: [true, 'Employee ID is required'],
    unique: true,
    // c) Custom setter to prepend "EMP-"
    set: function(value) {
      if (value.startsWith('EMP-')) {
        return value;
      }
      return 'EMP-' + value;
    }
  },
  
  department: {
    type: String,
    // b) Default value for department
    default: 'General',
    enum: ['IT', 'HR', 'Finance', 'Sales', 'General']
  },
  
  salary: {
    type: Number,
    required: [true, 'Salary is required'],
    min: [0, 'Salary cannot be negative'],
    // d) Custom getter to append " INR"
    get: function(value) {
      return value + ' INR';
    }
  },
  
  joinDate: {
    type: Date,
    default: Date.now
  }
}, {
  toJSON: { getters: true },
  toObject: { getters: true }
});

// Create Model
const Employee = mongoose.model('Employee', employeeSchema);

module.exports = Employee;


// ========== USAGE EXAMPLES ==========

const mongoose = require('mongoose');
const Employee = require('./models/Employee');

mongoose.connect('mongodb://localhost:27017/companyDB');

const performOperations = async () => {
  try {
    // CREATE - Insert employees
    await Employee.create([
      { empName: 'Alice Johnson', empId: '001', department: 'IT', salary: 50000 },
      { empName: 'Bob Smith', empId: '002', department: 'IT', salary: 55000 },
      { empName: 'Charlie Brown', empId: '003', department: 'HR', salary: 45000 },
      { empName: 'David Lee', empId: '004', department: 'IT', salary: 70000 },
      { empName: 'Eve Davis', empId: '005', salary: 40000 }  // Uses default dept
    ]);
    console.log('✓ Employees created');
    
    // e) Increase salary by 10% for IT department
    const updateResult = await Employee.updateMany(
      { department: 'IT' },
      [  // Use aggregation pipeline for percentage calculation
        {
          $set: {
            salary: { $multiply: ['$salary', 1.10] }  // salary * 1.10 (10% increase)
          }
        }
      ]
    );
    console.log(`✓ Updated ${updateResult.modifiedCount} employees`);
    
    // f) Find employees earning > 60,000 (project only name and salary)
    const highEarners = await Employee.find(
      { salary: { $gt: 60000 } },
      { empName: 1, salary: 1, _id: 0 }
    ).sort({ salary: -1 });
    
    console.log('\nHigh Earners (salary > 60,000):');
    highEarners.forEach(emp => {
      console.log(`${emp.empName}: ${emp.salary}`);
    });
    // Output:
    // David Lee: 77000 INR    (70000 * 1.10)
    // Bob Smith: 60500 INR    (55000 * 1.10)
    
    // Additional queries
    
    // Find IT employees
    const itEmployees = await Employee.find({ department: 'IT' });
    console.log('\nIT Department:', itEmployees);
    
    // Average salary by department
    const avgSalary = await Employee.aggregate([
      {
        $group: {
          _id: '$department',
          averageSalary: { $avg: '$salary' },
          count: { $sum: 1 }
        }
      },
      {
        $sort: { averageSalary: -1 }
      }
    ]);
    console.log('\nAverage Salary by Department:', avgSalary);
    
  } catch (error) {
    console.error('Error:', error.message);
  } finally {
    mongoose.connection.close();
  }
};

performOperations();
```

**Alternative Salary Update (Simple Way):**

```javascript
// e) Increase salary by 10% - Simple approach
const increaseITSalary = async () => {
  // Get all IT employees
  const itEmployees = await Employee.find({ department: 'IT' });
  
  // Update each one
  for (let emp of itEmployees) {
    emp.salary = emp.salary * 1.10;  // Increase by 10%
    await emp.save();
  }
  
  console.log(`Updated ${itEmployees.length} employees`);
};
```

**Using $mul Operator:**

```javascript
// e) Increase salary by 10% using $mul
const increaseITSalary = async () => {
  const result = await Employee.updateMany(
    { department: 'IT' },
    { $mul: { salary: 1.10 } }  // Multiply salary by 1.10
  );
  
  console.log(`Updated ${result.modifiedCount} employees`);
};
```

**Complete Output Example:**

```javascript
// After running all operations:

console.log('All Employees:');
const employees = await Employee.find();
employees.forEach(emp => {
  console.log(emp);
});

// Output:
// {
//   empName: 'Alice Johnson',
//   empId: 'EMP-001',        ← Setter applied
//   department: 'IT',
//   salary: '55000 INR',     ← Getter applied (50000 * 1.10)
//   joinDate: 2024-02-09T...
// }
// {
//   empName: 'Eve Davis',
//   empId: 'EMP-005',
//   department: 'General',   ← Default value
//   salary: '40000 INR',
//   joinDate: 2024-02-09T...
// }
```

**Key Concepts:**
- ✅ Custom setter: `empId` gets "EMP-" prefix
- ✅ Custom getter: `salary` gets " INR" suffix
- ✅ Default value: `department` defaults to "General"
- ✅ Update with percentage: Use `$multiply` or `$mul`
- ✅ Projection: Show only specific fields

**Remember:**
- `$inc` = Increase by fixed amount
- `$mul` = Multiply by value
- `$set` with `$multiply` = Calculate percentage

---

## BONUS: Complex Aggregation Queries

### Products Query with Multiple Conditions (6 marks)

**Answer:**

```javascript
// Find products where:
// - price >= 20 AND price < 50
// - category is 'Electronics' OR 'Home Goods'
// - on-sale field exists and is true

db.products.find({
  // Price condition (AND)
  price: { 
    $gte: 20,  // Greater than or equal to 20
    $lt: 50    // Less than 50
  },
  
  // Category condition (OR)
  category: { 
    $in: ['Electronics', 'Home Goods'] 
  },
  
  // on-sale exists and is true
  'on-sale': { 
    $exists: true,
    $eq: true
  }
});
```

**Step-by-Step Breakdown:**

```javascript
// Step 1: Price between 20 and 50
{ price: { $gte: 20, $lt: 50 } }

// Step 2: Category is Electronics OR Home Goods
{ category: { $in: ['Electronics', 'Home Goods'] } }

// Step 3: on-sale field exists and is true
{ 'on-sale': { $exists: true, $eq: true } }

// Combined (all conditions must be true)
db.products.find({
  price: { $gte: 20, $lt: 50 },
  category: { $in: ['Electronics', 'Home Goods'] },
  'on-sale': { $exists: true, $eq: true }
});
```

**Sample Data:**

```javascript
// These match:
{ name: 'Headphones', price: 30, category: 'Electronics', 'on-sale': true }
{ name: 'Lamp', price: 25, category: 'Home Goods', 'on-sale': true }

// These don't match:
{ name: 'TV', price: 500, category: 'Electronics', 'on-sale': true }  // price too high
{ name: 'Book', price: 30, category: 'Books', 'on-sale': true }  // wrong category
{ name: 'Mouse', price: 30, category: 'Electronics', 'on-sale': false }  // not on sale
{ name: 'Keyboard', price: 30, category: 'Electronics' }  // no on-sale field
```

**Remember:**
- `$gte` = ≥ (greater than or equal)
- `$lt` = < (less than)
- `$in` = OR for multiple values
- `$exists` = field must exist

---

### Sales Aggregation Query (6 marks)

**Answer:**

```javascript
db.sales.aggregate([
  // Stage 1: MATCH - Filter documents
  {
    $match: {
      // (region = "North" OR "South")
      region: { $in: ['North', 'South'] },
      
      // AND amount > 5000
      amount: { $gt: 5000 },
      
      // AND status ≠ "cancelled"
      status: { $ne: 'cancelled' }
    }
  },
  
  // Stage 2: GROUP - Calculate totals
  {
    $group: {
      _id: '$region',  // Group by region
      totalSales: { $sum: '$amount' },  // Sum all amounts
      averageSales: { $avg: '$amount' }  // Average amount
    }
  },
  
  // Stage 3: PROJECT - Shape output
  {
    $project: {
      _id: 0,  // Hide _id
      region: '$_id',  // Rename _id to region
      totalSales: 1,
      averageSales: 1
    }
  },
  
  // Stage 4: SORT - Order results
  {
    $sort: { totalSales: -1 }  // Descending order
  }
]);
```

**Sample Data and Output:**

```javascript
// Input documents:
{ region: 'North', amount: 6000, status: 'completed' }
{ region: 'North', amount: 7000, status: 'completed' }
{ region: 'South', amount: 8000, status: 'completed' }
{ region: 'North', amount: 3000, status: 'completed' }  // Filtered (amount < 5000)
{ region: 'North', amount: 6500, status: 'cancelled' }  // Filtered (cancelled)
{ region: 'East', amount: 7000, status: 'completed' }   // Filtered (wrong region)

// Output:
[
  {
    region: 'North',
    totalSales: 13000,  // 6000 + 7000
    averageSales: 6500  // (6000 + 7000) / 2
  },
  {
    region: 'South',
    totalSales: 8000,
    averageSales: 8000
  }
]
```

**Aggregation Pipeline Stages:**

```javascript
// 1. $match - Filter (like find())
{ $match: { field: value } }

// 2. $group - Group and calculate
{ $group: { _id: '$field', total: { $sum: '$amount' } } }

// 3. $project - Select/rename fields
{ $project: { field: 1, newName: '$oldName' } }

// 4. $sort - Order results
{ $sort: { field: 1 } }  // 1 = ascending, -1 = descending
```

**Remember:**
- Aggregation = Multi-stage data processing
- `$match` → `$group` → `$project` → `$sort`
- `$sum` = Total, `$avg` = Average

---

This comprehensive guide covers all MongoDB and Mongoose concepts with simple, practical examples perfect for beginners!
