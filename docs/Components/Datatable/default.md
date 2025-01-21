!!! note "Default Configuration"
    When creating a new datatable, the following code and configurations is by default.
    To create a new datatable, you can use the datatable builder in the FC UI. However, you can view the code and configurations in the ServiceNow backend by going to this url: `https://<your-instance>.service-now.com/nav_to.do?uri=x_509925_fc_graphql_query_broker.do`. **Keep in mind that it is not recommended to edit the code in the ServiceNow backend** unless you are absolutely sure about what you are doing. The recommended way to configure the datatable is to use the datatable builder in the FC UI.

=== "Filter Builder"

    ```javascript
    ((current, params) => {

        return '';

    })(current, params);
    ```

=== "Script"

    The script below configures the datatable by setting up essential properties and configurations:

    ### Parameters
    - `current`: The current GraphQL Query Broker GlideRecord object.
    - `params`: URL parameters passed to the component
    
    ### Configuration Setup
    - **GraphQL Namespace**: Retrieves the namespace from system properties using the scope
    - **Schema Reference**: Obtains the GraphQL schema record for further configuration
    
    ### Datatable Configuration Object Properties
    - **table**: The name of the table to query data from
    - **filters**: Applied filters for data retrieval
    - **allow_selection**: Controls if rows can be selected in the datatable
    - **gql**: The GraphQL query string for data fetching
    - **details_template**: Template for displaying record details
    
    ### GraphQL Path Configuration
    - For OOTB (Out of the Box) schema:
        - Results Path: `GlideRecord_Query.[table]._results`
        - Row Count Path: `GlideRecord_Query.[table]._rowCount`
    - For Custom schema:
        - Results Path: `[namespace].[schema_namespace].getMultiple`. This is the path to query results in the GraphQL response.
        - Row Count Path: `[namespace].[schema_namespace].getCount`. This is the path to get total row count to use for pagination in GraphQL response.
    
    ### Additional Settings
    - **data_columns**: JSON parsed column configurations defining the structure
    - **schema**: Type of schema being used (OOTB or custom)
    - **gql_schema**: The namespace of the GraphQL schema

    ```javascript title="Server Script"
    ((current, params) => {
        const namespace = gs.getProperty(current.sys_scope.scope + '.graphql_namespace');
        const gqlSchemaGr = current.graphql_schema.getRefRecord();
        let datatableConfig = {
            table: current.getValue('table'),
            filters: current.getValue('filters'),
            allow_selection: current.getValue('allow_selection'),
            gql: current.getValue('gql'),
            details_template: current.getValue('record_details'),
            gql_path: current.getValue('schema') == 'ootb' ? ('GlideRecord_Query.' + current.getValue('table') + '._results') : (namespace + '.' + gqlSchemaGr.getValue('namespace')+ '.getMultiple'),
            row_count_path: current.getValue('schema') == 'ootb' ? ('GlideRecord_Query.' + current.getValue('table') + '._rowCount') : (namespace + '.' + gqlSchemaGr.getValue('namespace')+ '.getCount'),
            data_columns: JSON.parse(current.getValue('columns_configs')),
            schema: current.getValue('schema'),
            gql_schema: gqlSchemaGr.getValue('namespace')
        };

        return datatableConfig;

    })(current, params);
    ```

=== "GQL"

    The GQL query is the GraphQL query that will be used to fetch the data from the backend.

    The GQL query builder function takes two parameters:

    * `params`: URL parameters passed from the frontend
    * `configs`: Configuration object returned from the script section above, containing:
        * `schema`: Type of schema ('ootb' or 'custom')
        * `table`: Table name to query
        * `filters`: Query conditions/filters
        * `offset`: Pagination offset
        * `limit`: Pagination limit
        * `gql`: Fields to retrieve in the query
        * `namespace`: GraphQL namespace for custom schemas
        * `gql_schema`: Schema namespace

    For OOTB schemas, it builds a query using the GlideRecord_Query structure.
    For custom schemas, it builds a query with the custom namespace and schema, including sys_id and specified fields.

    ```javascript title="Client Script"
        function execute(params, configs) {
        let gql = '';
        if (configs.schema == 'ootb') {
            gql = `
            {
                GlideRecord_Query{
                    ${configs.table}(queryConditions: "${configs.filters}", pagination: { offset: ${configs.offset}, limit: ${configs.limit} }) {
                        _rowCount
                        _results ${configs.gql}
                    }
                }
            }
        `;
        }else{
            gql = `
            {
                ${configs.namespace}{
                    ${configs.gql_schema}{
                        getMultiple(tableName: "${configs.table}", queryConditions: "${configs.filters}", pagination: { offset: ${configs.offset}, limit: ${configs.limit} })
                        {
                            sys_id{
                                value
                            }
                            ...on ${configs.namespace}_${configs.gql_schema}_${configs.table}  ${configs.gql}
                        }
                        getCount(tableName: "${configs.table}", queryConditions: "${configs.filters}")
                    }
                }
            }
            `;
        }

        return gql;
    }
    ```

=== "Columns Configs"

    This field by default is an empty array. This is where you can define the columns of the datatable. When you add a new column from the datatable builder, it will be added to this array.

=== "Record Script"

    * Record Script: This is the client side javascript function that will support actions from the individual field of each of the record when the datatable is rendered.
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

    ```javascript title="Client Script"
    function execute(record, service) {

        return {
            record: record
        }
    }
    ```