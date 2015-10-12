#ractive-datatable


### Demo

[Live Demo](http://jondum.github.com/ractive-datatable/demo/)

### Install

```
npm install ractive-datatable --save
```

### Features

* Minimal (3.45kb gzipped **including** styles, 1.71kb without)
* Well designed default styling
* Cell Editing
* Pagination (incl. navigation buttons)
* Filtering (with sexy highlights)
* Sorting
* Per-cell partials (new!)
* Selection (new!) (wip!)

### Usage

Add the datatable to your Ractive instance:

```
Ractive.extend({
    ...
    components: {
        datatable: require('ractive-datatable')
    },
    ...
});
```

Use it

```
<datatable data='{{data}}' on-edit='dataedited' config='{{config}}' filter='{{filter}}'></datatable>
```

Includes minimal styling under the class `.ractive-datatable`. Styles are included in the javascript and added to the page on load. If you don't want these styles in the javascript, `require()` `src/datatable.js` and handle the styles as needed.

To use a specific partial for a cell, create an inline partial expression inside the tag of the component:

```
<datatable data='{{data}}' on-edit='dataedited' config='{{config}}' filter='{{filter}}'>
    {{#partial timestamp}}
        {{ moment(this).fromNow() }}
    {{/partial}}
</datatable>
```

Will render every row in the "timestamp" column with the passed in partial (in this case formatting the timestamp using moment.js).

### API

##### Properties

`data`: Array of Objects where each key is a column

`editable`: globally allow/disallow editing

`filter`: A string to filter the rows on. Searches through all cells with a case-insensitive RegEx and displays only rows that match. Cells with matches are highlighted.

`columns`: Array specifying which columns to display and order (any keys missing in this array will not be shown). If left `undefined`, the datatable will extract columns from the first item in the data array.

`config`: Object to configure individual columns. Each key represents a column and must be an object with optional `edit` and `display` keys.

`selectionMode`: Either `row` or `cell` (WIP). Allows for rows to be selected on click

`selection`: An array of the currently selected objects from `data`.

```
config: {
    'created': {edit: false},
    'id': {edit: false},
    'hiddenField': {display: false},
    'someOtherColumn': {edit: false}  
}
```

`page`: the current page

`lastPage` (readonly): the last page or total number of pages

##### Methods


`previousPage`: Go to the previous page

`nextPage`: Go to the next page


##### Events

`edit`: Dispatched when an edit is made. Sends the entire row and the field that is edited (useful for extracting specific information from the row that changed).

```
this.on('dataedited', function(row, field) {
    
    var change = {};
    change.id = row.id;
    change[field] = row[field];
    
    changes.push(change);
    
});
```

For other events you probably would be better off using Ractive's observers on your datatable instance and the property you want.

Open to PRs and stuff. I'm around.


