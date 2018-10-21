---
title: 'Writing MySQL schema migrations: best practices'
date: 2011-10-08 17:50:25 -04:00
permalink: "/2011-10-08/mysql-schema-migrations-best-practices/"
categories:
- Notebook
tags:
- best practices
- migration
- MySQL
id: 311
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=311
---

Building MySQL web applications with a team of developers will inevitably present the challenge of database schema changes. Any good web developer understands the importance of keeping all code under version control, but how many follow the same principle for the database?

What follows are some best practices that I have learned over the past several years from the school of hard knocks.<!--more-->

## Use revision control

  * **Version control all schema documentation** and setup files, right alongside your code. This could be in the form of a <a href="http://www.mysql.com/products/workbench/" target="_blank">MySQL Workbench</a> file (.mwb), SQL, plaintext, PHP, or HTML files.
  * **Version control your database schema.** I personally prefer to use individual SQL files for this purpose, one file per revision (each revision file could contain multiple SQL statements). Include INSERTs for tables that need to be pre-populated for your application to work properly, such as role or permission options, country or state data, and categories.
  * **Correlate code revisions and database schema revisions.** For each codebase revision, there should be a way to re-create the corresponding database schema. An effective way to do this is to name individual SQL revision files after the codebase revision in which they were created (e.g. `r389.sql`).
  * **Store the database version in the database.** You should be able to tell what database schema version you are running, and the codebase version that goes with it.

## Rules of engagement

  * **Write readable SQL.** Proper indentation, newlines, and comments are as important in your SQL as they are in other types of code. See my [MySQL Style Guide](http://jonathonhill.net/coding-for-readability/mysql-style-guide/) for a suggested way to do it.
  * **Terminate each SQL statement with the proper delimiter.** Usually, this will be a semicolon. This is recommended even if there is only a single SQL statement in the migration to prevent query syntax errors when running multiple migrations.
  * **Migrate forwards _and_ backwards.** When writing a database migration, include a way to roll back to the previous revision _after_ the migration is already applied. Your version control system provides this for your code, but you&#8217;ll be limited if you can&#8217;t roll back your database too. You can do this by creating pairs of migration files for each revision, one to make a change, and one to undo it.
  * **<span style="text-decoration: underline;">Always</span> test your migrations**, no excuses. Don&#8217;t change your database schema with a GUI tool and _then_ write a migration script as an afterthought. Write your migration, and run it on your own database before committing it to the codebase.
  * **Never change a migration file,** unless you are certain that no one else has run the migration yet. If you test, you shouldn&#8217;t ever need to anyway.

## Write bulletproof migrations

  * **Consider index speed.** For large tables, re-indexing can take a while. Consider dropping an index and re-adding it after altering indexed columns or running an `UPDATE` query.
  * **Omit column order clauses** such as ``ADD COLUMN `column_b` VARCHAR(50) <span style="text-decoration: underline;">AFTER `column_a`</span>``. Unless a very specific column order is required, this is unnecessary and can introduce errors if a database happens to lack `` `column_a` `` during migration.
  * **Preface `CREATE TABLE` statements with `DROP TABLE IF EXISTS`.** Likewise, use **`DROP TABLE IF EXISTS`** when removing a table.
  * **Avoid key conflicts** by running a `DELETE` or `TRUNCATE` before `INSERT`, or use `REPLACE` instead.
  * **Set character set and collation settings at the database level**, and remove references to specific character sets or collations at the table or column level. MySQL Workbench is notorious for adding these when changing or creating `VARCHAR()` or `TEXT` columns, which can cause display or foreign key problems if a mismatch occurs.
  * **Beware of losing or corrupting data.** Data loss may be insignificant during development, but consider the impact your migration will have on a production database. 
      * Altering a column is generally preferable to dropping and re-adding a column.
      * When changing between incompatible datatypes (such as changing from `VARCHAR()` to `DATE`), you should use this sequence: 
          1. Rename the old column or table,
          2. Add the new column or table,
          3. Run an `UPDATE` query to reformat and copy the data from the old location to the new one, and _then_
          4. Drop the old column or table.
  * **Be aware of index name length limits.** MySQL silently truncates really long index names, and if you try to add a similar (long) index name, it could inexplicably clash with the other index.

## Play nice with InnoDB

MySQL&#8217;s InnoDB storage engine, now the default in MySQL 5.5, is very strict and often obscure in its error messages. These rules will save you lots of grief when applying migrations that affect InnoDB tables, especially when it involves foreign key constraints.

  * **Avoid suppressing InnoDB foreign key checks** with `FOREIGN_KEY_CHECKS=0`, unless you know what you are doing and have a very good reason.
  * **Match foreign keys <span style="text-decoration: underline;">exactly</span>.** Type, length, attributes (signed/unsigned, nullable, etc), character set, collation, and storage engine must all match identically.
  * **Drop foreign key constraints before altering primary or foreign keys.** You&#8217;ll need to use separate `ALTER TABLE` statements for this, one to remove the constraint and change the column, and one to re-add the constraint.
  * **Drop the primary key index before changing a primary key column.** Much like the previous point, but you&#8217;ll need to run `DROP PRIMARY KEY`, make your change, and then ``ADD PRIMARY KEY (`column`)`` afterwards.
  * **Remove references to specific databases**. Your foreign keys should not depend on the presence of another specific database, and should work regardless of the database name. MySQL Workbench is notorious for adding schema prefixes to table names when creating foreign key constraints.
  * **Be aware of foreign key constraint name length limits.** See the point &#8220;Be aware of index name length limits&#8221; above.

## Smooth the migration process

  * ******Apply all the statements for a single revision inside a transaction******, so that whenever it fails, your database is not left in an unstable state. This way, you can fix the error and re-run the entire migration.****
  
**** 
  * ****Automate schema migrations.**** Don&#8217;t make your co-workers apply schema revisions by hand; script it.**
  
** 
  * **Take a backup first.*** On production databases, this is just raw common sense. Take precautions against Murphy&#8217;s law.
  * **Test on a backup copy.*** Some bugs can lurk in production databases that might not be present in your development database due to data differences. Know that your migration will work on the production database, before you apply it.

* Specific to production databases.