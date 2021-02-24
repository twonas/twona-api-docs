# Search and Sort Queries

In some cases you can **filter** and **sort** the results of a returned list using query strings. Results can also be returned with **page**.

## Syntax

A filter/sort query is composed of three parts: the query **_q_** to filter (search) the result set, the sort **_short_by_** to arrange the order, and the page **page** to navigate the pages of results. You can add more than one filter/short term. Spaces are not allowd in the final query string.

```
q=field1:term1+field2:term2+field3:term3&sort_by=field1,-field2&page=2 // + -> is a condition
```

**IMPORTANT:** the string needs to be encoded before it is added to the url.

### Filter field

You can establish filter (search) queries using: **q=[conditions]**. Individual consitions consist of the form **field=term** and can be concatenated by using AND, AND NOT and OR statements as follows:

Concatenation | Description
--------- | ----------
`+`    | AND
`-`    | AND NOT
<code>&#124;</code> | OR (start a new condition block)

### Modifiers

You can use modifiers to expand the filter (search) criteria as follows:

Modifier | Types allowed | Description
-------- | ------------- | -----------
`*`      | string        | Define the position of the string
`>`      | number, date  | Greater than, after than
`<`      | number, date  | Lower than, before than

Example    |  Modifier
---------- | ----------
`:term`   | (Without modifier) Search exactly the term.
`:*term`   | Search the term at the end of the value.
`:term*`   | Search the term at the beginning of the value.
`:*term*`  | The value contains the term. (In this case the term can be also at the beginning and/or at the end of the value).

**IMPORTANT:** query field is mandatory.

### Sort field

You can sort the result set by using: **sort_by=[field1,field2]**  

By default the order is ASC. You can use `-` to specify the order DESC before the field as in the example below.

```
q=...&sort_by=field1,-field2...
```

### Page field

Results can be returned by pages using **page=[int]** as follows:

```
q=...&sort_by=...&page=2
```

## Search Examples

A table like:

ID | NAME | EMAIL
----- | ------ | -------
**1** | Seva Halter | seva.halter@twonas.com
**2** | Brigitte Marland | brigitter.marland@twonas.com
**3** | Diego Beltrán | diego@twonas.com
**4** | Donna Brinn | donna@twona.com
**5** | Seva Blade | seva.blade@gmail.com

### Example 1

Where the `email` is equal to `diego@twonas.com`.

String | Encoded | Expected result
-------- | -------- | --------
`q=email:diego@twonas.com` | `email%3Adiego%40twonas.com` | [1]

### Example 2

Where the `email` field ends with `twonas.com`.

String | Encoded | Expected result
-------- | -------- | --------
`q=email:*twonas.com&sort_by=id` | `q%3Demail%3A%2Atwonas.com%26sort_by%3Did` | [1,2,3,4]

### Example 3

Where the `email` field ends with `twonas.com` and starts with `diego`.

String | Encoded | Expected result
-------- | -------- | --------
`q=email:*twonas.com+email:diego*` | `q%3email%3A%2Atwonas.com%2Bemail%3Adiego%2A`  | [3]

### Example 4

Where the `email` field ends with `twonas.com` and starts with `diego`, or the `email` field starts with `seva`.

String | Encoded | Expected result
-------- | -------- | --------
`q=email:*twonas.com+email:diego*|email:seva*&sort_by=-id` | `q%3Demail%3A%2Atwonas.com%2Bemail%3Adiego%2A%7Cemail%3Aseva%2A%26sort_by%3D-id` | [5,3,1]

### Example 5

Where the `email` field starts with `seva` and not ends with `twonas.com`.

String | Encoded | Expected result
-------- | -------- | --------
`q=email:seva*-email:*twonas.com` | `q%3email%3Aseva%2A-email%3A%2Atwonas.com`  | [5]

### Example 6

Where the `email` field starts with `seva` and not ends with `twonas.com`, or the `email` field starts with `diego`.

String | Encoded | Expected result
-------- | -------- | --------
`q=email:seva*-email:*twonas.com|email:diego*&sort_by=name` | `q%3Demail%3Aseva%2A-email%3A%2Atwonas.com%7Cemail%3Adiego%2A%26sort_by%3Dname`  | [3, 5]
