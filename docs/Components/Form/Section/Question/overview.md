A question is a field that allows the user to input data. Every question type has a set of attributes that can be configured to customize its behavior. However, all question types share a set of common attributes. The final attributes metadata is returned from the `attributes` script.

## Common Attributes
All question types include these base attributes:
```json
{
  "params": {},
  "table": "string",
  "target_id": "string",
  "task_id": "string",
  "active": "boolean",
  "options": {
    "width": "string",
    "label": "string", 
    "placeholder": "string",
    "css_classes": "string",
    "css_styles": "string"
  }
}
```

## Question Types


The `triggered_questions` array present in all question types contains the `number` values of other questions on the form. When the value of any question listed in this array changes, the current question will re-evaluate its attributes and potentially update its behavior, visibility, or available options. This enables dynamic form behavior where questions can react to changes in other questions' values.

For example, if a select question has `triggered_questions: ["1", "2"]`, it means this select question will react whenever the values of questions numbered "1" or "2" change on the form.

### E-Signature
Electronic signature input field
```json
{
  "triggered_questions": [],
  "pdf": {} // Optional PDF document configuration
}
```

### Text
Single line text input field
```json
{
  "triggered_questions": []
}
```

### Currency
Currency input field
```json
{
  "triggered_questions": []
}
```

### SSN
Social Security Number input field
```json
{
  "triggered_questions": []
}
```

### Email
Email input field with validation
```json
{
  "triggered_questions": [],
  "regex": "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$",
  "regex_message": "Invalid Email"
}
```

### Phone
Phone number input field
```json
{
  "triggered_questions": []
}
```

### Textarea
Multi-line text input field
```json
{
  "triggered_questions": [],
  "min_length": 0,
  "max_length": 500
}
```

### Date
Date input field with format validation
```json
{
  "triggered_questions": [],
  "regex": "^(0[1-9]|1[012])[\\/](0[1-9]|[12][0-9]|3[01])[\\/]\\d{4}$"
}
```

### Datepicker
Date picker widget
```json
{
  "triggered_questions": []
}
```

### Liquid
Liquid template rendering field
```json
{
  "triggered_questions": [],
  "data": {}
}
```

### Attachment
File attachment field
```json
{
  "triggered_questions": [],
  "table": "string",
  "target_id": "string",
  "show_delete": true
}
```

### Select
Dropdown select field
```json
{
  "triggered_questions": [],
  "select_options": [],
  "help": ""
}
```

### Radio
Radio button group
```json
{
  "triggered_questions": [],
  "select_options": [
    {
      "label": "Yes",
      "value": "Yes"
    },
    {
      "label": "No",
      "value": "No"
    }
  ],
  "vertical": true
}
```

### Autocomplete
Search-based selection field
```json
{
  "triggered_questions": [],
  "search_table": "",
  "search_query": "",
  "display_columns": [],
  "value_column": "sys_id",
  "min_length": 3,
  "help": "",
  "error": null
}
```

### Multiselect
Multiple selection field
```json
{
  "triggered_questions": [],
  "select_options": [
    {
      "label": "Option 1",
      "value": "Option 1"
    },
    {
      "label": "Option 2",
      "value": "Option 2"
    }
  ],
  "save_type": "reference"
}
```

### Multiselect List
Advanced multiple selection field with search
```json
{
  "triggered_questions": [],
  "search_table": "",
  "search_query": "",
  "display_columns": [],
  "value_column": "sys_id",
  "min_length": 3,
  "help": "",
  "required_count": 1,
  "select_options": []
}
```

### Tree
Hierarchical selection field
```json
{
  "triggered_questions": [],
  "select_options": [
    {
      "id": "option_1",
      "title": "Option 1",
      "item_count": 0,
      "children": [],
      "selectable": true
    }
  ],
  "save_type": "reference"
}
```

### Checkbox
Boolean checkbox field
```json
{
  "triggered_questions": [],
  "select_options": [
    {
      "label": "Yes",
      "value": true
    },
    {
      "label": "No",
      "value": false
    }
  ]
}
```

### Dynamic Select
Dynamic dropdown selection field
```json
{
  "triggered_questions": [],
  "select_options": []
}
```

### Dynamic Multiselect
Dynamic multiple selection field
```json
{
  "triggered_questions": [],
  "select_options": []
}
```

### Worknote
Work notes and comments field
```json
{
  "triggered_questions": [],
  "additional_comments": [],
  "work_notes": [],
  "allow_comment": "boolean",
  "allow_worknote": "boolean"
}
```

### Widget
Custom widget container
```json
{
  "triggered_questions": [],
  "widget_id": ""
}
```

### Repeater
Repeatable form section
```json
{
  "triggered_questions": [],
  "columns": [
    {
      "field_label": "Field Label",
      "field_name": "Field Name",
      "field_type": "string",
      "width": "col-md-12",
      "required": true
    }
  ]
}
```

### Related List
Related records list
```json
{
  "triggered_questions": [],
  "data": [],
  "questionnaire_id": "",
  "page_id": "hr_questionnaire",
  "columns": [
    {
      "label": "",
      "value": ""
    }
  ]
}
```

### Datatable
Data table display
```json
{
  "triggered_questions": [],
  "data": [],
  "query_name": "",
  "page_size": 10,
  "show_pagination": true,
  "hide_details": true,
  "allow_selection": false,
  "show_search": true,
  "questionnaire_id": ""
}
```