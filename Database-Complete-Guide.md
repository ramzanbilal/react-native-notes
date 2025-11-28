# Complete Database Guide for Mobile App Development

**A Comprehensive Guide for React Native Developers**

Master SQL, Database Design, SQLite, Realm, WatermelonDB, Supabase, PostgreSQL & MySQL

---

## Table of Contents

1. [SQL Fundamentals](#sql-fundamentals)
2. [Database Design - Simple to Complex](#database-design)
3. [SQLite in React Native](#sqlite-in-react-native)
4. [Realm Database](#realm-database)
5. [WatermelonDB](#watermelondb)
6. [Supabase with PostgreSQL](#supabase-with-postgresql)
7. [MySQL for Backend](#mysql-for-backend)
8. [Real-World Chat App Example](#chat-app-example)
9. [Best Practices](#best-practices)

---

## SQL Fundamentals

### What is SQL?

SQL (Structured Query Language) is the standard language for managing relational databases. It allows you to:
- Create and modify database structures
- Insert, update, and delete data
- Query and retrieve data
- Control access and permissions

### Essential SQL Commands

#### 1. DDL (Data Definition Language)

**CREATE TABLE** - Define database structure

```sql
-- Basic table
CREATE TABLE users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  username TEXT NOT NULL UNIQUE,
  email TEXT NOT NULL UNIQUE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Table with foreign key
CREATE TABLE posts (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id INTEGER NOT NULL,
  title TEXT NOT NULL,
  content TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

**ALTER TABLE** - Modify existing tables

```sql
-- Add column
ALTER TABLE users ADD COLUMN phone TEXT;

-- Rename column (SQLite 3.25.0+)
ALTER TABLE users RENAME COLUMN username TO user_name;

-- Drop column (SQLite 3.35.0+)
ALTER TABLE users DROP COLUMN phone;
```

**DROP TABLE** - Delete table

```sql
DROP TABLE IF EXISTS posts;
```

#### 2. DML (Data Manipulation Language)

**INSERT** - Add data

```sql
-- Single insert
INSERT INTO users (username, email) 
VALUES ('john_doe', 'john@example.com');

-- Multiple inserts
INSERT INTO users (username, email) VALUES
  ('jane_smith', 'jane@example.com'),
  ('bob_jones', 'bob@example.com');

-- Insert with SELECT
INSERT INTO archived_users 
SELECT * FROM users WHERE created_at < '2020-01-01';
```

**SELECT** - Retrieve data

```sql
-- Basic select
SELECT * FROM users;

-- Select specific columns
SELECT id, username, email FROM users;

-- WHERE clause
SELECT * FROM users WHERE id = 1;

-- Multiple conditions
SELECT * FROM users 
WHERE created_at > '2024-01-01' 
  AND email LIKE '%@gmail.com';

-- ORDER BY
SELECT * FROM users ORDER BY created_at DESC;

-- LIMIT and OFFSET (pagination)
SELECT * FROM users LIMIT 10 OFFSET 20;

-- DISTINCT values
SELECT DISTINCT email FROM users;
```

**UPDATE** - Modify data

```sql
-- Update single record
UPDATE users 
SET email = 'newemail@example.com' 
WHERE id = 1;

-- Update multiple columns
UPDATE users 
SET username = 'new_username', 
    email = 'new@example.com'
WHERE id = 1;

-- Update with conditions
UPDATE posts 
SET status = 'archived' 
WHERE created_at < '2023-01-01';
```

**DELETE** - Remove data

```sql
-- Delete specific record
DELETE FROM users WHERE id = 1;

-- Delete with conditions
DELETE FROM posts WHERE user_id = 5;

-- Delete all (be careful!)
DELETE FROM users;
```

#### 3. Advanced SQL Queries

**JOINs** - Combine data from multiple tables

```sql
-- INNER JOIN (only matching records)
SELECT users.username, posts.title
FROM users
INNER JOIN posts ON users.id = posts.user_id;

-- LEFT JOIN (all users, even without posts)
SELECT users.username, posts.title
FROM users
LEFT JOIN posts ON users.id = posts.user_id;

-- Multiple joins
SELECT 
  users.username,
  posts.title,
  comments.content
FROM users
INNER JOIN posts ON users.id = posts.user_id
INNER JOIN comments ON posts.id = comments.post_id;
```

**Aggregate Functions**

```sql
-- COUNT
SELECT COUNT(*) FROM users;
SELECT COUNT(*) as total_users FROM users WHERE created_at > '2024-01-01';

-- SUM
SELECT SUM(price) as total_revenue FROM orders;

-- AVG
SELECT AVG(age) as average_age FROM users;

-- MIN and MAX
SELECT MIN(price), MAX(price) FROM products;

-- GROUP BY
SELECT user_id, COUNT(*) as post_count
FROM posts
GROUP BY user_id;

-- HAVING (filter after GROUP BY)
SELECT user_id, COUNT(*) as post_count
FROM posts
GROUP BY user_id
HAVING COUNT(*) > 5;
```

**Subqueries**

```sql
-- Subquery in WHERE
SELECT * FROM users 
WHERE id IN (SELECT user_id FROM posts WHERE likes > 100);

-- Subquery in SELECT
SELECT 
  username,
  (SELECT COUNT(*) FROM posts WHERE user_id = users.id) as post_count
FROM users;

-- EXISTS
SELECT * FROM users
WHERE EXISTS (
  SELECT 1 FROM posts WHERE posts.user_id = users.id
);
```

**String Operations**

```sql
-- LIKE pattern matching
SELECT * FROM users WHERE email LIKE '%@gmail.com';
SELECT * FROM users WHERE username LIKE 'john%';

-- CONCAT (SQLite uses ||)
SELECT username || ' - ' || email as user_info FROM users;

-- UPPER/LOWER
SELECT UPPER(username) FROM users;
SELECT LOWER(email) FROM users;

-- LENGTH
SELECT username FROM users WHERE LENGTH(username) > 10;

-- TRIM
SELECT TRIM(username) FROM users;
```

**Date/Time Operations**

```sql
-- Current timestamp
SELECT datetime('now');
SELECT date('now');

-- Date comparison
SELECT * FROM posts 
WHERE date(created_at) = date('now');

-- Date arithmetic
SELECT * FROM users 
WHERE created_at > datetime('now', '-7 days');

-- Extract parts
SELECT 
  strftime('%Y', created_at) as year,
  strftime('%m', created_at) as month
FROM posts;
```

**Indexes for Performance**

```sql
-- Create index
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_posts_user_id ON posts(user_id);
CREATE INDEX idx_posts_created ON posts(created_at);

-- Composite index
CREATE INDEX idx_posts_user_created ON posts(user_id, created_at);

-- Unique index
CREATE UNIQUE INDEX idx_users_username ON users(username);

-- Drop index
DROP INDEX IF EXISTS idx_users_email;
```

**Transactions** - Ensure data consistency

```sql
-- Begin transaction
BEGIN TRANSACTION;

INSERT INTO users (username, email) VALUES ('new_user', 'new@example.com');
INSERT INTO profiles (user_id, bio) VALUES (last_insert_rowid(), 'My bio');

-- If everything is okay
COMMIT;

-- If something goes wrong
ROLLBACK;
```

---

## Database Design

### Simple to Complex Database Design

#### Level 1: Single Table (Beginner)

**Use Case:** Simple todo app

```sql
CREATE TABLE todos (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  title TEXT NOT NULL,
  description TEXT,
  completed BOOLEAN DEFAULT 0,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**When to use:**
- No relationships needed
- Simple data structure
- Small apps with single entity

#### Level 2: One-to-Many Relationships (Intermediate)

**Use Case:** Blog with posts and comments

```sql
-- Users table
CREATE TABLE users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  username TEXT NOT NULL UNIQUE,
  email TEXT NOT NULL UNIQUE,
  password_hash TEXT NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Posts table (one user has many posts)
CREATE TABLE posts (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id INTEGER NOT NULL,
  title TEXT NOT NULL,
  content TEXT,
  status TEXT DEFAULT 'draft', -- draft, published, archived
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Comments table (one post has many comments)
CREATE TABLE comments (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  post_id INTEGER NOT NULL,
  user_id INTEGER NOT NULL,
  content TEXT NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (post_id) REFERENCES posts(id) ON DELETE CASCADE,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Indexes for better performance
CREATE INDEX idx_posts_user_id ON posts(user_id);
CREATE INDEX idx_comments_post_id ON comments(post_id);
CREATE INDEX idx_comments_user_id ON comments(user_id);
```

**Relationships:**
- One user → Many posts
- One post → Many comments
- One user → Many comments

#### Level 3: Many-to-Many Relationships (Advanced)

**Use Case:** E-commerce with products and categories

```sql
-- Users
CREATE TABLE users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  username TEXT NOT NULL UNIQUE,
  email TEXT NOT NULL UNIQUE,
  full_name TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Products
CREATE TABLE products (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  description TEXT,
  price REAL NOT NULL,
  stock INTEGER DEFAULT 0,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Categories
CREATE TABLE categories (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL UNIQUE,
  description TEXT
);

-- Junction table: Products ↔ Categories (Many-to-Many)
CREATE TABLE product_categories (
  product_id INTEGER NOT NULL,
  category_id INTEGER NOT NULL,
  PRIMARY KEY (product_id, category_id),
  FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE,
  FOREIGN KEY (category_id) REFERENCES categories(id) ON DELETE CASCADE
);

-- Orders
CREATE TABLE orders (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id INTEGER NOT NULL,
  total_amount REAL NOT NULL,
  status TEXT DEFAULT 'pending', -- pending, processing, shipped, delivered
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Order Items (connects orders with products)
CREATE TABLE order_items (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  order_id INTEGER NOT NULL,
  product_id INTEGER NOT NULL,
  quantity INTEGER NOT NULL,
  price REAL NOT NULL, -- Price at time of purchase
  FOREIGN KEY (order_id) REFERENCES orders(id) ON DELETE CASCADE,
  FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE RESTRICT
);

-- Indexes
CREATE INDEX idx_orders_user_id ON orders(user_id);
CREATE INDEX idx_order_items_order_id ON order_items(order_id);
CREATE INDEX idx_order_items_product_id ON order_items(product_id);
```

**Relationships:**
- Many products → Many categories (via junction table)
- One user → Many orders
- One order → Many order items
- One product → Many order items

#### Level 4: Complex Schema with Self-Referencing (Expert)

**Use Case:** Social media platform

```sql
-- Users
CREATE TABLE users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  username TEXT NOT NULL UNIQUE,
  email TEXT NOT NULL UNIQUE,
  bio TEXT,
  avatar_url TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Posts
CREATE TABLE posts (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id INTEGER NOT NULL,
  content TEXT NOT NULL,
  image_url TEXT,
  likes_count INTEGER DEFAULT 0,
  comments_count INTEGER DEFAULT 0,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Comments with nested replies (self-referencing)
CREATE TABLE comments (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  post_id INTEGER NOT NULL,
  user_id INTEGER NOT NULL,
  parent_comment_id INTEGER, -- NULL for top-level comments
  content TEXT NOT NULL,
  likes_count INTEGER DEFAULT 0,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (post_id) REFERENCES posts(id) ON DELETE CASCADE,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
  FOREIGN KEY (parent_comment_id) REFERENCES comments(id) ON DELETE CASCADE
);

-- Followers (self-referencing many-to-many)
CREATE TABLE followers (
  follower_id INTEGER NOT NULL, -- User who follows
  following_id INTEGER NOT NULL, -- User being followed
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (follower_id, following_id),
  FOREIGN KEY (follower_id) REFERENCES users(id) ON DELETE CASCADE,
  FOREIGN KEY (following_id) REFERENCES users(id) ON DELETE CASCADE,
  CHECK (follower_id != following_id) -- Can't follow yourself
);

-- Likes (polymorphic - can like posts or comments)
CREATE TABLE likes (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id INTEGER NOT NULL,
  likeable_type TEXT NOT NULL, -- 'post' or 'comment'
  likeable_id INTEGER NOT NULL, -- ID of post or comment
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
  UNIQUE(user_id, likeable_type, likeable_id) -- Can't like same thing twice
);

-- Notifications
CREATE TABLE notifications (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id INTEGER NOT NULL, -- User receiving notification
  type TEXT NOT NULL, -- 'like', 'comment', 'follow'
  actor_id INTEGER NOT NULL, -- User who triggered notification
  reference_type TEXT, -- 'post', 'comment'
  reference_id INTEGER,
  is_read BOOLEAN DEFAULT 0,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
  FOREIGN KEY (actor_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Indexes
CREATE INDEX idx_posts_user_id ON posts(user_id);
CREATE INDEX idx_posts_created ON posts(created_at DESC);
CREATE INDEX idx_comments_post_id ON comments(post_id);
CREATE INDEX idx_comments_user_id ON comments(user_id);
CREATE INDEX idx_comments_parent ON comments(parent_comment_id);
CREATE INDEX idx_followers_follower ON followers(follower_id);
CREATE INDEX idx_followers_following ON followers(following_id);
CREATE INDEX idx_likes_user ON likes(user_id);
CREATE INDEX idx_likes_likeable ON likes(likeable_type, likeable_id);
CREATE INDEX idx_notifications_user ON notifications(user_id);
CREATE INDEX idx_notifications_read ON notifications(user_id, is_read);
```

**Advanced Features:**
- Self-referencing (comments can reply to comments)
- Polymorphic relationships (likes can be on posts or comments)
- Many-to-many with self-reference (followers)
- Computed/denormalized fields (likes_count for performance)
- Constraints to prevent invalid data

### Database Design Principles

#### 1. Normalization

**First Normal Form (1NF):**
- Each column contains atomic values
- No repeating groups

```sql
-- ❌ BAD: Repeating groups
CREATE TABLE users (
  id INTEGER PRIMARY KEY,
  name TEXT,
  phone1 TEXT,
  phone2 TEXT,
  phone3 TEXT
);

-- ✅ GOOD: Separate table
CREATE TABLE users (
  id INTEGER PRIMARY KEY,
  name TEXT
);

CREATE TABLE user_phones (
  id INTEGER PRIMARY KEY,
  user_id INTEGER,
  phone TEXT,
  type TEXT, -- 'home', 'work', 'mobile'
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

**Second Normal Form (2NF):**
- Must be in 1NF
- No partial dependencies

**Third Normal Form (3NF):**
- Must be in 2NF
- No transitive dependencies

```sql
-- ❌ BAD: City and country depend on zip_code
CREATE TABLE addresses (
  id INTEGER PRIMARY KEY,
  street TEXT,
  zip_code TEXT,
  city TEXT,
  country TEXT
);

-- ✅ GOOD: Separate zip code data
CREATE TABLE addresses (
  id INTEGER PRIMARY KEY,
  street TEXT,
  zip_code_id INTEGER,
  FOREIGN KEY (zip_code_id) REFERENCES zip_codes(id)
);

CREATE TABLE zip_codes (
  id INTEGER PRIMARY KEY,
  code TEXT UNIQUE,
  city TEXT,
  country TEXT
);
```

#### 2. Denormalization (When to Break Rules)

Sometimes breaking normalization improves performance:

```sql
-- Denormalized for performance
CREATE TABLE posts (
  id INTEGER PRIMARY KEY,
  user_id INTEGER,
  content TEXT,
  likes_count INTEGER DEFAULT 0, -- Denormalized!
  comments_count INTEGER DEFAULT 0, -- Denormalized!
  created_at TIMESTAMP
);

-- Instead of counting every time:
-- SELECT COUNT(*) FROM likes WHERE post_id = ?
-- We just read: post.likes_count
```

**When to denormalize:**
- Read-heavy operations
- Expensive calculations
- Counts and aggregations
- Performance is critical

**Remember to update counts:**

```sql
-- When someone likes a post
INSERT INTO likes (user_id, post_id) VALUES (?, ?);
UPDATE posts SET likes_count = likes_count + 1 WHERE id = ?;

-- When someone unlikes
DELETE FROM likes WHERE user_id = ? AND post_id = ?;
UPDATE posts SET likes_count = likes_count - 1 WHERE id = ?;
```

---

## SQLite in React Native

### Installation

```bash
# For Expo projects
npx expo install expo-sqlite

# For bare React Native
npm install react-native-sqlite-storage
# OR
npm install react-native-quick-sqlite
```

### Basic Setup (Expo SQLite)

```typescript
// database/db.ts
import * as SQLite from 'expo-sqlite';

// Open database
const db = SQLite.openDatabaseSync('myapp.db');

// Initialize database
export const initDatabase = async () => {
  try {
    await db.execAsync(`
      CREATE TABLE IF NOT EXISTS users (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        username TEXT NOT NULL UNIQUE,
        email TEXT NOT NULL UNIQUE,
        created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
      );

      CREATE TABLE IF NOT EXISTS messages (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        user_id INTEGER NOT NULL,
        content TEXT NOT NULL,
        created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
        FOREIGN KEY (user_id) REFERENCES users(id)
      );

      CREATE INDEX IF NOT EXISTS idx_messages_user_id 
        ON messages(user_id);
      CREATE INDEX IF NOT EXISTS idx_messages_created 
        ON messages(created_at DESC);
    `);
    
    console.log('✅ Database initialized');
  } catch (error) {
    console.error('❌ Database initialization error:', error);
    throw error;
  }
};

export default db;
```

### CRUD Service Layer

```typescript
// services/userService.ts
import db from '../database/db';

export interface User {
  id: number;
  username: string;
  email: string;
  created_at: string;
}

// CREATE
export const createUser = async (
  username: string, 
  email: string
): Promise<User> => {
  try {
    const result = await db.runAsync(
      'INSERT INTO users (username, email) VALUES (?, ?)',
      [username, email]
    );
    
    return {
      id: result.lastInsertRowId,
      username,
      email,
      created_at: new Date().toISOString()
    };
  } catch (error) {
    console.error('Error creating user:', error);
    throw error;
  }
};

// READ - Get all users
export const getAllUsers = async (): Promise<User[]> => {
  try {
    const users = await db.getAllAsync<User>(
      'SELECT * FROM users ORDER BY created_at DESC'
    );
    return users;
  } catch (error) {
    console.error('Error fetching users:', error);
    return [];
  }
};

// READ - Get single user
export const getUserById = async (id: number): Promise<User | null> => {
  try {
    const user = await db.getFirstAsync<User>(
      'SELECT * FROM users WHERE id = ?',
      [id]
    );
    return user || null;
  } catch (error) {
    console.error('Error fetching user:', error);
    return null;
  }
};

// READ - Search users
export const searchUsers = async (query: string): Promise<User[]> => {
  try {
    const users = await db.getAllAsync<User>(
      'SELECT * FROM users WHERE username LIKE ? OR email LIKE ?',
      [`%${query}%`, `%${query}%`]
    );
    return users;
  } catch (error) {
    console.error('Error searching users:', error);
    return [];
  }
};

// UPDATE
export const updateUser = async (
  id: number, 
  updates: Partial<User>
): Promise<boolean> => {
  try {
    const fields: string[] = [];
    const values: any[] = [];

    if (updates.username) {
      fields.push('username = ?');
      values.push(updates.username);
    }
    if (updates.email) {
      fields.push('email = ?');
      values.push(updates.email);
    }

    if (fields.length === 0) return false;

    values.push(id);
    
    await db.runAsync(
      `UPDATE users SET ${fields.join(', ')} WHERE id = ?`,
      values
    );
    
    return true;
  } catch (error) {
    console.error('Error updating user:', error);
    return false;
  }
};

// DELETE
export const deleteUser = async (id: number): Promise<boolean> => {
  try {
    await db.runAsync('DELETE FROM users WHERE id = ?', [id]);
    return true;
  } catch (error) {
    console.error('Error deleting user:', error);
    return false;
  }
};
```

### React Component Example

```typescript
// screens/UsersScreen.tsx
import React, { useState, useEffect } from 'react';
import { View, FlatList, TextInput, Button, Text, StyleSheet } from 'react-native';
import { getAllUsers, createUser, deleteUser, User } from '../services/userService';

const UsersScreen = () => {
  const [users, setUsers] = useState<User[]>([]);
  const [username, setUsername] = useState('');
  const [email, setEmail] = useState('');
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    loadUsers();
  }, []);

  const loadUsers = async () => {
    setLoading(true);
    const data = await getAllUsers();
    setUsers(data);
    setLoading(false);
  };

  const handleAddUser = async () => {
    if (!username || !email) return;
    
    try {
      await createUser(username, email);
      setUsername('');
      setEmail('');
      loadUsers();
    } catch (error) {
      alert('Error creating user');
    }
  };

  const handleDeleteUser = async (id: number) => {
    await deleteUser(id);
    loadUsers();
  };

  return (
    <View style={styles.container}>
      <View style={styles.form}>
        <TextInput
          style={styles.input}
          placeholder="Username"
          value={username}
          onChangeText={setUsername}
        />
        <TextInput
          style={styles.input}
          placeholder="Email"
          value={email}
          onChangeText={setEmail}
          keyboardType="email-address"
        />
        <Button title="Add User" onPress={handleAddUser} />
      </View>

      <FlatList
        data={users}
        keyExtractor={(item) => item.id.toString()}
        renderItem={({ item }) => (
          <View style={styles.userCard}>
            <View style={styles.userInfo}>
              <Text style={styles.username}>{item.username}</Text>
              <Text style={styles.email}>{item.email}</Text>
            </View>
            <Button 
              title="Delete" 
              color="red"
              onPress={() => handleDeleteUser(item.id)} 
            />
          </View>
        )}
        refreshing={loading}
        onRefresh={loadUsers}
      />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 16,
  },
  form: {
    marginBottom: 20,
  },
  input: {
    borderWidth: 1,
    borderColor: '#ddd',
    padding: 10,
    marginBottom: 10,
    borderRadius: 5,
  },
  userCard: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    padding: 12,
    backgroundColor: 'white',
    marginBottom: 8,
    borderRadius: 8,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 1 },
    shadowOpacity: 0.1,
    shadowRadius: 2,
    elevation: 2,
  },
  userInfo: {
    flex: 1,
  },
  username: {
    fontSize: 16,
    fontWeight: 'bold',
  },
  email: {
    fontSize: 14,
    color: '#666',
  },
});

export default UsersScreen;
```

---

## Realm Database

### Why Realm?

- **Object-Oriented**: Work with objects, not SQL
- **Real-time Updates**: Automatic UI refresh
- **Fast**: Optimized for mobile
- **Sync**: Built-in cloud synchronization
- **Cross-Platform**: Works on iOS, Android, Web

### Installation

```bash
npm install realm @realm/react
```

### Schema Definition

```typescript
// models/User.ts
import Realm from 'realm';

export class User extends Realm.Object<User> {
  _id!: Realm.BSON.ObjectId;
  username!: string;
  email!: string;
  avatar?: string;
  createdAt!: Date;

  static schema: Realm.ObjectSchema = {
    name: 'User',
    primaryKey: '_id',
    properties: {
      _id: 'objectId',
      username: 'string',
      email: 'string',
      avatar: 'string?',
      createdAt: 'date'
    }
  };
}

// models/Message.ts
export class Message extends Realm.Object<Message> {
  _id!: Realm.BSON.ObjectId;
  senderId!: Realm.BSON.ObjectId;
  receiverId!: Realm.BSON.ObjectId;
  content!: string;
  isRead!: boolean;
  createdAt!: Date;

  static schema: Realm.ObjectSchema = {
    name: 'Message',
    primaryKey: '_id',
    properties: {
      _id: 'objectId',
      senderId: 'objectId',
      receiverId: 'objectId',
      content: 'string',
      isRead: { type: 'bool', default: false },
      createdAt: 'date'
    }
  };
}

// models/Conversation.ts
export class Conversation extends Realm.Object<Conversation> {
  _id!: Realm.BSON.ObjectId;
  participants!: Realm.List<Realm.BSON.ObjectId>;
  lastMessage?: string;
  lastMessageAt?: Date;
  unreadCount!: number;

  static schema: Realm.ObjectSchema = {
    name: 'Conversation',
    primaryKey: '_id',
    properties: {
      _id: 'objectId',
      participants: 'objectId[]',
      lastMessage: 'string?',
      lastMessageAt: 'date?',
      unreadCount: { type: 'int', default: 0 }
    }
  };
}
```

### Realm Configuration

```typescript
// realm/config.ts
import { createRealmContext } from '@realm/react';
import { User, Message, Conversation } from '../models';

export const RealmContext = createRealmContext({
  schema: [User, Message, Conversation],
  schemaVersion: 1,
  // Optional: Add migration if schema changes
  onMigration: (oldRealm, newRealm) => {
    // Handle schema migrations
  }
});
```

### CRUD Operations with Realm

```typescript
// services/realmChatService.ts
import Realm from 'realm';
import { RealmContext } from '../realm/config';
import { User, Message, Conversation } from '../models';

const { useRealm, useQuery, useObject } = RealmContext;

// In a component or service
export const useChatService = () => {
  const realm = useRealm();

  // CREATE User
  const createUser = (username: string, email: string) => {
    realm.write(() => {
      realm.create('User', {
        _id: new Realm.BSON.ObjectId(),
        username,
        email,
        createdAt: new Date()
      });
    });
  };

  // CREATE Message
  const sendMessage = (
    senderId: Realm.BSON.ObjectId,
    receiverId: Realm.BSON.ObjectId,
    content: string
  ) => {
    realm.write(() => {
      // Create message
      const message = realm.create('Message', {
        _id: new Realm.BSON.ObjectId(),
        senderId,
        receiverId,
        content,
        isRead: false,
        createdAt: new Date()
      });

      // Update or create conversation
      const conversationId = [senderId.toString(), receiverId.toString()]
        .sort()
        .join('_');
      
      let conversation = realm.objectForPrimaryKey(
        'Conversation',
        new Realm.BSON.ObjectId(conversationId)
      );

      if (conversation) {
        conversation.lastMessage = content;
        conversation.lastMessageAt = new Date();
        conversation.unreadCount += 1;
      } else {
        realm.create('Conversation', {
          _id: new Realm.BSON.ObjectId(conversationId),
          participants: [senderId, receiverId],
          lastMessage: content,
          lastMessageAt: new Date(),
          unreadCount: 1
        });
      }
    });
  };

  // READ Messages (returns observable query)
  const getMessages = (userId1: string, userId2: string) => {
    return realm
      .objects('Message')
      .filtered(
        '(senderId == $0 AND receiverId == $1) OR (senderId == $1 AND receiverId == $0)',
        userId1,
        userId2
      )
      .sorted('createdAt', false);
  };

  // UPDATE - Mark as read
  const markAsRead = (messageId: Realm.BSON.ObjectId) => {
    realm.write(() => {
      const message = realm.objectForPrimaryKey('Message', messageId);
      if (message) {
        message.isRead = true;
      }
    });
  };

  // DELETE Message
  const deleteMessage = (messageId: Realm.BSON.ObjectId) => {
    realm.write(() => {
      const message = realm.objectForPrimaryKey('Message', messageId);
      if (message) {
        realm.delete(message);
      }
    });
  };

  return {
    createUser,
    sendMessage,
    getMessages,
    markAsRead,
    deleteMessage
  };
};
```

### React Component with Realm

```typescript
// screens/ChatScreen.tsx
import React, { useState } from 'react';
import { View, FlatList, TextInput, Button, Text, StyleSheet } from 'react-native';
import { RealmContext } from '../realm/config';
import { useChatService } from '../services/realmChatService';
import Realm from 'realm';

const { useQuery } = RealmContext;

interface ChatScreenProps {
  currentUserId: string;
  otherUserId: string;
}

const ChatScreen: React.FC<ChatScreenProps> = ({ currentUserId, otherUserId }) => {
  const [messageText, setMessageText] = useState('');
  const { sendMessage } = useChatService();

  // Reactive query - automatically updates UI when data changes!
  const messages = useQuery('Message', (collection) => {
    return collection
      .filtered(
        '(senderId == $0 AND receiverId == $1) OR (senderId == $1 AND receiverId == $0)',
        currentUserId,
        otherUserId
      )
      .sorted('createdAt', true);
  });

  const handleSend = () => {
    if (!messageText.trim()) return;

    sendMessage(
      new Realm.BSON.ObjectId(currentUserId),
      new Realm.BSON.ObjectId(otherUserId),
      messageText
    );
    
    setMessageText('');
  };

  return (
    <View style={styles.container}>
      <FlatList
        data={messages}
        inverted
        keyExtractor={(item) => item._id.toString()}
        renderItem={({ item }) => (
          <View
            style={[
              styles.messageBubble,
              item.senderId.toString() === currentUserId
                ? styles.sentMessage
                : styles.receivedMessage
            ]}
          >
            <Text style={styles.messageText}>{item.content}</Text>
            <Text style={styles.timestamp}>
              {item.createdAt.toLocaleTimeString()}
            </Text>
          </View>
        )}
      />

      <View style={styles.inputContainer}>
        <TextInput
          style={styles.input}
          value={messageText}
          onChangeText={setMessageText}
          placeholder="Type a message..."
          multiline
        />
        <Button title="Send" onPress={handleSend} />
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f5f5f5',
  },
  messageBubble: {
    maxWidth: '70%',
    padding: 12,
    borderRadius: 16,
    marginVertical: 4,
    marginHorizontal: 12,
  },
  sentMessage: {
    alignSelf: 'flex-end',
    backgroundColor: '#007AFF',
  },
  receivedMessage: {
    alignSelf: 'flex-start',
    backgroundColor: 'white',
  },
  messageText: {
    fontSize: 16,
  },
  timestamp: {
    fontSize: 12,
    marginTop: 4,
    opacity: 0.6,
  },
  inputContainer: {
    flexDirection: 'row',
    padding: 12,
    backgroundColor: 'white',
    alignItems: 'center',
  },
  input: {
    flex: 1,
    borderWidth: 1,
    borderColor: '#ddd',
    borderRadius: 20,
    paddingHorizontal: 16,
    paddingVertical: 8,
    marginRight: 8,
  },
});

export default ChatScreen;
```

### App Setup with Realm

```typescript
// App.tsx
import React from 'react';
import { RealmContext } from './realm/config';
import ChatScreen from './screens/ChatScreen';

const { RealmProvider } = RealmContext;

function App() {
  return (
    <RealmProvider>
      <ChatScreen 
        currentUserId="user1_id" 
        otherUserId="user2_id" 
      />
    </RealmProvider>
  );
}

export default App;
```

---

## WatermelonDB

### Why WatermelonDB?

- **Optimized for React Native**: Built specifically for mobile
- **Lazy Loading**: Only loads data when needed
- **Observable**: Automatic UI updates
- **SQLite Under the Hood**: Familiar SQL, better performance
- **Sync Support**: Built-in synchronization primitives
- **Handles Large Datasets**: Thousands of records smoothly

### Installation

```bash
npm install @nozbe/watermelondb @nozbe/with-observables

# For Expo
npx expo install @nozbe/watermelondb
```

### Schema Definition

```typescript
// model/schema.ts
import { appSchema, tableSchema } from '@nozbe/watermelondb';

export default appSchema({
  version: 1,
  tables: [
    tableSchema({
      name: 'users',
      columns: [
        { name: 'username', type: 'string' },
        { name: 'email', type: 'string', isIndexed: true },
        { name: 'avatar_url', type: 'string', isOptional: true },
        { name: 'bio', type: 'string', isOptional: true },
        { name: 'created_at', type: 'number' },
        { name: 'updated_at', type: 'number' }
      ]
    }),
    tableSchema({
      name: 'conversations',
      columns: [
        { name: 'name', type: 'string', isOptional: true },
        { name: 'is_group', type: 'boolean' },
        { name: 'last_message', type: 'string', isOptional: true },
        { name: 'last_message_at', type: 'number', isOptional: true },
        { name: 'created_at', type: 'number' },
        { name: 'updated_at', type: 'number' }
      ]
    }),
    tableSchema({
      name: 'messages',
      columns: [
        { name: 'conversation_id', type: 'string', isIndexed: true },
        { name: 'sender_id', type: 'string', isIndexed: true },
        { name: 'content', type: 'string' },
        { name: 'type', type: 'string' }, // 'text', 'image', 'file'
        { name: 'is_read', type: 'boolean' },
        { name: 'created_at', type: 'number' },
        { name: 'updated_at', type: 'number' }
      ]
    }),
    tableSchema({
      name: 'conversation_participants',
      columns: [
        { name: 'conversation_id', type: 'string', isIndexed: true },
        { name: 'user_id', type: 'string', isIndexed: true },
        { name: 'joined_at', type: 'number' },
        { name: 'created_at', type: 'number' },
        { name: 'updated_at', type: 'number' }
      ]
    })
  ]
});
```

### Model Definitions

```typescript
// models/User.ts
import { Model } from '@nozbe/watermelondb';
import { field, date, readonly } from '@nozbe/watermelondb/decorators';

export default class User extends Model {
  static table = 'users';

  @field('username') username!: string;
  @field('email') email!: string;
  @field('avatar_url') avatarUrl?: string;
  @field('bio') bio?: string;
  
  @readonly @date('created_at') createdAt!: Date;
  @readonly @date('updated_at') updatedAt!: Date;
}

// models/Conversation.ts
import { Model, Query } from '@nozbe/watermelondb';
import { field, date, readonly, children } from '@nozbe/watermelondb/decorators';
import Message from './Message';
import ConversationParticipant from './ConversationParticipant';

export default class Conversation extends Model {
  static table = 'conversations';
  
  static associations = {
    messages: { type: 'has_many', foreignKey: 'conversation_id' },
    conversation_participants: { type: 'has_many', foreignKey: 'conversation_id' }
  } as const;

  @field('name') name?: string;
  @field('is_group') isGroup!: boolean;
  @field('last_message') lastMessage?: string;
  @date('last_message_at') lastMessageAt?: Date;
  
  @readonly @date('created_at') createdAt!: Date;
  @readonly @date('updated_at') updatedAt!: Date;

  @children('messages') messages!: Query<Message>;
  @children('conversation_participants') participants!: Query<ConversationParticipant>;
}

// models/Message.ts
import { Model, Relation } from '@nozbe/watermelondb';
import { field, date, readonly, relation, immutableRelation } from '@nozbe/watermelondb/decorators';
import Conversation from './Conversation';
import User from './User';

export default class Message extends Model {
  static table = 'messages';
  
  static associations = {
    conversations: { type: 'belongs_to', key: 'conversation_id' },
    users: { type: 'belongs_to', key: 'sender_id' }
  } as const;

  @field('conversation_id') conversationId!: string;
  @field('sender_id') senderId!: string;
  @field('content') content!: string;
  @field('type') type!: string;
  @field('is_read') isRead!: boolean;
  
  @readonly @date('created_at') createdAt!: Date;
  @readonly @date('updated_at') updatedAt!: Date;

  @immutableRelation('conversations', 'conversation_id') conversation!: Relation<Conversation>;
  @relation('users', 'sender_id') sender!: Relation<User>;
}

// models/ConversationParticipant.ts
import { Model, Relation } from '@nozbe/watermelondb';
import { field, date, readonly, immutableRelation } from '@nozbe/watermelondb/decorators';
import Conversation from './Conversation';
import User from './User';

export default class ConversationParticipant extends Model {
  static table = 'conversation_participants';
  
  static associations = {
    conversations: { type: 'belongs_to', key: 'conversation_id' },
    users: { type: 'belongs_to', key: 'user_id' }
  } as const;

  @field('conversation_id') conversationId!: string;
  @field('user_id') userId!: string;
  @date('joined_at') joinedAt!: Date;
  
  @readonly @date('created_at') createdAt!: Date;
  @readonly @date('updated_at') updatedAt!: Date;

  @immutableRelation('conversations', 'conversation_id') conversation!: Relation<Conversation>;
  @immutableRelation('users', 'user_id') user!: Relation<User>;
}
```

### Database Setup

```typescript
// database/index.ts
import { Database } from '@nozbe/watermelondb';
import SQLiteAdapter from '@nozbe/watermelondb/adapters/sqlite';
import schema from './schema';
import migrations from './migrations';

// Import all models
import User from './models/User';
import Conversation from './models/Conversation';
import Message from './models/Message';
import ConversationParticipant from './models/ConversationParticipant';

const adapter = new SQLiteAdapter({
  schema,
  migrations,
  dbName: 'ChatApp',
  jsi: true, // Use JSI for better performance (optional)
});

export const database = new Database({
  adapter,
  modelClasses: [
    User,
    Conversation,
    Message,
    ConversationParticipant
  ],
});
```

### CRUD Operations

```typescript
// services/chatService.ts
import { database } from '../database';
import { Q } from '@nozbe/watermelondb';

export const chatService = {
  // CREATE User
  createUser: async (username: string, email: string) => {
    await database.write(async () => {
      const user = await database.collections.get('users').create((user: any) => {
        user.username = username;
        user.email = email;
      });
      return user;
    });
  },

  // CREATE Conversation
  createConversation: async (participantIds: string[], isGroup: boolean = false) => {
    await database.write(async () => {
      const conversation = await database.collections
        .get('conversations')
        .create((conv: any) => {
          conv.isGroup = isGroup;
        });

      // Add participants
      for (const userId of participantIds) {
        await database.collections
          .get('conversation_participants')
          .create((participant: any) => {
            participant.conversationId = conversation.id;
            participant.userId = userId;
            participant.joinedAt = new Date();
          });
      }

      return conversation;
    });
  },

  // CREATE Message
  sendMessage: async (
    conversationId: string,
    senderId: string,
    content: string,
    type: string = 'text'
  ) => {
    await database.write(async () => {
      // Create message
      const message = await database.collections
        .get('messages')
        .create((msg: any) => {
          msg.conversationId = conversationId;
          msg.senderId = senderId;
          msg.content = content;
          msg.type = type;
          msg.isRead = false;
        });

      // Update conversation
      const conversation = await database.collections
        .get('conversations')
        .find(conversationId);

      await conversation.update((conv: any) => {
        conv.lastMessage = content;
        conv.lastMessageAt = new Date();
      });

      return message;
    });
  },

  // READ - Get user conversations
  getUserConversations: (userId: string) => {
    return database.collections
      .get('conversation_participants')
      .query(Q.where('user_id', userId))
      .observe();
  },

  // READ - Get messages in conversation
  getConversationMessages: (conversationId: string) => {
    return database.collections
      .get('messages')
      .query(
        Q.where('conversation_id', conversationId),
        Q.sortBy('created_at', Q.desc)
      )
      .observe();
  },

  // UPDATE - Mark message as read
  markMessageAsRead: async (messageId: string) => {
    await database.write(async () => {
      const message = await database.collections
        .get('messages')
        .find(messageId);

      await message.update((msg: any) => {
        msg.isRead = true;
      });
    });
  },

  // DELETE - Delete message
  deleteMessage: async (messageId: string) => {
    await database.write(async () => {
      const message = await database.collections
        .get('messages')
        .find(messageId);

      await message.destroyPermanently();
    });
  },

  // Complex query - Get unread count
  getUnreadCount: async (conversationId: string, userId: string) => {
    const unreadMessages = await database.collections
      .get('messages')
      .query(
        Q.where('conversation_id', conversationId),
        Q.where('sender_id', Q.notEq(userId)),
        Q.where('is_read', false)
      )
      .fetchCount();

    return unreadMessages;
  }
};
```

### React Components with WatermelonDB

```typescript
// screens/ConversationsScreen.tsx
import React from 'react';
import { View, FlatList, Text, TouchableOpacity, StyleSheet } from 'react-native';
import { database } from '../database';
import withObservables from '@nozbe/with-observables';
import { Q } from '@nozbe/watermelondb';

interface ConversationsListProps {
  conversations: any[];
  onConversationPress: (id: string) => void;
}

const ConversationsList: React.FC<ConversationsListProps> = ({ 
  conversations,
  onConversationPress 
}) => {
  return (
    <FlatList
      data={conversations}
      keyExtractor={(item) => item.id}
      renderItem={({ item }) => (
        <TouchableOpacity
          style={styles.conversationCard}
          onPress={() => onConversationPress(item.id)}
        >
          <View style={styles.conversationInfo}>
            <Text style={styles.conversationName}>
              {item.name || 'Conversation'}
            </Text>
            <Text style={styles.lastMessage} numberOfLines={1}>
              {item.lastMessage || 'No messages yet'}
            </Text>
          </View>
          <Text style={styles.timestamp}>
            {item.lastMessageAt?.toLocaleTimeString() || ''}
          </Text>
        </TouchableOpacity>
      )}
    />
  );
};

// Enhance component with observables
const enhance = withObservables(['userId'], ({ userId }: { userId: string }) => ({
  conversations: database.collections
    .get('conversation_participants')
    .query(Q.where('user_id', userId))
    .observeWithColumns(['conversation_id'])
}));

export default enhance(ConversationsList);

const styles = StyleSheet.create({
  conversationCard: {
    flexDirection: 'row',
    padding: 16,
    borderBottomWidth: 1,
    borderBottomColor: '#eee',
  },
  conversationInfo: {
    flex: 1,
  },
  conversationName: {
    fontSize: 16,
    fontWeight: 'bold',
    marginBottom: 4,
  },
  lastMessage: {
    fontSize: 14,
    color: '#666',
  },
  timestamp: {
    fontSize: 12,
    color: '#999',
  },
});
```

```typescript
// screens/ChatScreen.tsx
import React, { useState } from 'react';
import { View, FlatList, TextInput, Button, Text, StyleSheet } from 'react-native';
import withObservables from '@nozbe/with-observables';
import { Q } from '@nozbe/watermelondb';
import { database } from '../database';
import { chatService } from '../services/chatService';

interface ChatScreenProps {
  conversationId: string;
  currentUserId: string;
  messages: any[];
}

const ChatScreen: React.FC<ChatScreenProps> = ({
  conversationId,
  currentUserId,
  messages
}) => {
  const [messageText, setMessageText] = useState('');

  const handleSend = async () => {
    if (!messageText.trim()) return;

    await chatService.sendMessage(
      conversationId,
      currentUserId,
      messageText
    );

    setMessageText('');
  };

  return (
    <View style={styles.container}>
      <FlatList
        data={messages}
        inverted
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => (
          <View
            style={[
              styles.messageBubble,
              item.senderId === currentUserId
                ? styles.sentMessage
                : styles.receivedMessage
            ]}
          >
            <Text style={styles.messageText}>{item.content}</Text>
            <Text style={styles.timestamp}>
              {new Date(item.createdAt).toLocaleTimeString()}
            </Text>
          </View>
        )}
      />

      <View style={styles.inputContainer}>
        <TextInput
          style={styles.input}
          value={messageText}
          onChangeText={setMessageText}
          placeholder="Type a message..."
          multiline
        />
        <Button title="Send" onPress={handleSend} />
      </View>
    </View>
  );
};

// Enhance with observables - auto-updates when messages change
const enhance = withObservables(
  ['conversationId'],
  ({ conversationId }: { conversationId: string }) => ({
    messages: database.collections
      .get('messages')
      .query(
        Q.where('conversation_id', conversationId),
        Q.sortBy('created_at', Q.desc)
      )
      .observe()
  })
);

export default enhance(ChatScreen);

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f5f5f5',
  },
  messageBubble: {
    maxWidth: '70%',
    padding: 12,
    borderRadius: 16,
    marginVertical: 4,
    marginHorizontal: 12,
  },
  sentMessage: {
    alignSelf: 'flex-end',
    backgroundColor: '#007AFF',
  },
  receivedMessage: {
    alignSelf: 'flex-start',
    backgroundColor: 'white',
  },
  messageText: {
    fontSize: 16,
  },
  timestamp: {
    fontSize: 12,
    marginTop: 4,
    opacity: 0.6,
  },
  inputContainer: {
    flexDirection: 'row',
    padding: 12,
    backgroundColor: 'white',
    alignItems: 'center',
  },
  input: {
    flex: 1,
    borderWidth: 1,
    borderColor: '#ddd',
    borderRadius: 20,
    paddingHorizontal: 16,
    paddingVertical: 8,
    marginRight: 8,
  },
});
```

---

## Supabase with PostgreSQL

### Why Supabase?

- **PostgreSQL Database**: Full power of Postgres
- **Real-time Subscriptions**: Live updates
- **Auto-generated APIs**: RESTful and GraphQL
- **Authentication**: Built-in user management
- **Storage**: File uploads and management
- **Row Level Security**: Built-in authorization

### Installation

```bash
npm install @supabase/supabase-js
```

### Initialize Supabase

```typescript
// lib/supabase.ts
import { createClient } from '@supabase/supabase-js';

