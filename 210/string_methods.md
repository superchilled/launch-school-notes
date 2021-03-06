# String Methods

  * [MDN String Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/prototype)

  * [Searching](#searching)
  * [Transforming](#transforming)
  * [Slicing and Substrings](#slicing-and-substrings)
  * [Trimming and Padding](#trimming-and-padding)

<a name='searching'></a>
## Searching

### `String.prototype.charAt()`

  * Returns the character at the specified index within a string
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charAt)

#### Parameters

  * An index value within the string the method is being called on

#### Return Value

  * An single character string
  * An empty string if the index is out of range

#### Example

```JavaScript
'ABC'.chaAt(0); // returns 'A'
```

### `String.prototype.indexOf()`

  * Returns the index within a string of the first occurence of a specified substring
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf)

#### Parameters

  * An string to search for within the overall string
  * An optional starting index

#### Return Value

  * The index of the the first occurence of the search string within the string that the method is called on
  * `-1` if the search string is not found in the string

#### Example

```JavaScript
'ABC'.indexOf('A'); // returns 0
'ABCA'.indexOf('A'); // returns 0
'ABC'.indexOf('D'); // returns -1
```

### `String.prototype.lastIndexOf()`

  * Returns the index within a string of the last occurence of a specified substring
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/lastIndexOf)

#### Parameters

  * An string to search for within the overall string
  * An optional starting index

#### Return Value

  * The index of the the last occurence of the search string within the string that the method is called on
  * `-1` if the search string is not found in the string

#### Example

```JavaScript
'ABC'.lastIndexOf('A'); // returns 0
'ABCA'.lastIndexOf('A'); // returns 3
'ABC'.lastIndexOf('D'); // returns -1
```

### `String.prototype.includes()`

  * Determines whether a substring exists within another string
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/includes)

#### Parameters

  * An string to search for within the overall string
  * An optional starting index

#### Return Value

  * `true` if the search string exists in the string that the method is called on
  * `false` if the search string does not exists in the string that the method is called on

#### Example

```JavaScript
'ABC'.includes('A'); // returns true
'ABC'.includes('D'); // returns false
```

### `String.prototype.startsWith()`

  * Determines whether a String begins with the characters of the specified search string
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/startsWith)

#### Parameters

  * A search string to be searched for within the overall string
  * An optional index value indicating a start position from which to begin searching

#### Return Value

  * `true` if the string starts with the search string
  * `false` if the string does not start with the search string

#### Examples

```JavaScript
'ABC'.startsWith('A'); // true
'ABC'.startsWith('AB'); // true
'ABC'.startsWith('B'); // false
'ABC'.startsWith('B', 1); // true
```

### `String.prototype.endsWith()`

  * Determines whether a string ends with a specified substring
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/endsWith)

#### Parameters

  * An string to search for within the overall string
  * An optional length value to determine the 'end' of the string being searched in (the default is the actual string length)

#### Return Value

  * `true` if the search string that the method is called on ends with the search string
  * `false` if the search string that the method is called on does not end with the search string

#### Example

```JavaScript
'ABC'.endsWith('A'); // returns false
'ABC'.endsWith('C'); // returns true
'ABC'.endsWith('C', 2); // returns false
```

### `String.prototype.search()`

  * Executes a search for a match between a regular expression and a String object
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/search)

#### Parameters

  * A regular expression object.
  * If a non RegExp object is passed it is implicitly converted to one using `new RegExp(obj)`

#### Return Value

  * If a match is found, `search()` returns the index of the first match within the string
  * If no match is found, `-1` is returned

#### Example

```JavaScript
'ABC'.search(/A/); // 0
'ABCA'.search(/A/); // 0
'ABCA'.search(/D/); // -1
```

### `String.prototype.match()`

  * Retreives the matches when matching a string against a regular expression
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/match)

#### Parameters

  * A regular expression object.
  * If a non RegExp object is passed it is implicitly converted to one using `new RegExp(obj)`
  * If no parameter is passed, the return value is an Array with and empty String `[""]`

#### Return Value

  * If the string matches the RegExp, an Array is returned with the entire matched string as the first element, followed by any results captured in parentheses
  * If thre were no matches, `null` is returned

#### Example

```JavaScript
'ABC'.match(/A/); // ["A", index: 0, input: "ABC"]
'ABC'.match(/A|B/); // ["A", index: 0, input: "ABC"]
'ABC'.match(/A|B/g); // ["A", "B"]
```

<a name='transforming'></a>
## Transforming

### `String.prototype.toString()`
* [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toString)

  * Returns a String representing the specified String Object

