# MongoDB Library Management System

## Overview
This project implements a library management system using MongoDB, featuring book management and an e-commerce platform integration. The system demonstrates fundamental MongoDB operations, data modeling, and best practices for database management.

## Prerequisites
- MongoDB (version 2.0 or higher)
- MongoDB Shell (mongosh)
- Optional: MongoDB Compass for GUI-based interaction

## Installation

1. Install MongoDB Community Edition:
   ```bash
   # For Ubuntu
   sudo apt-get install mongodb

   # For MacOS using Homebrew
   brew install mongodb-community

   # For Windows
   # Download and install from MongoDB website
   ```

2. Verify installation:
   ```bash
   mongo --version
   ```

3. Start MongoDB service:
   ```bash
   # For Ubuntu
   sudo service mongodb start

   # For MacOS
   brew services start mongodb-community

   # For Windows
   net start MongoDB
   ```

## Database Setup

1. Connect to MongoDB:
   ```bash
   mongo
   ```

2. Create and use the library database:
   ```javascript
   use library
   ```

3. Create required collections:
   ```javascript
   db.createCollection("books")
   db.createCollection("users")
   db.createCollection("products")
   db.createCollection("orders")
   ```

## Project Structure

```
library-management-system/
├── scripts/
│   ├── init-db.js        # Database initialization script
│   ├── insert-data.js    # Sample data insertion script
│   └── queries.js        # Common queries and operations
├── models/
│   ├── book-model.js     # Book schema and operations
│   ├── user-model.js     # User schema and operations
│   └── order-model.js    # Order schema and operations
└── README.md
```

## Data Models

### Books Collection
```javascript
{
  _id: ObjectId,
  title: String,
  author: String,
  publishedYear: Number,
  genre: String,
  ISBN: String,
  rating: Number
}
```

### Users Collection
```javascript
{
  _id: ObjectId,
  username: String,
  email: String,
  address: {
    street: String,
    city: String,
    state: String,
    zip: String
  },
  createdAt: Date
}
```

### Products Collection
```javascript
{
  _id: ObjectId,
  name: String,
  price: Number,
  description: String,
  category: String,
  stock: Number,
  createdAt: Date
}
```

### Orders Collection
```javascript
{
  _id: ObjectId,
  userId: ObjectId,
  products: [{
    productId: ObjectId,
    quantity: Number,
    price: Number
  }],
  totalAmount: Number,
  status: String,
  createdAt: Date
}
```

## Basic Operations

### Insert Books
```javascript
db.books.insertMany([
  {
    title: "Book Title",
    author: "Author Name",
    publishedYear: 2024,
    genre: "Genre",
    ISBN: "ISBN-13"
  }
])
```

### Query Books
```javascript
// Find all books
db.books.find()

// Find by author
db.books.find({ author: "Author Name" })

// Find by publication year range
db.books.find({ publishedYear: { $gt: 2000 } })
```

### Update Books
```javascript
// Update single book
db.books.updateOne(
  { ISBN: "ISBN-13" },
  { $set: { rating: 5 } }
)

// Update multiple books
db.books.updateMany(
  { genre: "Fiction" },
  { $inc: { rating: 1 } }
)
```

### Delete Books
```javascript
// Delete single book
db.books.deleteOne({ ISBN: "ISBN-13" })

// Delete multiple books
db.books.deleteMany({ genre: "Unwanted Genre" })
```

## Indexing
The following indexes are created for optimization:

```javascript
// Author index for quick author searches
db.books.createIndex({ author: 1 })

// Compound index for genre and year queries
db.books.createIndex({ genre: 1, publishedYear: -1 })

// Text index for title searches
db.books.createIndex({ title: "text" })
```

## Aggregation Examples

### Books per Genre
```javascript
db.books.aggregate([
  { $group: { _id: "$genre", totalBooks: { $sum: 1 } } }
])
```

### Average Publication Year
```javascript
db.books.aggregate([
  { $group: { 
    _id: null, 
    averageYear: { $avg: "$publishedYear" } 
  } }
])
```

## Performance Considerations

1. Indexes are created on frequently queried fields
2. Compound indexes for common query patterns
3. Text indexes for full-text search capabilities
4. Appropriate data modeling choices for scalability

## Error Handling

The system implements proper error handling for:
- Duplicate ISBN entries
- Invalid data formats
- Missing required fields
- Connection issues

## Monitoring and Maintenance

1. Check database status:
   ```javascript
   db.serverStatus()
   ```

2. View collection statistics:
   ```javascript
   db.books.stats()
   ```

3. Review indexes:
   ```javascript
   db.books.getIndexes()
   ```

## Backup and Recovery

1. Create backup:
   ```bash
   mongodump --db library --out /backup/path
   ```

2. Restore from backup:
   ```bash
   mongorestore --db library /backup/path/library
   ```

## Troubleshooting

Common issues and solutions:

1. Connection refused:
   - Verify MongoDB service is running
   - Check MongoDB port (default: 27017)
   - Ensure proper authentication

2. Slow queries:
   - Review index usage with explain()
   - Check for missing indexes
   - Optimize query patterns

3. Data consistency issues:
   - Verify schema validation
   - Check for duplicate records
   - Review update operations

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit changes
4. Push to the branch
5. Create a Pull Request

