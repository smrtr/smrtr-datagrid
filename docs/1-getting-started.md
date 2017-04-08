## Getting Started

#### Download DataGrid
[Download the master zip from github](https://github.com/joegreen88/smrtr-datagrid/archive/master.zip)

All of the code is version controlled under git and hosted at github, which you can
[browse online](https://github.com/joegreen88/smrtr-datagrid).
This download includes the source files and the unit tests. 

*Note: The contents of the `tests` folder are not required for DataGrid to work.*

#### Or Build from Git
Alternatively, if you have access to git, you can connect to the repository here:

    git clone git://github.com/smrtr/smrtr-datagrid.git

#### Or Install with Composer
The best way to use Smrtr DataGrid in your application is to define it as a dependency with
[composer](http://getcomposer.org/).

##### `composer.json`
    {
        \"require\": {
            \"smrtr/datagrid\": \">=1.3.2\"
        }
    }

Run `composer install` or `composer update --prefer-source` to have composer download the
Smrtr DataGrid files and generate an `autoload.php`.

#### Include the Class
To use DataGrid you just need to include one file in your PHP script:

    <?php
    require_once 'smrtr-datagrid/DataGrid.php';
    ?>

or, if installed with composer:

    <?php
    require_once 'vendor/autoload.php';
    ?>

Then instantiate the class:

    <?php
    $Grid = new \\Smrtr\\DataGrid;
    ?>

### Test Data
To help you get started DataGrid comes bundled with a 500 row CSV file inside `tests/application/input` called
`directgov_external_search_2012-02-05.csv`.

To quickly have a play with it you can load the test data into a Smrtr Datagrid instance like so:

    <?php
    $Grid = new \\Smrtr\\DataGrid;
    $Grid->loadCSV('smrtr-datagrid/tests/application/input/directgov_external_search_2012-02-05.csv', true, true);
    ?>