#### Parameters

  * None. Called directly on the String Object to be transformed

#### Return Value

  * A string literal representation of the String Object the method was called on

#### Example

```JavaScript
var a = new String('Hello');
a // {0: "H", 1: "e", 2: "l", 3: "l", 4: "o", length: 5}
a.toString(); // 'Hello'
```

### `String.prototype.valueOf()`

  * Returns the primitive value of a String object. Equivalent to `toString()`
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/valueOf)

#### Parameters

  * None. Called directly on the String Object to be transformed

#### Return Value

  * A string literal representation of the String Object the method was called on

#### Example

```JavaScript
var a = new String('Hello');
a // {0: "H", 1: "e", 2: "l", 3: "l", 4: "o", length: 5}
a.valueOf(); // 'Hello'
```

### `String.fromCharCode()`

  * Returns a string created by using the specified sequence of Unicode Values
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/fromCharCode)

#### Parameters

  * Can take multiple, comma separated Parameters
  * Each argument is a Unicode value between 0 and 65535

#### Return Value

  * The return value is a string

#### Example

```JavaScript
String.fromCharCode(65, 66, 67);  // returns "ABC"
```

### `String.prototype.charCodeAt()`

  * Returns an integer between 0 and 65535 representing the Unicode value of a character at a certain index in a string
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charCodeAt)

#### Parameters

  * An index value within the string the method is being called on

#### Return Value

  * An integer between 0 and 65535

#### Example

```JavaScript
'ABC'.charCodeAt(0); // returns 65
```

### `String.prototype.valueOf()`

  * Returns the primitive value of a String object. Equivalent to `toString()`
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/valueOf)


### `String.prototype.split()`

  * Splits a String object into an array of strings by separating them into substrings based on a specified separator
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split)

#### Parameters

  * An optional separator.
    * If no separator is specified, the array contains only one 'substring', which is the entire string
    * If the separator is an empty string `''`, the string is split into an array of single characters
    * The specified separator isnot included in the returned array
  * An optional integer represnting a limit on the number of splits to be found. Only substrings up to the number of splits are included in the returned array. Any leftover test is not included.

#### Example

```JavaScript
'abc,def'.split() // ["abc,def"]
'abc,def'.split(''); // "a", "b", "c", ",", "d", "e", "f"]
'abc,def'.split('', 3); // ["a", "b", "c"]
'abc,def'.split(','); // ["abc", "def"]
```

### `String.prototype.concat()`

  * Combines a string with one or more other Substrings
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/concat)

#### Parameters

  * One or more strings to concatenate into the string on which the method is called

#### Return Value

  * A new string containing the comined text of the string on which the method is called and those provided as arguments to the method call

#### Example

```JavaScript
'abc'.concat('def'); // 'abcdef'
'abc'.concat('def', 'ghi'); // "abcdefghi"
```

### `String.prototype.replace()`

  * Replaces some or all matches of a pattern within the string on which the method is called with a 'replacement'
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace)

#### Parameters

  * A RegExp object OR a string literal to be searched for in the string
  * A replacement string literal OR a function to be invoked in order to replace matches

#### Return Value

  * A new string with some or all of the matches replaced by the specified replacement

#### Example

```JavaScript
'abc'.replace('a', 'b'); // 'bbc'
'abc'.replace(/[a-z]/, 'b'); // 'bbc'
'abc'.replace('a', match => match.toUpperCase()); // 'Abc'
```

### `String.prototype.repeat()`

  * Constructs a string consisting of a specified number of copies of the string on which the method is called
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/repeat)

#### Parameters

  * An non-negative integer representing the count of repetitions of the string to occur

#### Return Value

  * A new string containing the specified number of copies of the given string concatenated together

#### Example

```JavaScript
'abc'.repeat(0); // ''
'abc'.repeat(1); // 'abc'
'abc'.repeat(2); // 'abcabc'
```

### `String.prototype.toLowerCase()`

  * Transforms the string on which the method is called to a lowercase version of itself
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase)

#### Parameters

  * None. The method is called directly on the string to be transformed without any arguments

#### Return Value

  * A lowercase version of the string on which the method is called

#### Example

```JavaScript
'ABC'.toLowerCase(); // 'abc'
'Abc'.toLowerCase(); // 'abc'
'ABC1'.toLowerCase(); // 'abc1'
```

### `String.prototype.toUpperCase()`

  * Transforms the string on which the method is called to an uppercase version of itself
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase)

#### Parameters

  * None. The method is called directly on the string to be transformed without any arguments

