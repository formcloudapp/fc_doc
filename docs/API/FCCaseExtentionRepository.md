A repository class for managing case extensions in ServiceNow. Extends the base FCRepository class.

## Constructor

### `new FCCaseExtensionRepository(overrideACL)`

Creates a new instance of the case extension repository.

**Parameters:**
- `overrideACL` (boolean): Whether to override Access Control List checks

**Example:**
```javascript
const repo = new FCCaseExtensionRepository(true);
```

## Methods

### `getById(sysId, isSerialized)`

Retrieves a case extension by its system ID.

**Parameters:**
- `sysId` (string): The system ID of the case extension
- `isSerialized` (boolean, optional): If true, returns serialized object instead of GlideRecord

**Returns:**
- `GlideRecord|Object`: The case extension record

**Example:**
```javascript
const repo = new FCCaseExtensionRepository(true);

// Get as GlideRecord
const extensionGr = repo.getById('1234567890abcdef1234567890abcdef');

// Get as serialized object
const extensionObj = repo.getById('1234567890abcdef1234567890abcdef', true);
```

### `getByTarget(targetId)`

Retrieves a case extension by the associated target task ID.

**Parameters:**
- `targetId` (string): The ID of the target task

**Returns:**
- `GlideRecord|Object`: The case extension record

**Example:**
```javascript
const repo = new FCCaseExtensionRepository(true);
const extension = repo.getByTarget('task_sys_id_here');
```

### `loadExtensionToTarget(formObj, targetId)`

Loads extension data into a form object based on the target task ID. Updates question answers in the form sections based on stored extension data.

**Parameters:**
- `formObj` (Object): The form object containing sections and questions
- `targetId` (string): The ID of the target task

**Example:**
```javascript
const repo = new FCCaseExtensionRepository(true);

const formObject = {
    sections: [{
        questions: [{
            property: 'category',
            answer: ''
        }, {
            property: 'priority',
            answer: ''
        }]
    }]
};

// Load extension data into form
repo.loadExtensionToTarget(formObject, 'task_sys_id_here');

// formObject questions will now have their answers populated from the extension data
```

## Properties

### `type`

**Type:** string  
**Value:** 'FCCaseExtensionRepository'  
Specifies the type of this repository.

### `moduleName`

**Type:** string  
**Value:** 'FCCaseExtensionRepository'  
Specifies the module name for logging and error handling.
