# Welcome to FC Documentation

### Getting Started

Please follow the instructions below in sequential order:

* Download: <a href="https://github.com/formcloudapp/fc_doc/blob/main/docs/FC_01.zip" download>FC Server Application</a>
* Download: <a href="https://github.com/formcloudapp/fc_doc/blob/main/docs/FC_01_Client.zip" download>FC Client Application</a>
* Go to Retrieved Update Sets and Import XML to load the "FC Server Application" update set.
* Preview update set and commit.
* Go to `https://[your_instance]/sys_ws_operation.do?sys_id=ed18366c975746d0e12c3eb0f053afce`
* Set your Application Scope to FC and upload all files from the FC_Client_Application.zip as attachments on this record.
* When working within the application, we want to set the current application scope to FC during insert, update, or delete operations, due to the siloed nature of the design. However, given the limitation that the application scope can only be set from the global scope, we need to create a script include in the global scope that is callable from all application scopes. Please create a new script include as outlined below:
    * Name: FCGlobalUtility
    * API Name: global.FCGlobalUtility
    * Application: Global
    * Accessible from: All Application Scope
    * Script: 
        ```javascript
        var FCGlobalUtility = Class.create();
        FCGlobalUtility.prototype = {
            initialize: function() {},
            setApplicationScopeById: function(scopeId) {
                if (gs.getCurrentApplicationId() != scopeId) {
                    gs.setCurrentApplicationId(scopeId);
                }
            },
            beautifyEncodedQuery: function(table, query) {
                var gqs = new GlideQueryString(table, query);
                var prettyQuery = gqs.getPrettyQuery();
                return prettyQuery;
            },
            getAvatar: function() {
                return gs.getUser().getAvatar();
            },
            getImpersonatingUserName: function() {
                return gs.getImpersonatingUserName();
            },
            isImpersonating: function() {
                return new GlideImpersonate().isImpersonating();
            },
            getFields: function(gr) {
                var fields = gr.getFields();
                var result = [];
                for (var i = 0; i < fields.size(); i++) {

                    var field = fields.get(i);

                    result.push({
                        label: field.getLabel(),
                        name: field.getName(),
                        field_type: field.getInternalType().toString()
                    });

                }
                return result;
            },
            type: 'FCGlobalUtility'
        };
        ```
    * Navigate to `https://[your_instance]/x_509925_fc_core` to see the portal.

