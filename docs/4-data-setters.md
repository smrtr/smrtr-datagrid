## Data setters
These methods allow you to amend the data stored in the datagrid object. This data can be easily retrieved later. 

 > *Only scalar typed values may be assigned to a datagrid by default.*
 >
 > To enable non-scalar values like objects and arrays to be set onto the grid, first call 
 > `$grid->scalarValuesOnly(false)` to enable non-scalar values.

### Setting an individual data point

#### setValue( `$rowKeyOrLabel`, `$columnKeyOrLabel`, `$value` )
The `$rowKeyOrLabel` and `$columnKeyOrLabel` parameters each expect a key (int) or a label (string),
which are used to find the co-ordinates of an existing data point on the grid and write `$value` to its position.

### Setting a row

#### updateRow( `$keyOrLabel`, `$row` )
Overwrite an existing row identified by `$keyOrLabel` with the first `n` entries of the array `$row`,
where `n` is the width of the grid.

*Smaller rows are automatically padded with null values to match the size of the grid.*

#### appendRow( `$row`, `$label=null` )
Creates a new row with the first `n` entries of the array `$row`, where `n` is the width of the grid,
and appends it to the end of the datagrid.

*Smaller rows are automatically padded with null values to match the size of the grid.*

The key for the new row is auto-generated, you may optionally give a label with the `$label` parameter.

#### prependRow( `$row`, `$label=null` )
Creates a new row with the first `n` entries of the array `$row`, where `n` is the width of the grid,
and prepends it to the start of the datagrid.

*Smaller rows are automatically padded with null values to match the size of the grid.*

The key for the new row is always going to be 0, you may optionally give a label with the `$label` parameter.
The keys for the rest of the datagrid are automatically adjusted.

### Setting columns

#### updateColumn( `$keyOrLabel`, `$column` )
Overwrite an existing column identified by `$keyOrLabel` with the first `n` entries of the array `$column`,
where `n` is the height of the grid.

*Smaller columns are automatically padded with null values to match the size of the grid.*

#### appendColumn( `$column`, `$label=null` )
Creates a new column with the first `n` entries of the array `$column`, where `n` is the height of the grid,
and appends it to the end of the datagrid.

*Smaller columns are automatically padded with null values to match the size of the grid.*

The key for the new column is auto-generated, you may optionally give a label with the `$label` parameter.

#### prependColumn( `$column`, `$label=null` )
Creates a new column with the first `n` entries of the array `$column`, where `n` is the height of the grid,
and prepends it to the start of the datagrid.

*Smaller columns are automatically padded with null values to match the size of the grid.*

The key for the new column is always going to be 0, you may optionally give a label with the `$label` parameter.
The keys for the rest of the datagrid are automatically adjusted.

### Setting the entire grid

#### loadArray( `$data` )
Imports a 2D array `$data` into the grid. Clears all previously loaded data.

> **See also:</strong> loadCSV(), loadJSON(), readCSV(), readJSON()**
