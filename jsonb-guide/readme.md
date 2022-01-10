### **What is `JSON`?**

1. JSON - JavaScript Object Notation 
2. JSON has become the most popular data interchange format for web applications.

### **PostgreSQL support**

1. PostgreSQL supported `**JSON**` Data type since the 9.2 version.
2. Version 9.4 introduced the **`JSONB`** data type, a binary form of **`JSON`** that can also take advantage of indexes.
3. Version 9.3 significantly beefed up `**JSON**` support with new functions for extracting, editing, and casting to other data types.
- **How JSON syntax is defined?**
    
    A JSON object consists of key/value pairs, surrounded by “{}” and separated by “,” as follows:
    
    `{"name": {"first_name": "Lily", "last_name":"Bush"}}`
    
    Values can just be more **JSON objects,** so nesting with arbitrary hierarchies is possible.
    
    `{"toyes": ["Toy Car", "Toy Train"]}`
    
    Objects can also be arrays (an ordered lists of objects)
    
- **How JSON are used in PostgreSQL? What are JSON and JSONB data types?**
    
    PostgreSQL offers two types for storing JSON data: `json` and `jsonb`.
    
    ```sql
    CREATE TABLE orders (
    	id serial NOT NULL PRIMARY KEY,
    	info json NOT NULL
    );
    ```
    
    The `orders` table consists of two columns:
    
    1. The `id` column is the primary key column that identifies the order.
    2. The `info` column stores the data in the form of JSON.
    
    The `JSON` and `JSONB` data types accept *almost* identical sets of values as input.
    
- **The key difference between `JSON` and `JSONB`**
    
    Let's look at an example:
    
    ```sql
    SELECT '{"c":0,   "a":2,"a":1}'::json, '{"c":0,   "a":2,"a":1}'::jsonb;
    
    json                   | jsonb
    -----------------------+------------------
    {"c":0,   "a":2,"a":1} |  { "a": 1,"c": 0}
    ```
    
    1. `JSON` is stored in its plain text format, while `JSONB` is stored in some binary representation.
    2. `JSON` stores white space and that is why we can see spaces when key "a" is stored, while `JSONB` does not.
    3.   `JSON` **stores all** the values of a key. This is the reason you can see multiple values (2     and 1) against the key "a", while `JSONB` only **stores the last value**.
    
    1. `JSON` maintains the order in which elements are inserted, while `JSONB` maintains the "sorted" order.
    2. `JSONB` objects are stored as a **decompressed binary** as opposed to **raw data** in `JSON`, where no reparsing of data is required during retrieval.
    3. `JSONB` also supports indexing, which can be a significant advantage.
    
- **When Should You Use `JSON` vs. `JSONB`?**
    1. If you only work with the JSON representation in your application, PostgreSQL is only used to store & retrieve this representation, you should use `JSON`.
    2. If you do a lot of operations on the JSON value in PostgreSQL, or use indexing on some JSON field, you should use `JSONB`.

### **How to use JSONB?**
1. **Creating a DB and a Table**
    ```sql
    CREATE TABLE books (
        book_id serial NOT NULL,
        data JSONB
    );
    -- CREATE TABLE
    
    ```

2. **Populating the DB**
    PostgreSQL automatically validates the input to make sure what you are adding is valid JSON.
    
    ```sql
    INSERT INTO books VALUES 
	(1, '{"title": "Atomic Habits","genres": ["Productivity", "Non-Fiction"], "authors": ["James Clear"],"published": true}'),
	(2, '{"title": "The Psychology of Money","genres": ["Mindset", "Non-Fiction"], "authors": ["Morgan Housel"],"published": false}');
    -- Inserted Row (0,2)
    ```
    
3. **Querying JSON**
    The easiest way to traverse the hierarchy of a JSON object is by using pointer symbols.
    ```sql
    SELECT id, data->'title' AS title FROM books;

        title
    ---------------------------
    "Atomic Habits"
    "The Psychology of Money"
    (2 rows)
    ```
    > **-> operator returns values out of JSON columns in JSON format**

4. **Filter Results**
    You may also filter a result set no differently as you normally would, using the WHERE clause but through JSON keys:
    ```sql
    SELECT * FROM books WHERE data->'published' = 'false';
    ```
    Which in this case returns the raw JSON data:
    ```sql
        book_id |                                              data
    ---------+-------------------------------------------------------------------------------------------------------------------------
        2 | {"title": "The Psychology of Money", "genres": ["Mindset", "Non-Fiction"], "authors": ["Morgan Housel"],"published": false}
    (1 row)
    ```




