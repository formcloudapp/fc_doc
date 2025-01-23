## Overview
The `FCTaskRepository` class provides methods for interacting with ServiceNow Task records. It extends the `FCRepository` class and offers functionality for querying, updating, and managing tasks.

!!! note
    All tasks are stored in the `task` table. Using the `FCTaskRepository`, we can retrieve any record from tables that inherite from the `task` table.

## Constants

```javascript
CLOSE: 3            // Task state: Closed Complete
CLOSE_INCOMPLETE: 4 // Task state: Closed Incomplete
WORK: 2            // Task state: Work in Progress
CANCEL: 7          // Task state: Cancelled
```

## Methods

### Constructor
```javascript
var taskRepo = new FCTaskRepository();
// Or with a specific table and ACL override
var taskRepo = new FCTaskRepository('incident', true);
```

### Task Retrieval Methods

#### getAllByAssignedToMe(isSerialized)
Retrieves all tasks assigned to the current user.

```javascript
// Example 1: Get GlideRecord objects
var taskRepo = new FCTaskRepository();
var myTasks = taskRepo.getAllByAssignedToMe(false);

// Example 2: Get serialized objects
var serializedTasks = taskRepo.getAllByAssignedToMe(true);
```

#### getActiveByAssignedToMe(isSerialized)
Retrieves active tasks assigned to the current user.

```javascript
var taskRepo = new FCTaskRepository();
var activeTasks = taskRepo.getActiveByAssignedToMe(true);
```

#### getByAssignedTo(userId, isSerialized)
Retrieves tasks assigned to a specific user.

```javascript
var taskRepo = new FCTaskRepository();
var userTasks = taskRepo.getByAssignedTo('user_sys_id', true);
```

#### getSLAComingDue(numberOfDays, isSerialized)
Retrieves tasks approaching their SLA deadline.

```javascript
var taskRepo = new FCTaskRepository();
// Get tasks due within next 3 days
var upcomingTasks = taskRepo.getSLAComingDue(3, true);
```

#### getSLABreached(isSerialized)
Retrieves tasks that have breached their SLA.

```javascript
var taskRepo = new FCTaskRepository();
var breachedTasks = taskRepo.getSLABreached(true);
```

### Task Management Methods

#### closeCompleteTask(taskId)
Closes a task as complete.

```javascript
var taskRepo = new FCTaskRepository();
taskRepo.closeCompleteTask('task_sys_id');
```

#### closeIncompleteTask(taskId)
Closes a task as incomplete.

```javascript
var taskRepo = new FCTaskRepository();
taskRepo.closeIncompleteTask('task_sys_id');
```

#### workTask(taskId)
Sets a task to work in progress state.

```javascript
var taskRepo = new FCTaskRepository();
taskRepo.workTask('task_sys_id');
```

#### assignToMe(taskId)
Assigns a task to the current user.

```javascript
var taskRepo = new FCTaskRepository();
try {
    taskRepo.assignToMe('task_sys_id');
} catch(e) {
    gs.error('Unable to assign task: ' + e.message);
}
```

#### isAssignableToMe(assignmentGroupId)
Checks if current user can be assigned to a task.

```javascript
var taskRepo = new FCTaskRepository();
var canAssign = taskRepo.isAssignableToMe('group_sys_id');
if (canAssign) {
    // Proceed with assignment
}
```

#### getCountByFilterNumber(filterNumber)
Gets the count of tasks matching a specific filter.

```javascript
var taskRepo = new FCTaskRepository();
var taskCount = taskRepo.getCountByFilterNumber('filter_number');
gs.info('Number of matching tasks: ' + taskCount);
```

## Complete Usage Example

```javascript
// Initialize repository
var taskRepo = new FCTaskRepository();

// Get all active tasks assigned to current user
var myActiveTasks = taskRepo.getActiveByAssignedToMe(true);

// Process tasks approaching SLA
var upcomingTasks = taskRepo.getSLAComingDue(2, true);
for (var i = 0; i < upcomingTasks.length; i++) {
    var task = upcomingTasks[i];
    gs.info('Task ' + task.number + ' is approaching SLA deadline');
    
    // Set task to work in progress
    taskRepo.workTask(task.sys_id);
}

// Check if task is assignable and assign
if (taskRepo.isAssignableToMe('group_sys_id')) {
    try {
        taskRepo.assignToMe('task_sys_id');
        gs.info('Task successfully assigned');
    } catch(e) {
        gs.error('Assignment failed: ' + e.message);
    }
}
```

## Error Handling
- Methods that modify tasks (close, work, cancel) return the GlideRecord of the updated task
- `assignToMe()` throws an error if the current user doesn't have permission to reassign the task
- `isAssignableToMe()` returns false if the assignment group is nil or user is not a member of the group

## Notes
- All methods that return records support both serialized and non-serialized output
- When `isSerialized` is true, methods return plain JavaScript objects
- When `isSerialized` is false, methods return GlideRecord objects
- The repository supports ACL override through the constructor's second parameter
