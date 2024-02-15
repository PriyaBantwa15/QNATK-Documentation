## useDatatable
 `useDatatable` in qnatk use to fetch the data from database table and display that data in the web page. It displays the data in perfect manner like how much data will show at a time and also provide the pagination like if  the table contains the 20 rows so while using `useDatatable` it will print first 10 rows and rest will print in next page. I also sorts the data accordingly. it also handles the error which have been occurred while fetching the data from the table.

This is the code snippet to use  `useDatatable` in your vue 3 project:
```javascript
useDatatable(Api, baseModel, baseUrl, transformSortBy);
```
`useDatatable`  takes four parameters. The parameters are as follows:

1. **Api** - The API object that will be used for fetching data from the server.
1. **baseModel** - baseModel is a variable that stores the model name(Table name) which is used to fetch the data from provided model name passed to baseModel.
1. **baseUrl** -  This parameter represents the Base Url for making request to the server.
1. **transformSortBy** - Its a function which sorts the  column based on sort key provided.

After calling `useDatatable` function , it returns an object with following properties:

1. **callBacks** - It is an object  containing callback functions like `rowIterator`(stores the row name), `downloadRowIterator` (stores the row data) and `aclCan`(stores the `actionName` and `BaseModel`),
1. **fetchOptions** -  An object containing options for fetching data from the database. You can pass this option directly into Vue
1. **data** -  Represents the table's data source.
1. **responseData** - It is a ref variable. This property holds the response data returned by the API call.
1. **actions** -  An array containing actions like Add(add new row), Edit(edit the already exist data), Delete(to delete the particular row) and many more which will add in the backend.,
1. **pagination** -  An object containing pagination information like sortBy, descending, page, rowsPerPage, rowsNumber etc.
1. **loading** -  A boolean value indicating whether the HTTP request is in progress or not.
1. **error** - An object containing error information, if any error occurred during the HTTP request.
1. **fetchData** - An object containing values which were fetched from table. You can pass this option directly into Vue
1. **onRequest** -
1. **closeDialogAndReload** -  A method to close add or edit form dialog and reload data of the table.
1. **downloadData** -  A method to download the data in CSV format.
1. **lacHookName** -    Hook name to execute before loading the data.