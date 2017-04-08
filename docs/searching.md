## Searching

[Previous: Iterating & Filtering](iterating-filtering.md)

Inspired by jQuery, the search methods provided by Smrtr DataGrid comprise a prime featureset which take this class from
just being a 2D array wrapper to being a manageable, query-able data store.

### Methods
Smrtr DataGrid allows you to use selector strings to search the grid much like you would use CSS selectors to search the
DOM tree in jQuery. Where the grid has labelled data, those labels can be used in a selector string to search for
particular value(s) in the vector identified by that label.

#### searchRows( `$expression` )
Performs a search query on the grid's rows based on the string `$expression` and returns a new
`\Smrtr\DataGrid` object containing the matched rows.

###### Example:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array(1, 2, 3),
      array(2, 4, 6),
      array(4, 8, 12)
    ));
    $grid->columnLabels(array('first', 'second', 'third'));
    $result = $grid->searchRows('first=2');
    print_r($result->getArray());
    // [ [ 2, 4, 6 ] ]

#### searchColumns( `$expression` )
Performs a search query on the grid's columns based on the string `$expression` and returns a new
`\Smrtr\DataGrid` object containing the matched columns.

###### Example:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array(1, 2, 3),
      array(2, 4, 6),
      array(4, 8, 12)
    ));
    $grid->rowLabels(array('row_A', 'row_B', 'row_C'));
    $result = $grid->searchColumns('row_B=6');
    print_r($result->getArray());
    // [ [ 3, 6, 12 ] ]

### Selectors
In the examples above we introduced the equality operator (`field=value`) which looks for values equal to the value
provided. Our selectors are defined as follows:

 - **Field(s):** identifies the row / column / key / label to be checked.
 - **Operator:** defines how to check the field.
 - **Value(s):** the value(s) to check the field against.

**Special Characters:** `+ - , ( ) | " ' \ > = < ! ^ * $`

These characters have special meaning, which will be explained in the following sections.
Do not use these characters anywhere in your search expression unless you understand their meaning:

#### Fields
 - `/` is a special field which maps to the keys of the vectors being searched.
 - `//` is a special field which maps to the labels of the the vectors being searched.
 - `/n`, where `n` is a number, is a special field which maps to a key perpendicular to the vector being searched.
   *E.g. passing `/0=bar` into `searchRows()` will look for rows with the value 'bar' in the first column, while passing
   `/1=bar` into `searchColumns()` will look for columns with the value 'bar' in the second row.* 
 - Any other string (excluding special characters) will be mapped to a label perpendicular to the vectors being
   searched. *E.g. passing `foo=bar` into `searchRows()` will look for a column named "foo", while passing `foo=bar`
   into `searchColumns()` will look for a row labelled "foo".*
 - Fields can be grouped together with the `|` character (pipe) to form a composite field
   (a field that matches whenever at least one of its sub-fields matches).

###### Examples:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array(1, 2, 3),
      array(2, 4, 6),
      array(4, 8, 12)
    ));
    $grid->rowLabels(array('row_A', 'row_B', 'row_C'));
    $grid->columnLabels(array('col_A', 'col_B', 'col_C'));
    
    print_r($grid->searchRows('//=row_A')->getArray());
    // [ [ 1, 2, 3 ] ]
    
    print_r($grid->searchColumns('/=2')->getArray());
    // [ [ 3, 6, 12 ] ]
    
    print_r($grid->searchRows('col_A|/1=2')->getArray());
    // [ [ 1, 2, 3 ], [ 2, 4, 6 ] ]

##### Escaping special characters in field names
Field names with special characters in them should be quoted.

