## Keys and Labels

[Previous: Getting Started](getting-started.md)

A table full of numbers can mean nothing. But with its column & row headers a table can be understood. 
Smrtr DataGrid has a fluent interface for handling the identity of rows and columns through the use of Keys & Labels.

### Keys
Row keys and column keys are used internally to identify & distinguish rows and columns from one another.
Both sets of keys start from 0 and increase naturally for each row/column added to the grid.
As a result, the number of rows in a grid will always be one more than the largest row key. The same goes for columns.

 - Every row can be identified by a unique integer - its *row key*
 - Every column can be identified by a unique integer - its *column key*

### Labels
In addition to a unique key, every row and column may optionally be described by a unique label.
Labels are unique strings which are provided to give some context or meaning to the data they represent.

 - Labels are not required
 - Labels are set to `NULL` by default

### Commonly Used Methods

> **$rowOrColumn**: Methods accepting a `$rowOrColumn` parameter are looking for a string containing 'row' or 'column'.
> Any other value will likely result in an exception being thrown.

> **$keyOrLabel**: Methods accepting the `$keyOrLabel` parameter are looking for a key or label to identify the required
> row or column. Be sure to provide keys as integers and labels as strings.
> The string '1' will be treated as a label.

#### getKey( `$rowOrColumn`, `$keyOrLabel` )
Returns an integer: the key of the specified row or column.

You can also use the shorthand methods `$Grid->getRowKey('some row')` and `$Grid->getColumnKey('my column')`.

#### getKeys( `$rowOrColumn` )
Returns an array: the keys for the rows or columns.

Shorthand: `$Grid->getRowKeys()` and `$Grid->getColumnKeys()`.

#### getLabel( `$rowOrColumn`, `$key` )
Returns a string or `NULL`: the unique label of the specified row or column. Parameter `$key` must be an int.

Shorthand: `$Grid->getRowLabel(5)` and `$Grid->getColumnLabel($someColumnKey)`

#### getLabels( `$rowOrColumn` )
Returns an array: the labels for the rows or columns, indexed by the keys.

Shorthand: `$Grid->getRowLabels()` and `$Grid->getColumnLabels()`.

#### hasKey( `$rowOrColumn`, `$key` )
Returns a boolean. 

Shorthand: `$Grid->hasRowKey(7)` & `$Grid->hasColumnKey(35)`.

#### hasLabel( `$rowOrColumn`, `$label` )
Returns a boolean. 

Shorthand: `$Grid->hasRowLabel('one')` & `$Grid->hasColumnLabel('64')`.

[Next: Data Getters](data-getters.md)