=== "Attributes"

    The configuration metadata are returned from the `attributes` script below. Please see the [Question Types](/fc_doc/Components/Form/Section/Question/overview/#question-types) for more information on the attributes. 
    
    !!! note
        When configuring the attributes, it is **recommended** to use the Form Builder UI. This script is for advanced use cases where we need the attributes to be dynamically.

    ```javascript linenums="1"
        (function (current, table, target_id, task_id, params) {

        var attributes = new FCQuestionRepository().getAttributes(current, table, target_id, task_id, params);
        return attributes;
        
    })(current, table, target_id, task_id, params);
    ```
