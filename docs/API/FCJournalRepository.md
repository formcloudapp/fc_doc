# FCJournalRepository API Documentation

`FCJournalRepository` is a class that extends `FCRepository` to handle journal entries stored in the 'sys_journal_field' table.

## Constructor

### `new FCJournalRepository(overrideACL)`

Creates a new instance of the journal repository.

**Parameters:**
- `overrideACL` (boolean): Flag to indicate whether to override the Access Control List (ACL)

**Example:**
```javascript
var journalRepo = new FCJournalRepository(true);
```

## Methods

### `getJournals(recordId, type)`

Retrieves journal entries for a specified record and type, including user information and avatars.

**Parameters:**
- `recordId` (string): The unique identifier of the record to retrieve journal entries for
- `type` (string): The type of journal entries to retrieve (e.g., 'comments', 'work_notes')

**Returns:**
- Array<Object>: An array of journal entry objects containing:
  - `sys_created_on` (string): Creation date/time
  - `user_name` (string): Username of the entry creator
  - `value` (string): Journal entry content
  - `avatar` (string): URL to the user's avatar image
  - `name` (string): Full name of the user

**Example:**
```javascript
var journalRepo = new FCJournalRepository(true);

// Get all comments for incident INC0010001
var comments = journalRepo.getJournals('INC0010001', 'comments');

// Example response:
[
    {
        sys_created_on: "2024-03-20 14:30:00",
        user_name: "admin",
        value: "Issue has been resolved",
        avatar: "https://instance.service-now.com/avatars/admin.iix?t=small",
        name: "System Administrator"
    },
    {
        sys_created_on: "2024-03-20 14:00:00",
        user_name: "john.doe",
        value: "Working on the issue",
        avatar: "https://instance.service-now.com/avatars/john.doe.iix?t=small",
        name: "John Doe"
    }
]
```