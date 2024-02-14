# Composibles

## useForm
`useForm` in qnatk composibles are use to create a form  with validation and handle form submission. By its use you can submit the form data to the model. It also provides api which first validate and insert the form data into to the database table.

This is the code snippet to use  `useForm` in your vue 3 project:
```javascript
useForm(Api, initialUrl, defaultValues, METHOD);
```
`useForm`  takes four parameters. The parameters are as follows:

1. **Api** - The API object that will be used for fetching data from the server. 
1. **initialUrl** -  This parameter represents the URL of the page you want to load initially.
1. **defaultValues** -  This parameter representing an object containing key/value pairs corresponding to form fields and their default values. If this value is not provided then it defaults to an empty string. 
1. **METHOD** -  It specifies whether it's a GET or POST request. You can set this value to either.  `GET` or `POST`. If not provided, it defaults to `POST`


After calling `useForm` function , it returns an object with following properties:

1. **values** -  An object containing all form fields' values. It gets updated whenever a field value changes. It is a ref variable
1. **errors** -  An empty variable which contains all validation errors and form errors. If there's any error then this property will throw that error and execution will stop. It is a ref variable
1. **isLoading** -  A boolean value indicating whether a request is being made or not. It's set to true when the validation of form executed perfectly. It is a ref variable
1. **updateUrl** -  A method which updates the current url and triggers a new request to the server using the provided api. This function is executed when the update query is used to update the data in the database. It is a ref variable
1. **validateAndSubmit** -  A method which validates all fields and then submits the form if validation passes.
1. **validateResponse** - Its a helper function that validate the data based on DTOClass. If validation fails then throws error otherwise it will return formate response.
1. **responseData** - It is a ref variable. This property holds the response data returned by the API call.
1. **callbacks** -  An array of objects containing callback functions which can be executed when a specific event occurs (e.g., onSuccess, onError and beforeSubmit). It is a reactive variable.
1. **resetForm** -  A method which resets all fields to their original values and clears all filled values.

## use-datatables
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

1. **callBacks** - 
1. **fetchOptions** -
1. **data** -
1. **responseData** -
1. **actions** -
1. **pagination** -
1. **loading** -
1. **error** -
1. **fetchData** -
1. **onRequest** -
1. **closeDialogAndReload** -
1. **downloadData** -
1. **lacHookName** -