Passing options into the widget. This is useful when you want to pass data to the widget server script in order to customize the widget.

In this example, we will follow up with the previos example [Create and embed a new Widget](example.md) and pass options into the widget.

### Page Configuration

In the page configuration, we will pass the options into the `fc-widget` tag.

=== "Client Script"

    ```javascript linenums="1" hl_lines="9-11"
    function execute(helpers){
        async function closeIncident(incidentId){
            
            await helpers.service.call('closeIncident', [incidentId]);
            helpers.service.snackbar("Incident closed successfully!");
        
        }

        function getPageSize(){
            return 15;
        }

        return {
            closeIncident: closeIncident,
            getPageSize: getPageSize
        };
    }
    ```

=== "HTML Template"

    In line 19-21, we are passing the `page_size` option into the widget via the `:options` attribute. value must be a JSON string. We are also passing the getPageSize function defined in the client script as a value to the `page_size` property.

    ```html linenums="1" hl_lines="19-21"
    <div class="container main-container mt-5">
        <div class="row">
            <div class="col p-3">
                <div>
                    <span x-text="$scope.model.incident.number"></span>
                </div>
                <div>
                    <span x-text="$scope.model.incident.short_description"></span>
                </div>
                <div>
                    <mdui-button @click="$scope.closeIncident($scope.model.incident.sys_id)" variant="filled">
                        Close Incident
                    </mdui-button>
                </div>
            </div>
        </div>
        <div class="row">
            <div class="col">
                <fc-widget widget-id="priority_incident_widget" :has-options="true"
                    :options='JSON.stringify({ page_size: $scope.getPageSize()})'>
                </fc-widget>
            </div>
        </div>
    </div>
    ```



### Widget Configuration

Now that we setup the options to be passed into the widget from the page, we need to setup the widget to accept the options.

=== "Server Script"

    As you can see on line 11, we are passing the `page_size` option into the widget. This is the number of incidents that will be displayed in the widget.

    ```javascript linenums="1" hl_lines="11"
    (function (params) {

        function initialize() {
            var model = {
                condition: true,
                priority_incidents: []
            };

            var priorityIncidentsGr = new GlideRecord('incident');
            priorityIncidentsGr.addEncodedQuery('active=true^priority=1');
            priorityIncidentsGr.setLimit(params.page_size);
            priorityIncidentsGr.query();
            
            while(priorityIncidentsGr.next()){
                model.priority_incidents.push({
                    sys_id: priorityIncidentsGr.getUniqueValue(),
                    number: priorityIncidentsGr.getValue('number'),
                    short_description: priorityIncidentsGr.getValue('short_description')
                });
            }
            return model;
        }


        return {
            initialize: initialize
        };

    })(params);
    ```
