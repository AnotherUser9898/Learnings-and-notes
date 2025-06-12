#nodejs #fs-module

# 📁 Node.js `fs` Module — Complete Guide (Callback & Promise APIs Only)

> Includes **descriptions**, **parameter breakdowns** with **data type support**, detailed **examples**, and **output** for each function.

---

## 📚 Contents

1. `fs.readFile`
    
2. `fs.writeFile`
    
3. `fs.appendFile`
    
4. `fs.stat`
    
5. `fs.access`
    
6. `fs.readdir`
    
7. `fs.mkdir`
    
8. `fs.rm`
    
9. `fs.rename`
    
10. `fs.open`
    
11. `fs.read`
    
12. `fs.write`
    
13. `fs.ftruncate`
    
14. `fs.truncate`
    

---

## 1. `fs.readFile(path[, options], callback)`

### Description

Reads the entire contents of a file. Supports reading as a string (with encoding) or raw Buffer.

### Parameters

|Parameter|Type|Description|
|---|---|---|
|`path`|`string` \| `Buffer` \| `URL` \| `number`|Path to the file or a file descriptor|
|`options`|`string` or `object`|Encoding and flags|
|`callback`|`function(err, data)`|Called after read completes|

### Accepted `options`

- `encoding`: `'utf8'`, `'base64'`, `null` (for Buffer)
    
- `flag`: `'r'`, `'r+'`, etc.
    

### Examples

#### a. Path as `string`

```js
fs.readFile('example.txt', 'utf8', (err, data) => console.log(data));
```

**Output:**

```
Hello from string path
```

#### b. Path as `Buffer`

```js
fs.readFile(Buffer.from('example.txt'), 'utf8', (err, data) => console.log(data));
```

**Output:**

```
Hello from string path
```

#### c. Path as `URL`

```js
const { URL } = require('url');
fs.readFile(new URL('file:///absolute/path/example.txt'), 'utf8', (err, data) => console.log(data));
```

**Output:**

```
Hello from string path
```

#### d. Path as `file descriptor`

```js
fs.open('example.txt', 'r', (err, fd) => {
  fs.readFile(fd, 'utf8', (err, data) => {
    console.log(data);
    fs.close(fd, () => {});
  });
});
```

**Output:**

```
Hello from string path
```

---

## 2. `fs.writeFile(path, data[, options], callback)`

### Description

Writes `data` to a file, replacing the file if it exists.

### Parameters

|Parameter|Type|Description|
|---|---|---|
|`path`|`string` \| `Buffer` \| `URL` \| `number`|File path or descriptor|
|`data`|`string` \| `Buffer` \| `TypedArray` \| `DataView`|Content to write|
|`options`|`string` \| `object`|Encoding, mode, and flag|
|`callback`|`function(err)`|Called when complete|

### Accepted `options`

- `encoding`: `'utf8'`, `'ascii'`, `'base64'`
    
- `mode`: `0o666` (file permission)
    
- `flag`: `'w'` (default), `'a'`, `'wx'`
    

### Examples

```js
fs.writeFile('file.txt', 'Hello World!', { encoding: 'utf8', flag: 'w' }, (err) => {
  if (err) throw err;
  console.log('Written!');
});
```

**Output:**

```
Written!
```

---

## 3. `fs.appendFile(path, data[, options], callback)`

### Description

Appends `data` to the file. Creates it if not present.

### Parameters

Same as `fs.writeFile`, default `flag` is `'a'`.

### Example

```js
fs.appendFile('log.txt', 'Log entry\n', (err) => console.log('Appended'));
```

**Output:**

```
Appended
```

---

## 4. `fs.stat(path[, options], callback)`

### Description

Returns metadata about a file or directory.

### Parameters

|Parameter|Type|Description|
|---|---|---|
|`path`|`string` \| `Buffer` \| `URL`|Path to file or directory|
|`options`|`object`|`bigint` boolean (default: false)|
|`callback`|`function(err, stats)`|Called with `fs.Stats` object|

### Example

```js
fs.stat('file.txt', (err, stats) => {
  console.log(stats.size, stats.isFile());
});
```

**Output:**

```
12 true
```

---

## 5. `fs.access(path[, mode], callback)`

### Description

Checks file permissions or existence.

### Parameters

|Parameter|Type|Description|
|---|---|---|
|`path`|`string` \| `Buffer` \| `URL`|Path to check|
|`mode`|`number`|Permission to check (default: `fs.constants.F_OK`)|
|`callback`|`function(err)`|Called when done (error means not accessible)|

### Modes

- `fs.constants.F_OK` (default)
    
- `fs.constants.R_OK`, `W_OK`, `X_OK`
    

### Example

```js
fs.access('file.txt', fs.constants.R_OK, (err) => {
  console.log(err ? 'No read access' : 'Readable');
});
```

**Output:**

```
Readable
```

---

## 6. `fs.readdir(path[, options], callback)`

### Description

Reads contents of a directory.

### Parameters

|Parameter|Type|Description|
|---|---|---|
|`path`|`string` \| `Buffer` \| `URL`|Directory path|
|`options`|`object`|`{ encoding, withFileTypes }`|
|`callback`|`function(err, files)`|Called with file names or `Dirent` objects|

### Example

```js
fs.readdir('.', { withFileTypes: true }, (err, files) => {
  files.forEach(f => console.log(f.name, f.isFile()));
});
```

