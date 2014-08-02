---
title: Inserting records with zero as ID value in MySQL
layout: post
categories: mysql legacy
---

Working with a legacy database can lead to funny things.

I needed to create a table that for some reason had to have its IDs synchronized with a existing legacy table. Sounds easy, right?

I ran this SQL query on a MySQL server:

~~~sql
INSERT INTO `foo` (`id`, `column1`, `column2`) VALUES
	(0, 'dummy01', 'dummy02'),
	(1, 'dummy11', 'dummy12'),
	(2, 'dummy21', 'dummy22');
~~~

Note that I provided the IDs and the sequence starts with zero instead of the more common 1 value.

Running this query I got this error message:

> Duplicate entry '1' for key 'PRIMARY'

Sounds weird isn't it?

After some research I found that MySQL interprets an ID value of zero as "please use the next ID in the sequence" instead on the literal zero value.

Here is [an excerpt of the documentation](https://dev.mysql.com/doc/refman/5.5/en/example-auto-increment.html) explaining this behavior (emphasis is mine):

> No value was specified for the AUTO_INCREMENT column, so MySQL assigned sequence numbers automatically. **You can also explicitly assign 0 to the column to generate sequence numbers.** If the column is declared NOT NULL, it is also possible to assign NULL to the column to generate sequence numbers.

So the first record was assigned the next ID in the sequence, 1, and the second record which explicitly specified 1 as a value for the ID triggered a duplicate primary key error.

Now that we understand the reasoning behind this error how can we workaround it because I'm working with a legacy database which means that I can't change it easily. I want to do that only as a last resort because it might have bad side-effects.

Fortunately, there is an easy workaround modifying `sql_mode`:

~~~sql
SET @sql_mode_backup=@@SESSION.sql_mode;
SET SESSION sql_mode='NO_AUTO_VALUE_ON_ZERO';

INSERT INTO `foo` (`id`, `column1`, `column2`) VALUES
	(0, 'dummy01', 'dummy02'),
	(1, 'dummy11', 'dummy12'),
	(2, 'dummy21', 'dummy22');                
		
SET SESSION sql_mode=@sql_mode_backup;
~~~

To explain the magic happening here, the first line saves current session `sql_mode`, second line changes `sql_mode` to `NO_AUTO_VALUE_ON_ZERO ` for the current session and the last line finally restores the saved `sql_mode`.
