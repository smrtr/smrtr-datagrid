## Vectors

[Previous: Data Setters](data-setters.md)

Smrtr DataGrid comes packaged with a second class, `\Smrtr\DataGridVector`,
which provides proxies to specific rows or columns on `\Smrtr\DataGrid` objects.

Vector objects come in two flavours: **row** vectors and **column** vectors.

> **Heads up!** Vector objects are linked to rows/columns via their keys, *not via their values*.

### Getting a vector object
The following two methods can be called on any `\Smrtr\DataGrid` object.

#### row( `$keyOrlabel` )
Accepts a key (int) or a label (string) from the `$keyOrLabel` parameter,
and returns a new vector object linked to the identified row.

    <?php
    $firstRow = $grid->row(0);

*The returned object is a `\Smrtr\DataGridVector` object and is linked to the grid upon which
the `row()` method was called.*

#### column( `$keyOrlabel` )
Accepts a key (int) or a label (string) from the `$keyOrLabel` parameter,
and returns a new vector object linked to the identified column.

    <?php
    $someColumn = $grid->column('someColumn');

*The returned object is a `\Smrtr\DataGridVector` object and is linked to the grid upon which
the `column()` method was called.*

### Available vector methods
The following methods are available on all `\Smrtr\DataGridVector` objects.

#### data( `$labelled=false` )
Returns an array of values from the linked row/column. Set `$labelled` to `true` to return an associative array.

    <?php
    $grid = new \Smrtr\DataGrid(array(
      array('smrtr', 'data', 'grid'),
      array('data', 'grid', 'vector')
    ));
    $grid->columnLabels(array('c1', 'c2', 'c3'));
    
    print_r($grid->column(0)->data());
    // [ 0=>'smrtr', 1=>'data' ]
    
    print_r($grid->row(1)->data(true));
    // [ 'c1'=>'data', 'c2'=>'grid', 'c3'=>'vector' ]

#### getDistinct()
Returns an array of distinct values from the linked row/column.

    <?php
    $grid = new \Smrtr\DataGrid(array(
      array(1, 2, 2, 1, 4, 7, 3, 4, 6)
    ));
    print_r($grid->row(0)->getDistinct());
    // [ 1, 2, 4, 7, 3, 6 ]

#### label( `$label=false` )
Get or set the label for this row/column. 

###### Setting:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array('1a', '2a', '3a'),
      array('1b', '2b', '3b'),
      array('1c', '2c', '3c')
    ));
    $grid->column(1)->label('middle column');
    
    print_r($grid->getLabel('column', 1));
    // 'middle column'

###### Getting:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array('x', 'y', 'z')
    ));
    $grid->updateLabel('column', 0, 'first column');
    
    print_r($grid->column(0)->label());
    // 'first column'

### Array access
Part of the reason behind developing the `\Smrtr\DataGridVector` class has been to provide an ArrayAccess interface
for rows and columns on the grid which accepts keys and labels.

For the developer these enhancements become especially useful in PHP version 5.4 or later,
where the array access can be chained directly onto the method that invokes the vector object.

#### Getting values
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array('blue', 'skies', 'encourage', 'smiles'),
      array('green', 'houses', 'trap', 'rays'),
      array('red', 'herrings', 'mislead', 'people')
    ));
    $grid->columnLabels(array(
      'colour', 'plural noun #1', 'verb', 'plural noun #2'
    ));
    
###### >= PHP 5.4
    <?php
    echo $grid->column('colour')[1];
    // "green"
    
    echo $grid->row(1)['plural noun #1'];
    // "houses"
    
###### < PHP 5.4
    <?php
    $vector = $grid->row(2);
    echo $vector[3];
    // "people"
    
    $vector = $grid->column('verb');
    echo $vector[0];
    // "encourage"

#### Setting values
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array(null, null),
      array(null, null)
    ));
    $grid->rowLabels(array('row0', 'row1'));
    $grid->columnLabels(array('col0', 'col1'));

###### >= PHP 5.4
    <?php
    $grid->row('row0')[1] = "top right";
    echo $grid->getValue('row0', 'col1');
    // "top right"
    
    $grid->column(0)['row1'] = "bottom left";
    echo $grid->getValue(1, 'col0');
    // "bottom left"

###### < PHP 5.4
    <?php
    $vector = $grid->row('row0');
    $vector['col0'] = "top left";
    echo $grid->getValue(0, 0);
    // "top left"
    
    $vector = $grid->column(1);
    $vector[1] = "bottom right";
    echo $grid->getValue('row1', 1);
    // "bottom right"

#### Unsetting values

###### >= PHP 5.4
    <?php
    unset($grid->row(0)[0]);
    print_r($grid->getValue(0, 0));
    // NULL

###### < PHP 5.4
    <?php
    $vector = $grid->row('row0');
    unset($vector['col0']);
    print_r($grid->getValue('row0', 'col0');
    // NULL

### Iteration over a vector object
`\Smrtr\DataGridVector` objects also implement the Iterator interface,
meaning that they can be looped over with foreach statements.

    <?php
    $grid = new \Smrtr\DataGrid(array(
      array(3.7, 4.2, 1.4)
    ));
    foreach ($grid->row(0) as $key => $val) {
      echo $key.", ";
      $result += $val;
    }
    echo $result;
    // "0, 1, 2, 9.3"

[Next: Iterating & Filtering](iterating-filtering.md)