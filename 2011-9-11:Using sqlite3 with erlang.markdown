# Using sqlite3 with erlang part 1

In this post i want to tell about my experience with Erlang and sqlite3.

In one of projects we needed in embedded relational database management system, and our choise was sqlite3 <http://www.sqlite.org/>

There are not many sqlite3 drivers for erlang. I choose sqlite-erlang by ttyerl <https://github.com/ttyerl/sqlite-erlang> and it's fork erlang-sqlite3 by alexeyr <https://github.com/alexeyr>

  * https://github.com/alexeyr/erlang-sqlite3
  * https://github.com/ttyerl/sqlite-erlang
  
Finaly i choose erlang-sqlite3.

Installing instructions you can find in erlang-sqlite3 README.md <https://github.com/alexeyr/erlang-sqlite3/blob/master/README.md>

## Createing tables

First of all, to work with sqlite3/erlang we must create table.

For example Customers table with two fields:

```
*--------------------------*
|      id     |    data    |
*--------------------------*

```

Let's create this table:


```Erlang
%
% Init sqlite3
%
sqlite3:open(customersdb, [in_memory]).
%
% Table information
% Id - field name
% integer - data type
% [not_null] - field options
%
TableInfo = [{id, integer, [not_null]}, {data, text, [not_null]}],
%
% Create new table
% customersdb - db name
% customers - table name
% TableInfo - table options
%
sqlite3:create_table(customersdb, customers, TableInfo).
```

sqlite3 provides next data types - integer | text | double | blob | atom() | string().

## Create some querying

Let's create some querying to our database.

*Write some data to our db:*

```Erlang
Id1 = 1,
Data1 = "Some data",
sqlite3:write(customersdb, customers, [{id, Id1}, {data, Data1}]),
Id2 = 2,
Data2 = "Some second data",
sqlite3:write(customersdb, customers, [{id, Id2}, {data, Data2}]).
```

Delete from db:

```Erlang
sqlite3:delete(customersdb, customers, {id, 2}).
```

Execute SQL query:

```Erlang
sqlite3:sql_exec(customersdb, "SELECT data FROM customers WHERE id = ?", [{1, 1}]).
```

In this example we can see [{1, 1}] parameter. It's mean: first 1 is number of query ? parameter, second 1 is value of parameterized query, we try to select data from customers table where id = 1.

## Close db

And at last we must close out data base

```Erlang
sqlite3:close(customersdb).
```

I think it will be useful information for you.

*Fork this post for comments*