If the field name also contains quote characters matching the quote character used to quote the field name,
then those quote characters should be escaped with a backslash `\`. 

If the value already contains backslashes, they should also be escaped with an extra backslash.

###### Examples:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array(1, 2, 8),
      array(2, 4, 16),
      array(4, 8, 32)
    ));
    $grid->columnLabels(array('|', '||', '|||"'));
    
    print_r($grid->searchRows('"|"|"||"=4')->getArray());
    // [ [ 2, 4, 16 ], [ 4, 8, 32 ] ]
    
    print_r($grid->searchRows('"||"|"|||\""=8')->getArray());
    // [ [ 1, 2, 8 ], [ 4, 8, 32 ] ]

#### Operators

 - `=`: Checks for equality and matches if equal to provided value.
 - `!=`: Checks for equality and matches if not equal to provided value.
 - `<`: Checks for numeric size and matches if less than provided value.
 - `<=`: Checks for numeric size and matches if less than or equal to provided value.
 - `>`: Checks for numeric size and matches if greater than provided value.
 - `>=`: Checks for numeric size and matches if greater than or equal to provided value.
 - `*=`: Checks for presence and matches if string contains provided value. *case-insensitive.*
 - `^=`: Checks for presence and matches if string starts with provided value. *case-insensitive.*
 - `$=`: Checks for presence and matches if string ends with provided value. *case-insensitive.*

###### Examples:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array(1, 2, 3),
      array(2, 4, 6),
      array(4, 8, 12)
    ));
    $grid->rowLabels(array('row_A', 'row_B', 'row_C'));
    $grid->columnLabels(array('col_A', 'col_B', 'col_C'));
    
    print_r($grid->searchColumns('row_C<10')->getArray());
    // [ [ 1, 2, 4 ], [ 2, 4, 8 ] ]
    
    print_r($grid->searchRows('col_B>=4')->getArray());
    // [ [ 2, 4, 6 ], [ 4, 8, 12 ] ]
    
    print_r($grid->searchRows('//^=row_')->getArray());
    // [ [ 1, 2, 3 ], [ 2, 4, 6 ], [ 4, 8, 12 ] ]
    
    print_r(
      $grid->searchRows('/0|/1|/2*=1')->getArray()
    );
    // [ [ 1, 2, 3 ], [ 4, 8, 12 ] ]

#### Values
Like fields, values can be grouped together with the `|` character to form a composite value
(a value that matches whenever at least one of its sub-values matches).

###### Examples:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array("one", "two", "three"),
      array("two", "four", "six"),
      array("four", "eight", "twelve")
    ));
    $grid->rowLabels(array('row_A', 'row_B', 'row_C'));
    $grid->columnLabels(array('col_A', 'col_B', 'col_C'));
    
    print_r($grid->searchRows('col_A=one|four')->getArray());
    // [ [ "one", "two", "three" ], [ "four", "eight", "twelve" ] ]
    
    print_r($grid->searchColumns('row_B*=w|x')->getArray());
    // [ [ "one", "two", "four" ], [ "three", "six", "twelve" ] ]

##### Escaping special characters in values
Values with special characters in them should be quoted.

If the value also contains quote characters matching the quote character used to quote the value,
then those quote characters should be escaped with a backslash `\`.

If the value already contains backslashes, they should also be escaped with an extra backslash.

###### Examples:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array('"Magic" Johnson', 100),
      array(' dragons', 64)
    ));
    $grid->rowLabels(array('row1', 'row2'));
    $grid->columnLabels(array('legend', 'rating'));
    
    print_r($grid->searchRows('legend*="\"Magic\""')->toArray());
    // [ [ '"magic" Johnson', 100 ] ]

### Complex expressions
In this section we explain how to combine multiple selectors into a single search expression.

#### `,` OR
Use the `,` *(comma)* group operator to join two selectors A and B when you want to
return all vectors that match A or B (or both).

###### Example:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array("Wayne", "Rooney", "striker", 27),
      array("Kieran", "Gibbs", "left back", 23),
      array("Gareth", "Barry", "midfielder", 32),
      array("Theo", "Walcott", "striker", 24)
    ));
    $grid->columnLabels(array(
      "First Name", "Last Name", "Position", "Age"));
    $grid->rowLabels(array("WR", "KG", "GB", "TW"));
    
    print_r($grid->searchRows('Age<25 , Position=striker')->getRowLabels());
    // [ "KG", "TW", "WR" ]

#### `+` AND
Use the `+` *(plus)* group operator to join two selectors A and B when you want to
return all vectors that match both A and B.

###### Example:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array("Wayne", "Rooney", "striker", 27),
      array("Kieran", "Gibbs", "left back", 23),
      array("Gareth", "Barry", "midfielder", 32),
      array("Theo", "Walcott", "striker", 24)
    ));
    $grid->columnLabels(array(
      "First Name", "Last Name", "Position", "Age"));
    $grid->rowLabels(array("WR", "KG", "GB", "TW"));
    
    print_r($grid->searchRows('Age<25 + Position=striker')->getRowLabels());
    // [ "TW" ]

#### `-` NOT
Use the `-` *(minus)* group operator to join two selectors A and B when you want to
return all vectors that match A but not B.

###### Example:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array("Wayne", "Rooney", "striker", 27),
      array("Kieran", "Gibbs", "left back", 23),
      array("Gareth", "Barry", "midfielder", 32),
      array("Theo", "Walcott", "striker", 24)
    ));
    $grid->columnLabels(array(
      "First Name", "Last Name", "Position", "Age"));
    $grid->rowLabels(array("WR", "KG", "GB", "TW"));
    
    print_r($grid->searchRows('Age<25 - Position=striker')->getRowLabels());
    // [ "KG" ]

#### Nested Expressions
Nested expressions are also supported using mathematical bracket notation with `(` and `)`.

###### Examples:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array("Wayne", "Rooney", "striker", 27),
      array("Kieran", "Gibbs", "left back", 23),
      array("Gareth", "Barry", "midfielder", 32),
      array("Theo", "Walcott", "striker", 24)
    ));
    $grid->columnLabels(array(
      "First Name", "Last Name", "Position", "Age"));
    $grid->rowLabels(array("WR", "KG", "GB", "TW"));
    
    $s = 'Age<30 + (Position="left back" , Position=midfielder)';
    print_r($grid->searchRows($s)->getRowLabels());
    // [ "KG" ]
    
    $s = '(Age<30 + Position="left back") , Position=midfielder';
    print_r($grid->searchRows($s)->getRowLabels());
    // [ "KG", "GB" ]