#### Return Value

  * An uppercase version of the string on which the method is called

#### Example

```JavaScript
'abc'.toUpperCase(); // 'ABC'
'Abc'.toUpperCase(); // 'ABC'
'abc1'.toUpperCase(); // 'ABC1'
```

<a name='slicing-and-substrings'></a>
## Slicing and Substrings

### `String.prototype.slice()`

  * Extracts a specified section of a string
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/slice)

#### Parameters

  * An index within the string on which the method is called at which to begin the extraction
    * If the index is greater than or equal to the length of the string, an empty string is returned
    * If this value is negative, it is subtracted from `string.length` to give the start index
  * An optional end index before which to end extraction
    * If omitted, then the portion of the string from the beginning index to the end of the string is extracted (i.e. the default is `string.length -1`)
    * If a negative value is used it is subtracted from `string.length` to provide the end index

#### Return Value

  * A new string, which is the extracted portion of the string on which the method was called
  * If the start is greater than or equal to the length of the string, an empty string is returned
  * If the start index is larger than the end index, an empty string is returned

#### Example

```JavaScript
'abcdef'.slice(2); // 'cdef'
'abcdef'.slice(2, 4); // 'cd'
'abcdef'.slice(2, -2); // 'cd'
'abcdef'.slice(-4, -2); // 'cd'
'abcdef'.slice(4, 2); // ''
'abcdef'.slice(10); // ''
```

### `String.prototype.substr()`

  * Extracts a specified section of a string
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substr)

#### Parameters

  * An index within the string on which the method is called at which to begin the extraction
    * If the index is greater than or equal to the length of the string, an empty string is returned
    * If this value is negative, it is subtracted from `string.length` to give the start index
  * An optional length indicating the number of charaters to extract
    * If omitted, then the portion of the string from the beginning index to the end of the string is extracted
    * If a negative value is used an empty string is returned

#### Return Value

  * A new string, which is the extracted portion of the string on which the method was called
  * If the start is greater than or equal to the length of the string, an empty string is returned
  * If the number of characters to extract (length argument) negative value is used an empty string is returned

#### Example

```JavaScript
'abcdef'.substr(2); // 'cdef'
'abcdef'.substr(2, 4); // 'cdef'
'abcdef'.substr(2, -2); // ''
'abcdef'.substr(-4, 2); // 'cd'
'abcdef'.substr(4, 2); // 'ef'
'abcdef'.substr(10); // ''
```

### `String.prototype.substring()`

  * Extracts a specified section of a string
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substring)

#### Parameters

* An index within the string on which the method is called at which to begin the extraction
  * If the index is greater than or equal to the length of the string, an empty string is returned
  * If this value is negative, it is treated as 0
  * If this value is greater than index end, the two values are swapped
* An optional end index before which to end extraction
  * If omitted, then the portion of the string from the beginning index to the end of the string is extracted
  * If this value is negative, it is treated as 0

#### Return Value

* A new string, which is the extracted portion of the string on which the method was called
* If the start is greater than or equal to the length of the string, an empty string is returned
* If index start equals index end, an empty string is returned

#### Example

```JavaScript
'abcdef'.substring(2); // 'cdef'
'abcdef'.substring(2, 4); // 'cd'
'abcdef'.substring(2, 2); // ''
'abcdef'.substring(-4, 2); // 'ab'
'abcdef'.substring(2, -2); // 'ab'
'abcdef'.substring(4, 2); // 'cd'
'abcdef'.substring(10); // ''
```

### Similarities and differences between `slice`, `substr`, and `substring`

  * They all return a portion of a larger string based on arguments passed to the methods
  * All take a mandatory first argument and optional second argument

#### First Argument

  * For all three this is the **index at which extraction should begin**
  * If the start index is greater than the length of the string to be extracted from, all three methods will return an empty string
  * If negative:
    * `slice` will treat as the length of the string minus the absolute version of the argument
    * `substr` will treat as the length of the string minus the absolute version of the argument
    * `substring` will treat as `0`.

#### Second Argument

  * They all take a second, optional argument:
    * `slice` treats this as the **end index** before which extraction should end (i.e. if the end index is `5` then the character at index `4` should be included in the extracted string, but not the character at index `5`).
    * `substr` treats this as a **length argument**, indicating the *number of characters to be extracted* from the start point.
    * `substring` treats this as the **end index** before which extraction should end (i.e. if the end index is `5` then the character at index `4` should be included in the extracted string, but not the character at index `5`).

  * If negative:
    * `slice` will treat as the length of the string minus the absolute version of the argument
    * `substr` will return an empty string
    * `substring` will treat this as `0`

  * If less than the first argument:
    * `slice` will return an empty string
    * `substr` will return will return the number of characters indicated by the argument, as long as it is not negative
    * `substring` will effectively swap the two values

  * If ommitted:
    * With all three, all the characters from the start point to the end of the string are extracted. In this sense, using `slice`, `substr`, or `substring` with only one argument (as long as it is a positive integer) is identical in its results.

