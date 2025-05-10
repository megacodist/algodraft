---
status: Writing
---
An instance of this type represents an opened file ready for I/O operations.

### Methods
#### Opening

```
$configFile AS File<String> <- DO {{open the file $filePath in text, read mode}}
$binFile AS File<Binary> <- DO {{open the file $filePath in binary, read mode}}
$someFile AS File<Record> <- DO {{open the file $filePath to read its records}}
```

#### Closing
```
DO {{close $configFile}}
```
