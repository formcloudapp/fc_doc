## Overview

The FCLoggerUtility class provides logging functionality for error and debug messages in ServiceNow applications.

## Constructor

### `new FCLoggerUtility()`
Creates a new instance of the FCLoggerUtility class.

```javascript
var logger = new FCLoggerUtility();
```

## Methods

### `error(message, moduleName, stack)`
Logs an error message with a unique identifier and stack trace.

**Parameters:**
- `message` (string): The error message to log
- `moduleName` (string): The module name associated with the error
- `stack` (string): The stack trace for the error

**Returns:**
- `string`: A unique 6-digit error code

**Example:**
```javascript
var logger = new FCLoggerUtility();

try {
    // Some code that might throw an error
    throw new Error('Invalid data format');
} catch (e) {
    var errorCode = logger.error(
        'Failed to process data', 
        'DataProcessor', 
        e.stack
    );
    // errorCode will be something like '123456'
    // Log output will be: "[123456] DataProcessor: Failed to process data\nStack Trace:\n<stack trace details>"
}
```

### `debug(message, param)`
Logs a debug message with an optional parameter.

**Parameters:**
- `message` (string): The debug message to log
- `param` (any): Additional parameter to log alongside the message

**Example:**
```javascript
var logger = new FCLoggerUtility();

// Simple debug message
logger.debug('Processing started');

// Debug message with parameter
var userData = {
    id: '12345',
    name: 'John Doe'
};
logger.debug('User data received', userData);
```

## Usage Example

Here's a complete example showing how to use both methods in a typical scenario:

```javascript
var logger = new FCLoggerUtility();

function processUserData(userData) {
    try {
        logger.debug('Starting user data processing', userData);
        
        if (!userData.id) {
            throw new Error('User ID is required');
        }
        
        // Process the data...
        logger.debug('User data processed successfully');
        
    } catch (e) {
        var errorCode = logger.error(
            'User data processing failed', 
            'UserProcessor', 
            e.stack
        );
        gs.addErrorMessage('Processing failed. Error code: ' + errorCode);
    }
}

// Using the function
processUserData({
    name: 'John Doe'
    // Missing id field will trigger error
});
```

This example demonstrates how to:
1. Create a logger instance
2. Use debug logging for tracking execution flow
3. Handle errors with unique error codes
4. Include relevant context in both debug and error logs