#### When to use each one

  * If you you want to remove a specific number of chatracters from either end of a string, use `slice` with a negative second argument; e.g `slice(3, -3)` will remove three characters from either end of the string and return the rest
  * If you want to extract a specific number of characters starting at a certain point use `substr`; e.g. `substr(3, 3)` will extract three characters from a string, starting at index `3`
  * If you know the exact start and end indices for extraction use `substring`; e.g. `substring(3, 6)` will extract the characters at indices `3`, `4`, and `5`.


[Similarities and Differences between Methods](https://stackoverflow.com/questions/2243824/what-is-the-difference-between-string-slice-and-string-substring)

<a name='trimming-and-padding'></a>
## Trimming and Padding

### `String.prototype.trim()`

  * Removes whitespace from both ends of a string.
  * Whitespace is considered to be all the whitespace characters (space, tab, no-break space, etc.) and all the line terminator characters (LF, CR, etc.).
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/Trim)

#### Parameters

  * None. The method is called directly on the string to be transformed without any arguments

#### Return Value

  * A new version of the string with whitespace removed from both ends

#### Example

```JavaScript
' abc '.trim(); // 'abc'
' abc'.trim(); // 'abc'
'abc '.trim(); // 'abc'
'abc\n'.trim(); // 'abc'
```

### `String.prototype.trimLeft()`

* Removes whitespace from the left side of a string only.
* Whitespace is considered to be all the whitespace characters (space, tab, no-break space, etc.) and all the line terminator characters (LF, CR, etc.).
* **Note: this is a non-standard feature**
* [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/TrimLeft)

#### Parameters

* None. The method is called directly on the string to be transformed without any arguments

#### Return Value

* A new version of the string with whitespace removed from the left side

#### Example

```JavaScript
' abc '.trimLeft(); // 'abc '
' abc'.trimLeft(); // 'abc'
'abc '.trimLeft(); // 'abc '
'abc\n'.trimLeft(); // 'abc\n'
```

### `String.prototype.trimRight()`

* Removes whitespace from the right side of a string only.
* Whitespace is considered to be all the whitespace characters (space, tab, no-break space, etc.) and all the line terminator characters (LF, CR, etc.).
* **Note: this is a non-standard feature**
* [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/TrimRight)

#### Parameters

* None. The method is called directly on the string to be transformed without any arguments

#### Return Value

* A new version of the string with whitespace removed from the right side

#### Example

```JavaScript
' abc '.trimRight(); // ' abc'
' abc'.trimRight(); // ' abc'
'abc '.trimRight(); // 'abc'
'abc\n'.trimRight(); // 'abc'
```

### `String.prototype.padEnd()`

  * Pads the end of a string to a specified padding length using a specified padding string
  * [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/padEnd)

#### Parameters

  * An integer representing a padding length indicating the length the string should be once padded
    * If this value is less than the current length of the string no padding is added and the string is returned as it was
  * An optional padding string with which to pad the string. If omitted, the default `' '` is used.
    * If the entire padding string won't fit into the specified length, the it is truncated and only the portion which fits is used

#### Return Value

  * A new string of the specified length with the padding applied

#### Example

```JavaScript
'abc'.padEnd(4); // 'abc '
'abc'.padEnd(2); // 'abc'
'abc'.padEnd(5, 'd'); // 'abcdd'
'abc'.padEnd(5, 'hello'); // 'abche'
```

### `String.prototype.padStart()`

* Pads the start of a string to a specified padding length using a specified padding string
* [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/padStart)

#### Parameters

* An integer representing a padding length indicating the length the string should be once padded
  * If this value is less than the current length of the string no padding is added and the string is returned as it was
* An optional padding string with which to pad the string. If omitted, the default `' '` is used.
  * If the entire padding string won't fit into the specified length, the it is truncated and only the portion which fits is used

#### Return Value

* A new string of the specified length with the padding applied

#### Example

```JavaScript
'abc'.padStart(4); // ' abc'
'abc'.padStart(2); // 'abc'
'abc'.padStart(5, 'd'); // 'ddabc'
'abc'.padStart(5, 'hello'); // 'heabc'
```
