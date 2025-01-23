## Overview
`FCChoiceRepository` is a ServiceNow repository class that provides access to choice field options from the `sys_choice` table. It extends `FCRepository` and includes functionality to retrieve choice options while respecting table inheritance.

## Class Constructor

### `FCChoiceRepository(overrideACL)`
Creates a new instance of the choice repository.

#### Parameters
- `overrideACL` (boolean): When true, bypasses Access Control List checks

#### Example
```javascript
var choiceRepo = new FCChoiceRepository(true);
```

## Methods

### `getByTableField(tableName, fieldName)`
Retrieves all choice options for a specified table and field, including inherited choices from parent tables.

#### Parameters
- `tableName` (string): The name of the table containing the choice field
- `fieldName` (string): The name of the choice field

#### Returns
Array of objects with the following structure:
- `label` (string): Display label for the choice
- `value` (string): Stored value for the choice

#### Example
```javascript
try {
    var choiceRepo = new FCChoiceRepository(true);
    
    // Get choices for incident state field
    var stateChoices = choiceRepo.getByTableField('incident', 'state');
    
    // Log the choices
    stateChoices.forEach(function(choice) {
        gs.info('Label: ' + choice.label + ', Value: ' + choice.value);
    });
    
    // Example using the choices in a workflow
    var firstChoice = stateChoices[0];
    current.state = firstChoice.value;
    
} catch (error) {
    gs.error('Failed to retrieve choices: ' + error.message);
}
```

#### Example Response
```javascript
[
    { label: "New", value: "1" },
    { label: "In Progress", value: "2" },
    { label: "On Hold", value: "3" },
    { label: "Resolved", value: "4" },
    { label: "Closed", value: "5" }
]
```

## Inheritance Behavior

The repository automatically handles table inheritance. When retrieving choices for a table:

1. It first gets choices defined directly on the specified table
2. Then traverses up the table inheritance chain to get choices from parent tables
3. Removes any duplicate values, keeping the first occurrence
4. Sorts the final list alphabetically by label

## Error Handling

All methods include error handling and will throw detailed error messages when:

- Table access is denied
- Invalid table or field names are provided
- Database queries fail

Example error handling:

```javascript
try {
    var choiceRepo = new FCChoiceRepository(true);
    var choices = choiceRepo.getByTableField('incident', 'state');
} catch (error) {
    gs.error('Choice retrieval failed: ' + error.message);
    // Handle the error appropriately
}
```

## Best Practices

1. Always wrap repository calls in try-catch blocks
2. Consider ACL implications when setting `overrideACL`
3. Cache results when making multiple calls for the same table/field combination
4. Use the returned label/value pairs consistently in your application

## Dependencies

- Requires access to `sys_choice` table
- Requires access to `sys_db_object` table for inheritance traversal
- Extends `FCRepository` class (must be available in the system)
