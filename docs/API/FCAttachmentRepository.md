A repository class for managing attachments in ServiceNow. Extends `FCRepository`.

## Constructor

### `new FCAttachmentRepository(overrideACL)`
Creates a new instance of FCAttachmentRepository.

- **Parameters:**
  - `overrideACL` (boolean, optional): If true, uses GlideRecord. If false, uses GlideRecordSecure. Default: false

```javascript
var attachmentRepo = new FCAttachmentRepository(true);
```

## Methods

### `create(content, fileName, tableName, tableSysId)`
Creates an attachment from base64 encoded content.

- **Parameters:**
  - `content` (string): Base64 encoded content with data URI prefix
  - `fileName` (string): Name for the file
  - `tableName` (string): Target ServiceNow table
  - `tableSysId` (string): Target record's sys_id
- **Returns:** GlideRecord of created attachment
- **Throws:** Error if parameters missing or invalid content

```javascript
var content = 'data:application/pdf;base64,JVBERi0xLjcKCjEgMCBvYmo...';
var attachment = attachmentRepo.create(
    content,
    'document.pdf',
    'incident',
    '1234567890abcdef1234567890abcdef'
);
```

### `parseBase64Content(base64String)`
Parses base64 content string to extract content type and data.

- **Parameters:**
  - `base64String` (string): Base64 encoded content with data URI prefix
- **Returns:** Object with contentType and content properties, or null if invalid

```javascript
var result = attachmentRepo.parseBase64Content('data:image/png;base64,iVBORw0KGgoAAAA...');
// Returns: { contentType: 'image/png', content: 'iVBORw0KGgoAAAA...' }
```

### `getBase64ById(id)`
Retrieves base64 content of an attachment by sys_id.

- **Parameters:**
  - `id` (string): Attachment sys_id
- **Returns:** Base64 encoded content string
- **Throws:** Error if id missing or access denied

```javascript
var base64Content = attachmentRepo.getBase64ById('1234567890abcdef1234567890abcdef');
```

### `getBase64ByTemplateId(id)`
Retrieves base64 content of an attachment associated with a PDF template.

- **Parameters:**
  - `id` (string): PDF template sys_id
- **Returns:** Base64 encoded content string
- **Throws:** Error if no attachment found

```javascript
var templateContent = attachmentRepo.getBase64ByTemplateId('1234567890abcdef1234567890abcdef');
```

### `getById(sysId, isSerialize)`
Retrieves attachment record by sys_id.

- **Parameters:**
  - `sysId` (string): Attachment sys_id
  - `isSerialize` (boolean, optional): If true, returns JSON object
- **Returns:** GlideRecord or JSON object of attachment

```javascript
// Get GlideRecord
var attachmentRecord = attachmentRepo.getById('1234567890abcdef1234567890abcdef');

// Get serialized JSON
var attachmentJson = attachmentRepo.getById('1234567890abcdef1234567890abcdef', true);
```

### `deleteById(sysId)`
Deletes an attachment by sys_id.

- **Parameters:**
  - `sysId` (string): Attachment sys_id
- **Returns:** Boolean indicating success
- **Throws:** Error if deletion fails

```javascript
var isDeleted = attachmentRepo.deleteById('1234567890abcdef1234567890abcdef');
if (isDeleted) {
    gs.info('Attachment deleted successfully');
}
```

## Error Handling
All methods include error handling and will throw errors with detailed information when issues occur. Errors are handled through the inherited `throwError` method from `FCRepository`.
