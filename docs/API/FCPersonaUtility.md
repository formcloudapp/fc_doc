## Overview
The `FCPersonaUtility` class provides utility methods for managing and evaluating personas in the FC application. It allows you to retrieve and evaluate persona conditions based on specified parameters.

!!! note
    All personas are stored in the `x_509925_fc_taxonomy` table with the `type` field set to `persona`.

## Methods

### getAllPersonas(params)
Retrieves all personas and evaluates their conditions based on the provided parameters.

#### Parameters
- `params` (Object): Parameters used for condition evaluation.

#### Returns
- `Object`: A mapping of persona names to their evaluated condition results
  - Keys are persona names (lowercase, spaces replaced with underscores)
  - Values are boolean results of condition evaluation

#### Example
```javascript
// Create an instance of FCPersonaUtility
var personaUtil = new FCPersonaUtility();

// Example parameters for evaluation
var params = {
    user: gs.getUserID(),
    role: 'admin',
    department: 'IT'
};

// Get all personas and their evaluation results
var personas = personaUtil.getAllPersonas(params);

// Example output:
// {
//     "it_admin": true,
//     "help_desk": false,
//     "end_user": true
// }

// Using the results
for (var personaName in personas) {
    if (personas[personaName]) {
        gs.info(personaName + ' condition evaluated to true');
    }
}
```

### isPersona(name, params)
Checks if a specific persona exists and evaluates its condition.

#### Parameters
- `name` (string): The name of the persona to check
- `params` (Object, optional): Parameters for condition evaluation. Defaults to empty object {}

#### Returns
- `boolean|string`: 
  - `false` if the persona doesn't exist or the condition is nil
  - Result of condition evaluation otherwise

#### Example
```javascript
var personaUtil = new FCPersonaUtility();

// Example 1: Check if user has 'IT Admin' persona
var params = {
    user: gs.getUserID(),
    role: 'admin'
};
var isITAdmin = personaUtil.isPersona('IT Admin', params);

if (isITAdmin) {
    gs.info('User has IT Admin persona');
}

// Example 2: Check persona without parameters
var isEndUser = personaUtil.isPersona('End User');

// Example 3: Check with custom parameters
var customParams = {
    department: 'IT',
    location: 'New York',
    isManager: true
};
var isITManager = personaUtil.isPersona('IT Manager', customParams);
```

## Usage Notes
1. Persona conditions are evaluated using `GlideScopedEvaluator`
2. The condition script has access to:
   - `current`: The persona GlideRecord
   - `params`: The parameters object passed to the method
3. Persona names are normalized to lowercase with underscores replacing spaces
4. Invalid or nil conditions always return `false`

## Error Handling
- If `name` parameter is nil in `isPersona()`, returns `false`
- If persona record is not found, returns `false`
- If condition evaluation results in nil, returns `false`

This utility is particularly useful for role-based access control and conditional behavior based on user personas in your ServiceNow instance.
