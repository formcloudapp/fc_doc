# FCRepository

A utility class for interacting with ServiceNow tables, providing a simplified interface for database operations.

## Constructor

### FCRepository(tableName, overrideACL)

Creates a new instance of FCRepository for a specific table.

**Parameters:**

- `tableName` (string): The name of the ServiceNow table
- `overrideACL` (boolean, optional): If true, uses GlideRecord. If false, uses GlideRecordSecure. Default: false

**Example:**
```javascript
// Using GlideRecord (overriding ACL)
var repo = new FCRepository('incident', true);

// Using GlideRecordSecure (respecting ACL)
var secureRepo = new FCRepository('incident', false);
```

## Methods

### getById(sysId, isSerialized)

Retrieves a single record by its sys_id.

**Parameters:**

- `sysId` (string): The sys_id of the record to fetch
- `isSerialized` (boolean, optional): If true, returns a serialized object. If false, returns GlideRecord. Default: false

**Returns:** GlideRecord|Object|null - The requested record

**Example:**
```javascript
// Get GlideRecord
var record = new FCRepository('incident', true)
    .getById('71c6eb7647810200e90d87e8dee490af');

// Get serialized object
var serializedRecord = new FCRepository('incident', true)
    .getById('71c6eb7647810200e90d87e8dee490af', true);
```

### addQuery(query)

Adds an encoded query to filter records.

**Parameters:**

- `query` (string): Encoded query string

**Returns:** FCRepository - Returns self for method chaining

**Example:**
```javascript
// Get GlideRecord with high priority incidents
var highPriorityGr = new FCRepository('incident', true)
    .addQuery('priority=1')
    .getMultiple();

// Get serialized array of high priority incidents
var highPriorityArray = new FCRepository('incident', true)
    .addQuery('priority=1')
    .getMultiple(true);
```

### orderBy(field)

Orders the results by a specified field in ascending order.

**Parameters:**

- `field` (string): Field name to order by

**Returns:** FCRepository - Returns self for method chaining

**Example:**
```javascript
// Get GlideRecord ordered by number
var orderedGr = new FCRepository('incident', true)
    .orderBy('number')
    .getMultiple();

// Get serialized array ordered by number
var orderedArray = new FCRepository('incident', true)
    .orderBy('number')
    .getMultiple(true);
```

### setLimit(limit)

Sets the maximum number of records to return.

**Parameters:**

- `limit` (number): Maximum number of records to return

**Returns:** FCRepository - Returns self for method chaining

**Example:**
```javascript
// Get GlideRecord with 5 records
var limitedGr = new FCRepository('incident', true)
    .setLimit(5)
    .getMultiple();

// Get serialized array with 5 records
var limitedArray = new FCRepository('incident', true)
    .setLimit(5)
    .getMultiple(true);
```

### getMultiple(isSerialized)

Gets all records matching the current query conditions.

**Parameters:**

- `isSerialized` (boolean, optional): If true, returns array of serialized objects. If false, returns GlideRecord. Default: false

**Returns:** Array<Object>|GlideRecord - The matching records

**Example:**
```javascript
// Get GlideRecord
var gr = new FCRepository('incident', true)
    .setLimit(5)
    .getMultiple();

// Get serialized array
var result = new FCRepository('incident', true)
    .setLimit(5)
    .getMultiple(true);
```

### update(fieldValues)

Updates an existing record with provided field values.

**Parameters:**

- `fieldValues` (Object): Object containing field-value pairs to update, must include sys_id

**Returns:** boolean - True if update successful, false otherwise

**Example:**
```javascript
var repo = new FCRepository('incident', true);
var result = repo.update({
    sys_id: '71c6eb7647810200e90d87e8dee490af',
    short_description: 'Updated description',
    priority: 1
});
```

### insert(fieldValues)

Creates a new record with provided field values.

**Parameters:**

- `fieldValues` (Object): Object containing field-value pairs for the new record

**Returns:** string|null - sys_id of new record if successful, null if failed

**Example:**
```javascript
var repo = new FCRepository('incident', true);
var newSysId = repo.insert({
    short_description: 'New incident',
    priority: 3,
    caller_id: gs.getUserID()
});
```

## Advanced Usage Example

Here's a more complex example showing method chaining and multiple operations:

```javascript
// Get the 10 most recent high priority incidents as serialized objects
var recentHighPriority = new FCRepository('incident', true)
    .addQuery('priority=1')
    .orderByDesc('sys_created_on')
    .setLimit(10)
    .getMultiple(true);

// Process the results
if (recentHighPriority.length > 0) {
    var repo = new FCRepository('incident', true);
    repo.update({
        sys_id: recentHighPriority[0].sys_id.value,
        work_notes: 'Automatically processed by system'
    });
}
```
