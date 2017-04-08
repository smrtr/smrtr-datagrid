## Data getters

[Previous: Keys & Labels](keys-labels.md)

These methods return a set of data from the grid.

### Getting an individual data point

#### getValue( `$rowKeyOrLabel`, `$columnKeyOrLabel` )
The `$rowKeyOrLabel` and `$columnKeyOrLabel` parameters each expect a key (int) or a label (string),
which are used to find the co-ordinates of an existing data point on the grid and return it.

###### Example grid:
    <?php
    $grid = new \\Smrtr\\DataGrid(array(
      array('top left', 'top right'),
      array('bottom left', 'bottom right')
    ));
    $grid->rowLabels(array('top', 'bottom'))
         ->columnLabels(array('left', 'right'));

###### Using keys:
    <?php
    echo $grid->getValue(1, 0);
    // "bottom left"

###### Using labels:
    <?php
    echo $grid->getValue('top', 'right');
    // "top right"

###### Using a mix of keys and labels:
    <?php
    echo $grid->getValue(0, 'left');
    // "top left"
    
    echo $grid->getValue('bottom', 1);
    // "bottom right"

### Getting row data

#### getRow( `$keyOrLabel` )
Accepts a key (int) or a label (string) from the `$keyOrLabel` parameter which identifies a row and returns an array
containing the data in that row.

*The returned array is of length `n` where `n` is the number of columns on the grid.*

    <?php
    $grid = new \\Smrtr\\DataGrid(array(
      array('foo', 'bar', 'foo', 'baz', 'baz'),
      array('first', 'second', 'third')
    ));
    
    print_r($grid->getRow(0));
    // [ 0=>"foo", 1=>"bar", 2=>"foo", 3=>"baz", 4=>"baz" ]
    
    print_r($grid->getRow(1));
    // [ 0=>"first", 1=>"second", 2=>"third", 3=>null, 4=>null ]

#### getRowDistinct( `$keyOrLabel` )
The same as `getRow()` except that repeated values and null values are removed from the returned array. 

*As such, the returned array is of variable length.*

    <?php
    $grid = new \\Smrtr\\DataGrid(array(
      array('foo', 'bar', 'foo', 'baz', 'baz'),
      array('first', 'second', 'third')
    ));
    
    print_r($grid->getRowDistinct(0));
    // [ 0=>"foo", 1=>"bar", 2=>"baz" ]

### Getting column data

#### getColumn( `$keyOrLabel` )
Accepts a key (int) or a label (string) from the `$keyOrLabel` parameter which identifies a column and returns
an array containing the data in that column.

*The returned array is of length `n` where `n` is the number of rows on the grid.*

    $grid = new \\Smrtr\\DataGrid(array(
      array('blue', 'planet', 'baz'),
      array('hello', 'planet', 'earth'),
      array('blue', 'red')
    ));
    
    print_r($grid->getColumn(0));
    // [ 0=>"blue", 1=>"hello", 2=>"blue" ]
    
    print_r($grid->getColumn(2));
    // [ 0=>"baz", 1=>"earth", 2=>null ]

#### getColumnDistinct( `$keyOrLabel` )
The same as `getColumn()` except that repeated values and null values are removed from the returned array.

*As such, the returned array is of variable length.*

    $grid = new \\Smrtr\\DataGrid(array(
      array('blue', 'planet', 'baz'),
      array('hello', 'planet', 'earth'),
      array('blue', 'red')
    ));
    
    print_r($grid->getColumnDistinct(1));
    // [ 0=>"planet", 1=>"red" ]
    
    print_r($grid->getColumnDistinct(2));
    // [ 0=>"baz", 1=>"earth" ]

### Getting grid data

#### getArray()
Returns a 2D array of all grid data. Each sub-array represents a row.

    <?php
    $grid = new \\Smrtr\\DataGrid(array(
      array('foo', 'bar', 'baz'),
      array('first', 'second')
    ));
    
    print_r($grid->getArray());
    /**
     * 0 => [ 0=>"foo", 1=>"bar", 2=>"baz" ],
     * 1 => [ 0=>"first", 1=>"second", 2=>null ]
     */

#### getAssociativeArray()
Returns a 2D array of all grid data, indexed by labels (where available). Each sub-array represents a row.

*Rows/columns without a unique label are simply indexed by key instead.*

    <?php
    $grid = new \\Smrtr\\DataGrid(array(
      array('foo', 'bar', 'baz'),
      array('first', 'second')
    ));
    $grid->rowLabels(array('row1', 'row2'));
    $grid->columnLabels(array('a', 'b', null));
    
    print_r($grid->getAssociativeArray());
    /**
     * "row1" => [ "a"=>"foo", "b"=>"bar", 2=>"baz" ],
     * "row2" => [ "a"=>"first", "b"=>"second", 2=>null ]
     */

[Next: Data Setters](data-setters.md)