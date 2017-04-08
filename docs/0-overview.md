# Overview

Smrtr DataGrid is a one-stop-shop for handling & manipulating tabular data between formats.
It's essentially a tool for working with 2d arrays, spreadsheets and data grids in PHP.

 - CSVs
 - JSON
 - Search
 - Transformations
 - Fluent API
 - Iteration & Filtering

The API is intuitive and easy to learn.

The search functionality uses a schema inspired by jQuery and lets you find data as easily as
`$Grid->findRows('name*=Joe')`.

## Using DataGrid

##### RapidFrames

> Central to RapidFrames' architecture is it's "CSV as Database". 
> 
> Developers are able to use a single columned csv to
> create nodes that form entire tree structures for pages, menus, you name it.
 
To extend this notion of a "CSV as Database" Smrtr DataGrid comes in to allow RapidFrames' API to query the project
for responses such as:

 - Get me all the children/descendants of this page
 - Get me all the pages at these different depths
 - Get me the nested sitemap, get me a non nested sitemap
 - Get me details of another csv that is in this project.

These results are useful for creating page trees, side navs, and even a rudimentary log in system.

Smrtr DataGrid provides a really intuitive API to allow RapidFrames to build questions that can be answered by
Smrtr DataGrid in a rapid way!