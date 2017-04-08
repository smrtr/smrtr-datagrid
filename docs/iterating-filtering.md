## Iterating & Filtering

[Previous: Vectors](vectors.md)

Smrtr DataGrid comes with some methods to help you iterate over the grid and also perform filtering.
These methods are essentially helpers that use callbacks, making it very easy to act on the grid data.

> **$callback:** The following methods accept a `$callback` parameter.
> You can provide an anonymous function, a named function or a method so long as the parameter is callable
> according to [is_callable](http://php.net/manual/en/function.is-callable.php).

### Iterating over the grid
There are two ways to iterate over the grid: row-wise or column-wise. Methods are provided for both.

#### eachRow( `$callback`, `$returnVectorObject = false` )
Iterates over the grid's rows and executes the callback function on each row.

The key *(int)*, the label *(null or string)* and the row data are passed to the callback function:

##### $callback( `$key`, `$label`, `$row` )
The `$row` parameter is passed as an array by default.

###### Example:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array(1, 4, 7),
      array(2, 5, 8),
      array(3, 6, 9)
    ));
    $rowSums = array();
    $grid->eachRow(function($key, $label, $row) use (&$rowSums) {
      $rowSums[] = $row[0] + $row[1] + $row[2];
    });
    print_r($rowSums);
    // [ 12, 15, 18 ]

Set `$returnVectorObject` to true to pass `$row` as a `\Smrtr\DataGridVector` instance.

###### Example using `\Smrtr\DataGridVector` objects:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array(1, 4, 7),
      array(2, 5, 8),
      array(3, 6, 9)
    ));
    $grid->rowLabels(array("r0", "r1", "r2"));
    $grid->columnLabels(array("A", "B", "C"));
    $grid->eachRow(function($key, $label, $row) {
      $rowSum = $row['A'] + $row['B'] + $row['C'];
      echo PHP_EOL."$label total: ".$rowSum;
    }, true);
    // r0 total: 12
    // r1 total: 15
    // r2 total: 18

#### eachColumn( `$callback`, `$returnVectorObject=false` )
Iterates over the grid's columns and executes the callback function on each column.

The key *(int)*, the label *(null or string)* and the column data are passed to the callback function:

##### $callback( `$key`, `$label`, `$column` )
The `$column` parameter is passed as an array by default.

###### Example:
    <?php
    $grid = new Smrtr_DataGrid(array(
      array(1, 4, 7),
      array(2, 5, 8),
      array(3, 6, 9)
    ));
    $columnSums = array();
    $grid->eachColumn(function($key, $label, $col) use (&$columnSums) {
      $columnSums[] = $col[0] + $col[1] + $col[2];
    });
    print_r($columnSums);
    // [ 6, 15, 24 ]

Set `$returnVectorObject` to true to pass `$column` as a `\Smrtr\DataGridVector` instance.

###### Example using `\Smrtr\DataGridVector` objects:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array(1, 4, 7),
      array(2, 5, 8),
      array(3, 6, 9)
    ));
    $grid->rowLabels(array("r0", "r1", "r2"));
    $grid->columnLabels(array("A", "B", "C"));
    $grid->eachColumn(function($key, $label, $c) {
      $columnSum = $c['r0'] + $c['r1'] + $c['r2'];
      echo PHP_EOL."$label total: ".$columnSum;
    }, true);
    // A total: 6
    // B total: 15
    // C total: 24

### Filtering the grid
The filter functions accept a callback which this determines which vectors to keep and which vectors to discard
from the result. The return value of the callback function is evaluated to true or false.
Returning true keeps the vector while returning false discards it.

#### filterRows( `$callback`, `$returnVectorObject = false` )
Iterates over the grid's rows and executes the callback function on each row.
The return value of the callback function determines whether to keep the row (return true) or discard it (return false).

The key *(int)*, the label *(null or string)* and the row data are passed to the callback function:

##### $callback( `$key`, `$label`, `$row` )
The `$row` parameter is passed as an array by default.

###### Example:
    <?php
    function row_sum_is_odd( $key, $label, $vector ) {
      $total = 0;
      foreach ($vector as $x)
        $total += $x;
      return $total % 2;
    }
    $grid = new \Smrtr\DataGrid(array(
      array(2, 4, 3),
      array(3, 5, 8),
      array(3, 6, 6)
    ));
    print_r($grid->filterRows('row_sum_is_odd')->getRowKeys());
    // [ 0, 2 ]

Set `$returnVectorObject` to true to pass `$row` as a `\Smrtr\DataGridVector` instance.

###### Example using `\Smrtr\DataGridVector` objects:
    <?php
    function vals_are_distinct($key, $label, $vector) {
      return count($vector->getDistinct()) == $vector->grid()->info('rows');
    }
    $grid = new \Smrtr\DataGrid(array(
      array(23, 23, "34"),
      array("foo", 53, 38),
      array(377, 64, "0x98")
    ));
    print_r($grid->filterRows('vals_are_distinct', true)->getRowKeys());
    // [ 1, 2 ]

#### filterColumns( `$callback`, `$returnVectorObject = false` )
Iterates over the grid's columns and executes the callback function on each column.
The return value of the callback function determines whether to keep the column (return true) or discard it
(return false).

The key *(int)*, the label *(null or string)* and the column data are passed to the callback function:

##### $callback( `$key`, `$label`, `$column` )
The `$column` parameter is passed as an array by default.

###### Example:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array(21, 92, 3),
      array(65, 5, 88),
      array(10, 12, 6)
    ));
    echo "cols: " . $grid->filterColumns(function($key, $label, $column) {
      return false;
    })->info('columns');
    // "cols: 0"

Set `$returnVectorObject` to true to pass `$column` as a `\Smrtr\DataGridVector` instance.

###### Example using `\Smrtr\DataGridVector` objects:
    <?php
    $grid = new \Smrtr\DataGrid(array(
      array("john", "john", "jon"),
      array("wayne", "barnes", "tickle")
    ));
    $result = $grid->filterColumns(function($k, $l, $c) {
      return in_array("john", $c);
    }, true);
    print_r($result->getArray());
    // [ [ "john", "john" ], [ "wayne", "barnes" ] ]

[Next: Searching](searching.md)