const supabaseUrl = 'https://your-project.supabase.co';
const supabaseAnonKey = 'your-anon-key';

export const supabase = createClient(supabaseUrl, supabaseAnonKey);

// TypeScript types (optional but recommended)
export type Json =
  | string
  | number
  | boolean
  | null
  | { [key: string]: Json | undefined }
  | Json[]

export interface Database {
  public: {
    Tables: {
      users: {
        Row: {
          id: string
          username: string
          email: string
          avatar_url: string | null
          created_at: string
        }
        Insert: {
          id?: string
          username: string
          email: string
          avatar_url?: string | null
          created_at?: string
        }
        Update: {
          id?: string
          username?: string
          email?: string
          avatar_url?: string | null
          created_at?: string
        }
      }
      // Add other tables...
    }
  }
}
```

### PostgreSQL Schema for Chat App

```sql
-- Enable UUID extension
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- Users table
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  username TEXT NOT NULL UNIQUE,
  email TEXT NOT NULL UNIQUE,
  avatar_url TEXT,
  bio TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Conversations table
CREATE TABLE conversations (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name TEXT,
  is_group BOOLEAN DEFAULT FALSE,
  created_by UUID REFERENCES users(id),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Conversation participants (many-to-many)
CREATE TABLE conversation_participants (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  conversation_id UUID REFERENCES conversations(id) ON DELETE CASCADE,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  joined_at TIMESTAMPTZ DEFAULT NOW(),
  last_read_at TIMESTAMPTZ,
  UNIQUE(conversation_id, user_id)
);

-- Messages table
CREATE TABLE messages (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  conversation_id UUID REFERENCES conversations(id) ON DELETE CASCADE,
  sender_id UUID REFERENCES users(id) ON DELETE CASCADE,
  content TEXT NOT NULL,
  type TEXT DEFAULT 'text' CHECK (type IN ('text', 'image', 'file', 'voice')),
  metadata JSONB, -- For file URLs, image dimensions, etc.
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Message reactions
CREATE TABLE message_reactions (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  message_id UUID REFERENCES messages(id) ON DELETE CASCADE,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  emoji TEXT NOT NULL,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(message_id, user_id, emoji)
);

-- Indexes for performance
CREATE INDEX idx_messages_conversation ON messages(conversation_id, created_at DESC);
CREATE INDEX idx_messages_sender ON messages(sender_id);
CREATE INDEX idx_participants_conversation ON conversation_participants(conversation_id);
CREATE INDEX idx_participants_user ON conversation_participants(user_id);
CREATE INDEX idx_reactions_message ON message_reactions(message_id);

-- Function to update updated_at timestamp
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Triggers for updated_at
CREATE TRIGGER update_users_updated_at
  BEFORE UPDATE ON users
  FOR EACH ROW
  EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_conversations_updated_at
  BEFORE UPDATE ON conversations
  FOR EACH ROW
  EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_messages_updated_at
  BEFORE UPDATE ON messages
  FOR EACH ROW
  EXECUTE FUNCTION update_updated_at_column();

-- Row Level Security (RLS)
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
ALTER TABLE conversations ENABLE ROW LEVEL SECURITY;
ALTER TABLE conversation_participants ENABLE ROW LEVEL SECURITY;
ALTER TABLE messages ENABLE ROW LEVEL SECURITY;
ALTER TABLE message_reactions ENABLE ROW LEVEL SECURITY;

-- RLS Policies for users
CREATE POLICY "Users can view all users"
  ON users FOR SELECT
  USING (true);

CREATE POLICY "Users can update their own profile"
  ON users FOR UPDATE
  USING (auth.uid() = id);

-- RLS Policies for conversations
CREATE POLICY "Users can view conversations they're part of"
  ON conversations FOR SELECT
  USING (
    EXISTS (
      SELECT 1 FROM conversation_participants
      WHERE conversation_id = id
      AND user_id = auth.uid()
    )
  );

CREATE POLICY "Users can create conversations"
  ON conversations FOR INSERT
  WITH CHECK (auth.uid() = created_by);

-- RLS Policies for messages
CREATE POLICY "Users can view messages in their conversations"
  ON messages FOR SELECT
  USING (
    EXISTS (
      SELECT 1 FROM conversation_participants
      WHERE conversation_id = messages.conversation_id
      AND user_id = auth.uid()
    )
  );

CREATE POLICY "Users can send messages to their conversations"
  ON messages FOR INSERT
  WITH CHECK (
    auth.uid() = sender_id
    AND EXISTS (
      SELECT 1 FROM conversation_participants
      WHERE conversation_id = messages.conversation_id
      AND user_id = auth.uid()
    )
  );

-- View for conversation list with last message
CREATE VIEW conversation_list AS
SELECT 
  c.id,
  c.name,
  c.is_group,
  c.created_at,
  (
    SELECT json_agg(
      json_build_object(
        'id', u.id,
        'username', u.username,
        'avatar_url', u.avatar_url
      )
    )
    FROM conversation_participants cp
    JOIN users u ON u.id = cp.user_id
    WHERE cp.conversation_id = c.id
  ) as participants,
  (
    SELECT json_build_object(
      'id', m.id,
      'content', m.content,
      'sender_id', m.sender_id,
      'created_at', m.created_at
    )
    FROM messages m
    WHERE m.conversation_id = c.id
    ORDER BY m.created_at DESC
    LIMIT 1
  ) as last_message
FROM conversations c;
```

### CRUD Operations with Supabase

```typescript
// services/supabaseChatService.ts
import { supabase } from '../lib/supabase';

export const supabaseChatService = {
  // CREATE User
  createUser: async (username: string, email: string) => {
    const { data, error } = await supabase
      .from('users')
      .insert([{ username, email }])
      .select()
      .single();

    if (error) throw error;
    return data;
  },

  // CREATE Conversation
  createConversation: async (
    participantIds: string[],
    name?: string,
    isGroup: boolean = false
  ) => {
    // Create conversation
    const { data: conversation, error: convError } = await supabase
      .from('conversations')
      .insert([
        {
          name,
          is_group: isGroup,
          created_by: participantIds[0]
        }
      ])
      .select()
      .single();

    if (convError) throw convError;

    // Add participants
    const participants = participantIds.map(userId => ({
      conversation_id: conversation.id,
      user_id: userId
    }));

    const { error: partError } = await supabase
      .from('conversation_participants')
      .insert(participants);

    if (partError) throw partError;

    return conversation;
  },

  // CREATE Message
  sendMessage: async (
    conversationId: string,
    senderId: string,
    content: string,
    type: string = 'text',
    metadata?: any
  ) => {
    const { data, error } = await supabase
      .from('messages')
      .insert([
        {
          conversation_id: conversationId,
          sender_id: senderId,
          content,
          type,
          metadata
        }
      ])
      .select()
      .single();

    if (error) throw error;
    return data;
  },

  // READ - Get user's conversations
  getUserConversations: async (userId: string) => {
    const { data, error } = await supabase
      .from('conversation_participants')
      .select(`
        conversation_id,
        conversations (
          id,
          name,
          is_group,
          created_at,
          conversation_participants (
            user_id,
            users (
              id,
              username,
              avatar_url
            )
          )
        )
      `)
      .eq('user_id', userId)
      .order('joined_at', { ascending: false });

    if (error) throw error;
    return data;
  },

  // READ - Get messages in conversation
  getConversationMessages: async (
    conversationId: string,
    limit: number = 50,
    offset: number = 0
  ) => {
    const { data, error } = await supabase
      .from('messages')
      .select(`
        id,
        content,
        type,
        metadata,
        created_at,
        sender:users (
          id,
          username,
          avatar_url
        )
      `)
      .eq('conversation_id', conversationId)
      .order('created_at', { ascending: false })
      .range(offset, offset + limit - 1);

    if (error) throw error;
    return data;
  },

  // READ - Get single conversation
  getConversation: async (conversationId: string) => {
    const { data, error } = await supabase
      .from('conversations')
      .select(`
        id,
        name,
        is_group,
        created_at,
        conversation_participants (
          user_id,
          joined_at,
          users (
            id,
            username,
            avatar_url
          )
        )
      `)
      .eq('id', conversationId)
      .single();

    if (error) throw error;
    return data;
  },

  // UPDATE - Mark messages as read
  markMessagesAsRead: async (
    conversationId: string,
    userId: string
  ) => {
    const { error } = await supabase
      .from('conversation_participants')
      .update({ last_read_at: new Date().toISOString() })
      .eq('conversation_id', conversationId)
      .eq('user_id', userId);

    if (error) throw error;
  },

  // UPDATE - Update message
  updateMessage: async (messageId: string, content: string) => {
    const { data, error } = await supabase
      .from('messages')
      .update({ content })
      .eq('id', messageId)
      .select()
      .single();

    if (error) throw error;
    return data;
  },

  // DELETE - Delete message
  deleteMessage: async (messageId: string) => {
    const { error } = await supabase
      .from('messages')
      .delete()
      .eq('id', messageId);

    if (error) throw error;
  },

  // REAL-TIME - Subscribe to new messages
  subscribeToMessages: (
    conversationId: string,
    callback: (message: any) => void
  ) => {
    const channel = supabase
      .channel(`messages:${conversationId}`)
      .on(
        'postgres_changes',
        {
          event: 'INSERT',
          schema: 'public',
          table: 'messages',
          filter: `conversation_id=eq.${conversationId}`
        },
        async (payload) => {
          // Fetch sender info
          const { data: message } = await supabase
            .from('messages')
            .select(`
              id,
              content,
              type,
              created_at,
              sender:users (
                id,
                username,
                avatar_url
              )
            `)
            .eq('id', payload.new.id)
            .single();

          callback(message);
        }
      )
      .subscribe();

    return () => {
      supabase.removeChannel(channel);
    };
  },

  // REAL-TIME - Subscribe to user's conversations
  subscribeToConversations: (
    userId: string,
    callback: () => void
  ) => {
    const channel = supabase
      .channel(`user-conversations:${userId}`)
      .on(
        'postgres_changes',
        {
          event: '*',
          schema: 'public',
          table: 'conversation_participants',
          filter: `user_id=eq.${userId}`
        },
        callback
      )
      .subscribe();

    return () => {
      supabase.removeChannel(channel);
    };
  }
};
```

### React Components with Supabase

```typescript
// screens/ChatScreen.tsx
import React, { useState, useEffect } from 'react';
import { View, FlatList, TextInput, Button, Text, StyleSheet } from 'react-native';
import { supabaseChatService } from '../services/supabaseChatService';

interface Message {
  id: string;
  content: string;
  type: string;
  created_at: string;
  sender: {
    id: string;
    username: string;
    avatar_url: string | null;
  };
}

interface ChatScreenProps {
  conversationId: string;
  currentUserId: string;
}

const ChatScreen: React.FC<ChatScreenProps> = ({
  conversationId,
  currentUserId
}) => {
  const [messages, setMessages] = useState<Message[]>([]);
  const [messageText, setMessageText] = useState('');
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    loadMessages();

    // Subscribe to new messages
    const unsubscribe = supabaseChatService.subscribeToMessages(
      conversationId,
      (newMessage) => {
        setMessages(prev => [newMessage, ...prev]);
      }
    );

    return unsubscribe;
  }, [conversationId]);

  const loadMessages = async () => {
    setLoading(true);
    try {
      const data = await supabaseChatService.getConversationMessages(
        conversationId
      );
      setMessages(data);
    } catch (error) {
      console.error('Error loading messages:', error);
    }
    setLoading(false);
  };

  const handleSend = async () => {
    if (!messageText.trim()) return;

    try {
      await supabaseChatService.sendMessage(
        conversationId,
        currentUserId,
        messageText
      );
      setMessageText('');
    } catch (error) {
      console.error('Error sending message:', error);
      alert('Failed to send message');
    }
  };

  return (
    <View style={styles.container}>
      <FlatList
        data={messages}
        inverted
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => (
          <View
            style={[
              styles.messageBubble,
              item.sender.id === currentUserId
                ? styles.sentMessage
                : styles.receivedMessage
            ]}
          >
            {item.sender.id !== currentUserId && (
              <Text style={styles.senderName}>{item.sender.username}</Text>
            )}
            <Text style={styles.messageText}>{item.content}</Text>
            <Text style={styles.timestamp}>
              {new Date(item.created_at).toLocaleTimeString()}
            </Text>
          </View>
        )}
        refreshing={loading}
        onRefresh={loadMessages}
      />

      <View style={styles.inputContainer}>
        <TextInput
          style={styles.input}
          value={messageText}
          onChangeText={setMessageText}
          placeholder="Type a message..."
          multiline
        />
        <Button title="Send" onPress={handleSend} />
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f5f5f5',
  },
  messageBubble: {
    maxWidth: '70%',
    padding: 12,
    borderRadius: 16,
    marginVertical: 4,
    marginHorizontal: 12,
  },
  sentMessage: {
    alignSelf: 'flex-end',
    backgroundColor: '#007AFF',
  },
  receivedMessage: {
    alignSelf: 'flex-start',
    backgroundColor: 'white',
  },
  senderName: {
    fontSize: 12,
    fontWeight: 'bold',
    marginBottom: 4,
    opacity: 0.7,
  },
  messageText: {
    fontSize: 16,
  },
  timestamp: {
    fontSize: 12,
    marginTop: 4,
    opacity: 0.6,
  },
  inputContainer: {
    flexDirection: 'row',
    padding: 12,
    backgroundColor: 'white',
    alignItems: 'center',
  },
  input: {
    flex: 1,
    borderWidth: 1,
    borderColor: '#ddd',
    borderRadius: 20,
    paddingHorizontal: 16,
    paddingVertical: 8,
    marginRight: 8,
  },
});

