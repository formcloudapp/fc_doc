The Page component is similar to a ServiceNow widget, and like a widget, it has four main components:

=== "Server Script"

    The **"Server Script"** This is the server side script that will provide the data to the page.

    ```javascript
    (function (params) {

        function initialize() {
            let model = {
                condition: true
            };
            return model;
        }

        return {
            initialize: initialize,
        };

    })(params);
    ```

=== "Client Script"

    The **"Client Script"** This is the client side script that will provide the logic to the page.
    
    ```javascript
    function execute(helpers){
        function myFunction(){
            return 'Hello World!';
        }

        return {
            myFunction: myFunction
        };
    }
    ```

=== "Template"

    The **"Template"** This is the HTML template that will be used to display the page. The template support <a href="https://alpinejs.dev/" target="_blank">AlpineJS</a> for dynamic content and reactivity.

=== "CSS"

    The **"CSS"** This is the CSS code that will be used to style the page. The css is encapsulated in the page component and will not be applied to other pages.
