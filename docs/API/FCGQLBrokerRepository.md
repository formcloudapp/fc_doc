## Overview
The `FCGQLBrokerRepository` class is a repository pattern implementation for interacting with GraphQL query broker records in ServiceNow. It extends the base `FCRepository` class and provides functionality to retrieve and evaluate GraphQL broker records.

## Class Constructor

### `new FCGQLBrokerRepository(overrideACL)`
Creates a new instance of the GraphQL broker repository.

**Parameters:**
- `overrideACL` (boolean): When true, bypasses Access Control List restrictions

**Example:**
```javascript
var brokerRepo = new FCGQLBrokerRepository(false);
```

## Methods

### `getByName(name, params)`
Retrieves and evaluates a GraphQL broker record by its name.

**Parameters:**
- `name` (string): The name of the GraphQL broker record to retrieve
- `params` (Object, optional): Parameters that will be available in the script evaluation context

**Returns:**
- `Object`: Result containing:
  - Evaluated script result
  - `filters`: Result of the filter builder script
  - `sys_id`: System ID of the broker record

**Throws:**
- `Error`: If no record is found with the specified name

**Example:**
```javascript
try {
    // Simple query without parameters
    var result1 = brokerRepo.getByName('get_users');
    
    // Query with parameters
    var result2 = brokerRepo.getByName('get_user_by_id', {
        userId: 'abc123',
        includeRoles: true
    });
    
    gs.info('Query result: ' + JSON.stringify(result2));
} catch (ex) {
    gs.error('Failed to execute query: ' + ex.message);
}
```

## Usage Notes

1. The repository automatically handles the table name through the `FCConstantUtility`.
2. Both the main script and filter builder script have access to:
   - `current`: The GlideRecord of the broker record
   - `params`: The parameters passed to `getByName()`
3. The evaluated scripts should return objects that match your GraphQL query structure.
4. Error handling should be implemented at the calling level to handle potential exceptions.

## Best Practices

1. Always validate input parameters before using them in queries
2. Use meaningful names for broker records
3. Implement proper error handling when calling repository methods
4. Keep scripts modular and focused on a single responsibility
5. Document expected parameters for each broker record

This repository pattern provides a clean separation of concerns between GraphQL query management and business logic implementation.
