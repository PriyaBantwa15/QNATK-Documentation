# Composibles

## useForm
`useForm` in qnatk composibles are use to create a form  with validation and handle form submission. By its use you can submit the form data to the model. It also provides api which first validate and insert the form data into to the database table.

This is the code snippet to use  `useForm` in your vue 3 project:
```javascript
useForm(Api, initialUrl, defaultValues, METHOD);
```
`useForm`  takes four parameters.The parameters are as follows:

1. **Api** - The API object that will be used for fetching data from the server. 
1. **initialUrl** -  This parameter represents the URL of the page you want to load initially.
1. **defaultValues** -  This parameter representing an object containing key/value pairs corresponding to form fields and their default values. If this value is not provided then it defaults to an empty string. 
1. **METHOD** -  It specifies whether it's a GET or POST request. You can set this value to either.  `GET` or `POST`. If not provided, it defaults to `POST`

## use-datatables
 QNATK is a RAD framework extension for quasar and nest + sequelize.
 The main purpose of QNATK is to reduce the repeatative requirements like error handling , input validation and model mapping when developing web applications.

 The another main purpose is code maintainability  and reusability, so we can reuse components in different projects with minimal changes.
