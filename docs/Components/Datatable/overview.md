## Overview

The datatable component has two main components:

1. The backend support for the datatable which provide metadata and data to the datatable. The metadata includes the columns, the data type of each column, and the filter options for each column as well as the GraphQL query to fetch the data.
    * Name: The name of the datatable. This is used to identify the datatable in the backend. This must be unique as it is used to fetch the data from the backend.
    * Schema: The GraphQL schema of the datatable.
        * Custom: Selecting custom will use the custom schema.
        * OOTB: Selecting OOTB will use the OOTB schema such as GlideRecord_Query
    * Filter Builder: This is a javascript function that will evaluate to return a ServiceNow encoded query string.
        * There two parameters to this function:
            * `current`: This is the current GraphQL Query Broker GlideRecord object.
            * `params`: This is the parameters object coming from the URL query string on the frontend.
    * GQL: The GraphQL query to fetch the data from the backend.
    * Script: The javascript function to define the metadata of the datatable.
        *    function:
            * `current`: This is the current GraphQL Query Broker GlideRecord object.
            * `params`: This is the parameters object coming from the URL query string on the frontend.
    * Record Script: Record Script: This is the client side javascript function that will support actions from the individual field of each of the record when the datatable is rendered.
        * This function has two parameters:
            * `record`: This is the record object.
            * `service`: This is the client side service used to interact with the backend.
                * The service provides the following methods:
                    * `call(methodName, parameters, pageId)`: Call a server-side method
                    * `search(query)`: Update URL query parameters
                    * `navigateToPage(pageId, queryParams)`: Navigate to another page
                    * `navigate(url)`: Navigate to a URL
                    * `snackbar(message)`: Show a snackbar message
                    * `showLoading()`: Show loading indicator
                    * `hideLoading()`: Hide loading indicator 
                    * `confirm(message)`: Show a confirmation dialog
                    * `postAsync(url, payload)`: Make a POST request
                    * `getAsync(url)`: Make a GET request
                    * `graphQlAsync(query)`: Execute a GraphQL query
2. The datatable filter component