**Output:**

```
file.txt true
directory false
```

---

## 7. `fs.mkdir(path[, options], callback)`

### Description

Creates a directory.

### Parameters

|Parameter|Type|Description|
|---|---|---|
|`path`|`string` \| `Buffer` \| `URL`|Directory to create|
|`options`|`object`|`{ recursive, mode }`|
|`callback`|`function(err)`|Called when done|

### Options

- `recursive`: `true` (create parent folders)
    
- `mode`: e.g., `0o755`
    

### Example

```js
fs.mkdir('dir/subdir', { recursive: true }, () => console.log('Done'));
```

**Output:**

```
Done
```

---

## 8. `fs.rm(path[, options], callback)`

### Description

Deletes a file or folder.

### Parameters

|Parameter|Type|Description|
|---|---|---|
|`path`|`string` \| `Buffer` \| `URL`|Path to remove|
|`options`|`object`|`{ recursive, force }`|
|`callback`|`function(err)`|Called when done|

### Options

- `recursive`: true
    
- `force`: true
    

### Example

```js
fs.rm('dir', { recursive: true, force: true }, () => console.log('Deleted'));
```

**Output:**

```
Deleted
```

---

## 9. `fs.rename(oldPath, newPath, callback)`

### Description

Renames or moves a file.

### Parameters

|Parameter|Type|Description|
|---|---|---|
|`oldPath`|`string` \| `Buffer` \| `URL`|Existing file|
|`newPath`|`string` \| `Buffer` \| `URL`|New name or destination|
|`callback`|`function(err)`|Called when done|

### Example

```js
fs.rename('old.txt', 'new.txt', () => console.log('Renamed'));
```

**Output:**

```
Renamed
```

---

## 10. `fs.open(path, flags[, mode], callback)`

### Description

Opens a file and returns file descriptor.

### Parameters

|Parameter|Type|Description|
|---|---|---|
|`path`|`string` \| `Buffer` \| `URL`|File to open|
|`flags`|`string`|Mode to open in (`'r'`, `'w'`, etc.)|
|`mode`|`integer`|File mode (e.g. `0o666`)|
|`callback`|`function(err, fd)`|Called with file descriptor|

### Flags

- `'r'`, `'w'`, `'a'`, `'r+'`, etc.
    

### Example

```js
fs.open('file.txt', 'r', (err, fd) => {
  console.log('FD:', fd);
  fs.close(fd, () => {});
});
```

**Output:**

```
FD: 3
```

---

## 11. `fs.read(fd, buffer, offset, length, position, callback)`

### Description

Low-level read from a file.

### Parameters

|Parameter|Type|Description|
|---|---|---|
|`fd`|`integer`|File descriptor from `fs.open`|
|`buffer`|`Buffer`|Buffer to write to|
|`offset`|`integer`|Start in buffer|
|`length`|`integer`|Bytes to read|
|`position`|`integer`|Position in file|
|`callback`|`function(err, bytesRead, buffer)`|Called after read|

### Example

```js
const buffer = Buffer.alloc(10);
fs.open('file.txt', 'r', (err, fd) => {
  fs.read(fd, buffer, 0, 10, 0, (err, bytes, buff) => {
    console.log(buff.toString('utf8', 0, bytes));
    fs.close(fd, () => {});
  });
});
```

**Output:**

```
Hello Worl
```

---

## 12. `fs.write(fd, buffer, offset, length, position, callback)`

### Description

Low-level write to a file.

### Parameters

|Parameter|Type|Description|
|---|---|---|
|`fd`|`integer`|File descriptor from `fs.open`|
|`buffer`|`Buffer` \| `TypedArray` \| `DataView`|Data to write|
|`offset`|`integer`|Start in buffer|
|`length`|`integer`|Bytes to write|
|`position`|`integer`|Position in file|
|`callback`|`function(err, written, buffer)`|Called after write|

### Example

```js
const buffer = Buffer.from('12345');
fs.open('file.txt', 'r+', (err, fd) => {
  fs.write(fd, buffer, 0, 5, 6, (err, written, buff) => {
    console.log('Wrote:', buff.toString());
    fs.close(fd, () => {});
  });
});
```

**Output:**

```
Wrote: 12345
```

---

## 13. `fs.ftruncate(fd, len, callback)`

### Description

Truncates an open file to specified length.

### Parameters

|Parameter|Type|Description|
|---|---|---|
|`fd`|`integer`|File descriptor|
|`len`|`integer`|Number of bytes to retain|
|`callback`|`function(err)`|Called after truncation|

### Example

```js
fs.open('file.txt', 'r+', (err, fd) => {
  fs.ftruncate(fd, 5, (err) => {
    fs.close(fd, () => {});
  });
});
```

**Effect:** `file.txt` is cut to 5 bytes.

---

## 14. `fs.truncate(path, len, callback)`

### Description

Truncates file to specified length.

### Parameters

|Parameter|Type|Description|
|---|---|---|
|`path`|`string` \| `Buffer` \| `URL`|File path|
|`len`|`integer`|Length in bytes|
|`callback`|`function(err)`|Called after truncation|

### Example

```js
fs.truncate('file.txt', 5, () => console.log('Truncated'));
```

**Effect:** file is cut to 5 bytes.

---