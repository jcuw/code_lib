# Concept

### MySQL vs Oracle SQL 
    MySQL is open source whereas Oracle SQL is built for enterprise and it's not free.

### Schema
- A database contains multiple objects(i.e. tables, views, stored procedures, functions, etc.). We can think of `schema` as a collection of tables.

### INT VS BIGINT
- Range for `int` is actually not that big +/- 2,147,483,648 (2.1B)
- `BIGINT` is capable of handling values +/- -9,223,372,036,854,775,808 (9.2M trillion). It takes up 8 bytes of storage instead of 4.

### Double vs Float vs Decimal:
- `Double` and `Float` are approximate whereas `Decimal` is exact
- The key difference between the three is the level of precision:

    - `Float` (32 bits): 7 digits accuracy
    - `Double` (64 bits): 15-16 digits accuracy (aka doubled precision)
    - `Decimal`(128 bits): 28-29 significant digits. Most financial applications use it for computation.

### CHAR vs VARCHAR
-  `CHAR` stores plain text with fixed length
- `VARCHAR` stores text with dynamic lengths. mySQL default is `varchar(45)`

### TEXT VS LONGTEXT
- `TEXT` takes 2-byte overhead. Meaning it will occupy (# of char used +2) bytes in memory. 100 char text will occupy 102 bytes in memory.
- `LONGTEXT` takes 4-byte overhead. It can store ~4.3B bytes of data (~4GB)

### BLOB
- Binary Large Object (`BLOB`) stores large unstructured data including images or videos. It's commonly seen in web development (storing vid/pic)

# Syntax

### Create Schema
    CREATE SCHEMA `new_schema` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

`Character encoding` is the process of assining binary into characters that human can understand. `utf8mb4`,or 4-Byte UTF-8 Unicode Encoding, is an example of the most popular character encoding.

`utf8mb4_unicode_ci` is an extension of UTF-8 that enables the storage of emoji-related symbool.

### Create Table
    CREATE TABLE `new_schema`.`new_table` (
    `id` INT NOT NULL COMMENT 'This is a primary index',
    PRIMARY KEY (`id`)
    );
### Delete Table
    DROP TABLE `new_schema`.`new_table`;
### Clean Table
    TRUNCATE `new_schema`.`new_table`;
`TRUNCATE` is used to clear table contents

### Create new column
    ALTER TABLE `new_schema`.`users`
    ADD COLUMN `age` INT NULL AFTER `user_name`;

### Update column 
    ALTER TABLE `new_schema`.`users`
    CHANGE COLUMN `name` `user_name` VARCHAR(45) NOT NULL DEFAULT 'No Name';