export default ChatScreen;
```

---

## MySQL for Backend

### MySQL Schema for Chat App

```sql
-- Create database
CREATE DATABASE chat_app;
USE chat_app;

-- Users table
CREATE TABLE users (
  id CHAR(36) PRIMARY KEY DEFAULT (UUID()),
  username VARCHAR(50) NOT NULL UNIQUE,
  email VARCHAR(255) NOT NULL UNIQUE,
  password_hash VARCHAR(255) NOT NULL,
  avatar_url VARCHAR(500),
  bio TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  INDEX idx_email (email),
  INDEX idx_username (username)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Conversations table
CREATE TABLE conversations (
  id CHAR(36) PRIMARY KEY DEFAULT (UUID()),
  name VARCHAR(100),
  is_group BOOLEAN DEFAULT FALSE,
  created_by CHAR(36),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  FOREIGN KEY (created_by) REFERENCES users(id) ON DELETE SET NULL,
  INDEX idx_created_by (created_by)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Conversation participants
CREATE TABLE conversation_participants (
  id CHAR(36) PRIMARY KEY DEFAULT (UUID()),
  conversation_id CHAR(36) NOT NULL,
  user_id CHAR(36) NOT NULL,
  joined_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  last_read_at TIMESTAMP NULL,
  FOREIGN KEY (conversation_id) REFERENCES conversations(id) ON DELETE CASCADE,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
  UNIQUE KEY unique_participant (conversation_id, user_id),
  INDEX idx_conversation (conversation_id),
  INDEX idx_user (user_id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Messages table
CREATE TABLE messages (
  id CHAR(36) PRIMARY KEY DEFAULT (UUID()),
  conversation_id CHAR(36) NOT NULL,
  sender_id CHAR(36) NOT NULL,
  content TEXT NOT NULL,
  type ENUM('text', 'image', 'file', 'voice') DEFAULT 'text',
  metadata JSON,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  FOREIGN KEY (conversation_id) REFERENCES conversations(id) ON DELETE CASCADE,
  FOREIGN KEY (sender_id) REFERENCES users(id) ON DELETE CASCADE,
  INDEX idx_conversation_created (conversation_id, created_at DESC),
  INDEX idx_sender (sender_id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Message reactions
CREATE TABLE message_reactions (
  id CHAR(36) PRIMARY KEY DEFAULT (UUID()),
  message_id CHAR(36) NOT NULL,
  user_id CHAR(36) NOT NULL,
  emoji VARCHAR(10) NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (message_id) REFERENCES messages(id) ON DELETE CASCADE,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
  UNIQUE KEY unique_reaction (message_id, user_id, emoji),
  INDEX idx_message (message_id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- View for conversation list with last message
CREATE VIEW conversation_list AS
SELECT 
  c.id,
  c.name,
  c.is_group,
  c.created_at,
  (
    SELECT JSON_ARRAYAGG(
      JSON_OBJECT(
        'id', u.id,
        'username', u.username,
        'avatar_url', u.avatar_url
      )
    )
    FROM conversation_participants cp
    JOIN users u ON u.id = cp.user_id
    WHERE cp.conversation_id = c.id
  ) as participants,
  (
    SELECT JSON_OBJECT(
      'id', m.id,
      'content', m.content,
      'sender_id', m.sender_id,
      'created_at', m.created_at
    )
    FROM messages m
    WHERE m.conversation_id = c.id
    ORDER BY m.created_at DESC
    LIMIT 1
  ) as last_message
FROM conversations c;

-- Stored procedure to get unread message count
DELIMITER //
CREATE PROCEDURE GetUnreadCount(
  IN p_conversation_id CHAR(36),
  IN p_user_id CHAR(36)
)
BEGIN
  SELECT COUNT(*) as unread_count
  FROM messages m
  LEFT JOIN conversation_participants cp 
    ON cp.conversation_id = m.conversation_id 
    AND cp.user_id = p_user_id
  WHERE m.conversation_id = p_conversation_id
    AND m.sender_id != p_user_id
    AND (cp.last_read_at IS NULL OR m.created_at > cp.last_read_at);
END //
DELIMITER ;
```

### Key Differences: PostgreSQL vs MySQL

| Feature | PostgreSQL | MySQL |
|---------|-----------|-------|
| **UUID Generation** | `uuid_generate_v4()` | `UUID()` |
| **JSON Type** | `JSONB` (binary, faster) | `JSON` |
| **Arrays** | Native arrays `TEXT[]` | Use JSON or separate table |
| **Text Search** | Built-in full-text search | FULLTEXT indexes |
| **Window Functions** | Full support | Limited support |
| **Case Sensitivity** | Case-sensitive by default | Case-insensitive by default |
| **Constraints** | More strict | More flexible |
| **Performance** | Better for complex queries | Better for simple reads |

---

## Chat App Example

### Complete Chat Architecture

```
┌─────────────────┐
│  React Native   │
│   Mobile App    │
└────────┬────────┘
         │
         ├─── Local DB (SQLite/Realm/WatermelonDB)
         │    • Offline messages
         │    • Cache
         │    • Sync queue
         │
         └─── Backend (Supabase/MySQL/PostgreSQL)
              • Authentication
              • Message sync
              • Real-time updates
              • File storage
```

### Hybrid Approach: WatermelonDB + Supabase

```typescript
// services/hybridChatService.ts
import { database } from '../database'; // WatermelonDB
import { supabase } from '../lib/supabase'; // Supabase
import NetInfo from '@react-native-community/netinfo';

export const hybridChatService = {
  // Send message with offline support
  sendMessage: async (
    conversationId: string,
    senderId: string,
    content: string
  ) => {
    // Save to local DB immediately
    const localMessage = await database.write(async () => {
      return await database.collections
        .get('messages')
        .create((message: any) => {
          message.conversationId = conversationId;
          message.senderId = senderId;
          message.content = content;
          message.type = 'text';
          message.isRead = false;
          message.syncStatus = 'pending'; // pending, synced, failed
        });
    });

    // Try to sync to Supabase
    const netInfo = await NetInfo.fetch();
    if (netInfo.isConnected) {
      try {
        const { data, error } = await supabase
          .from('messages')
          .insert([
            {
              conversation_id: conversationId,
              sender_id: senderId,
              content
            }
          ])
          .select()
          .single();

        if (error) throw error;

        // Update local message with server ID and mark as synced
        await database.write(async () => {
          await localMessage.update((msg: any) => {
            msg.serverId = data.id;
            msg.syncStatus = 'synced';
          });
        });
      } catch (error) {
        console.error('Sync error:', error);
        // Mark as failed, will retry later
        await database.write(async () => {
          await localMessage.update((msg: any) => {
            msg.syncStatus = 'failed';
          });
        });
      }
    }

    return localMessage;
  },

  // Sync pending messages when online
  syncPendingMessages: async () => {
    const pendingMessages = await database.collections
      .get('messages')
      .query(Q.where('sync_status', 'pending'))
      .fetch();

    for (const message of pendingMessages) {
      try {
        const { data, error } = await supabase
          .from('messages')
          .insert([
            {
              conversation_id: message.conversationId,
              sender_id: message.senderId,
              content: message.content
            }
          ])
          .select()
          .single();

        if (error) throw error;

        await database.write(async () => {
          await message.update((msg: any) => {
            msg.serverId = data.id;
            msg.syncStatus = 'synced';
          });
        });
      } catch (error) {
        console.error('Failed to sync message:', message.id, error);
      }
    }
  },

  // Fetch and cache messages from server
  fetchAndCacheMessages: async (conversationId: string) => {
    const { data, error } = await supabase
      .from('messages')
      .select('*')
      .eq('conversation_id', conversationId)
      .order('created_at', { ascending: false })
      .limit(50);

    if (error) throw error;

    // Save to local DB
    await database.write(async () => {
      for (const serverMessage of data) {
        // Check if message exists
        const existing = await database.collections
          .get('messages')
          .query(Q.where('server_id', serverMessage.id))
          .fetch();

        if (existing.length === 0) {
          await database.collections
            .get('messages')
            .create((message: any) => {
              message.serverId = serverMessage.id;
              message.conversationId = serverMessage.conversation_id;
              message.senderId = serverMessage.sender_id;
              message.content = serverMessage.content;
              message.type = serverMessage.type;
              message.syncStatus = 'synced';
            });
        }
      }
    });
  }
};
```

---

## Best Practices

### 1. Database Design

✅ **DO:**
- Use meaningful table and column names
- Add indexes on frequently queried columns
- Use foreign keys to maintain referential integrity
- Normalize data to avoid duplication
- Add timestamps (created_at, updated_at)
- Use constraints (NOT NULL, UNIQUE, CHECK)

❌ **DON'T:**
- Store arrays as comma-separated strings
- Use generic names like "data" or "info"
- Skip indexes on foreign keys
- Store JSON when relational makes more sense
- Forget to handle cascading deletes

### 2. Performance

✅ **DO:**
- Use pagination (LIMIT and OFFSET)
- Create indexes on WHERE clause columns
- Use prepared statements
- Batch operations when possible
- Cache frequently accessed data locally
- Use transactions for multiple related operations

❌ **DON'T:**
- Use SELECT * in production
- Query inside loops
- Fetch all data at once
- Skip connection pooling (backend)
- Ignore query performance

### 3. Security

✅ **DO:**
- Use Row Level Security (RLS) in Supabase/PostgreSQL
- Validate and sanitize all inputs
- Use parameterized queries (prevent SQL injection)
- Hash passwords (never store plain text)
- Implement proper authentication
- Use environment variables for sensitive data

❌ **DON'T:**
- Trust user input
- Expose sensitive data in client
- Use weak password hashing
- Skip authorization checks
- Hardcode API keys or secrets

### 4. Mobile-Specific

✅ **DO:**
- Implement offline-first architecture
- Sync data in background
- Handle network failures gracefully
- Use incremental sync (only fetch changes)
- Compress large payloads
- Show loading states
- Cache images and assets locally

❌ **DON'T:**
- Assume constant internet connection
- Fetch large datasets on mobile networks
- Block UI during database operations
- Skip error handling
- Forget to clean up old cached data

### 5. Code Organization

✅ **DO:**
- Separate database logic into services
- Use TypeScript for type safety
- Create reusable database utilities
- Write migrations for schema changes
- Document complex queries
- Use environment-based configurations

❌ **DON'T:**
- Write SQL in components
- Mix business logic with database logic
- Skip error handling
- Hardcode database credentials
- Forget to handle edge cases

---

## Comparison: Which Database to Use?

| Database | Best For | Pros | Cons |
|----------|----------|------|------|
| **SQLite** | Simple local storage, settings, offline-first | Lightweight, no setup, fast | No built-in sync, manual SQL |
| **Realm** | Offline-first, object-oriented apps | Easy to use, real-time, sync built-in | Learning curve, vendor lock-in |
| **WatermelonDB** | Large datasets, performance-critical | Very fast, reactive, SQLite-based | More complex setup, newer library |
| **Supabase** | Full backend solution, MVP | Quick setup, real-time, PostgreSQL | Requires internet, learning curve |
| **PostgreSQL** | Complex queries, analytics | Powerful, reliable, feature-rich | Requires server management |
| **MySQL** | Web apps, simple queries | Fast reads, widely used | Less features than PostgreSQL |

### Recommendation for Different App Types:

**Simple Todo App:**
- SQLite (local only)

**Chat App:**
- WatermelonDB (local) + Supabase (backend)

**Social Media:**
- WatermelonDB (local) + PostgreSQL (backend)

**E-commerce:**
- Realm (local with sync) or WatermelonDB + PostgreSQL

**Offline-First Field App:**
- Realm with MongoDB Atlas sync

---

## Resources

### Documentation
- [SQLite Official Docs](https://www.sqlite.org/docs.html)
- [Realm Documentation](https://www.mongodb.com/docs/realm/)
- [WatermelonDB Docs](https://watermelondb.dev/docs)
- [Supabase Docs](https://supabase.com/docs)
- [PostgreSQL Docs](https://www.postgresql.org/docs/)
- [MySQL Docs](https://dev.mysql.com/doc/)

### Tools
- [DB Browser for SQLite](https://sqlitebrowser.org/) - GUI for SQLite
- [Postico](https://eggerapps.at/postico/) - PostgreSQL client (Mac)
- [pgAdmin](https://www.pgadmin.org/) - PostgreSQL admin
- [MySQL Workbench](https://www.mysql.com/products/workbench/) - MySQL GUI

### Learning SQL
- [SQL Tutorial - W3Schools](https://www.w3schools.com/sql/)
- [SQLBolt](https://sqlbolt.com/) - Interactive SQL lessons
- [PostgreSQL Tutorial](https://www.postgresqltutorial.com/)

---

## Conclusion

You now have a comprehensive understanding of:

✅ SQL fundamentals and advanced queries
✅ Database design from simple to complex
✅ SQLite implementation in React Native
✅ Realm object database
✅ WatermelonDB for performance
✅ Supabase with PostgreSQL
✅ MySQL for backend
✅ Real-world chat app implementation
✅ Best practices and security

**Next Steps:**
1. Practice SQL queries in [SQLite Browser](https://sqlitebrowser.org/)
2. Build a simple todo app with SQLite
3. Create a chat app using one of the databases
4. Experiment with Supabase real-time features
5. Implement offline sync with WatermelonDB

Happy coding! 🚀
