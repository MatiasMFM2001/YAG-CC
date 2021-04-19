# LIST / cslice cheat sheet

## templates.Slice
>>> `templates.Slice` - This is a custom composite data type defined using an underlying data type []interface{} . It is of kind slice (similar to array) having interface{} type as its value and can be initialized using cslice function. Retrieving specific element inside templates.Slice is by indexing its position number. <br><br>
>>> In human terms, a list/slice `templates.Slice` is a data type value that keep several items.

---

[DOCUMENTATION](https://docs.yagpdb.xyz/reference/templates#templates-slice)
### Defining a List
```go
{{$listExample := cslice "value1" 2 3.1 false}}
```

#### Functions
> _... means not required_
- `cslice` : create a list of type `templates.Slice`
    - Parameters/Argruments : 0 or more
    - Example 
        ```go
        {{cslice "value1" "value2" "value3"}}{{/* [value1 value2 value3] */}}
        {{cslice 1 "2" (sdict "three" 3) 4.5}}{{/* [1 2 map[three:3] 4.5] */}}
        {{1 2 3| cslice}}{{/* [1 2 3] */}}
        ```
- `index` : get a value from the list by its position number, outputs at any data type
    - Parameters/Argruments : 2 or more
        - ```index list position ...position```
        1. list : `templates.Slice` or `string`
        2. position : `int`
        3. ...position : `int`
    - Example
        ```go
        {{$example := cslice "first" "second" "third" }}
        {{index $example 0}}{{/* first */}}
        {{index $example 1}}{{/* second */}}
        {{index $example 2}}{{/* third */}}
        {{index $example 3}}{{/* error (index out of range) */}}
        {{index (cslice "value1" "value2" "value3") 2}}{{/* value3 */}}
        {{index (cslice (cslice 1 2) (cslice 3 4)) 0 1}}{{/* 2 */}}
        {{index "hello world" 0}}{{/* 104 */}}
        ```
- `slice` : extracts parts of a list and outputs the extracted parts in a new list.
    - ```slice list start ...end```
    - Parameters/Argruments : 2 or 3
        1. list : `templates.Slice` or `string`
        2. start : `int`
        3. ...end :  `int`
    - Example
        ```go
        {{$example := cslice "first" "second" "third" }}
        {{slice $example 1}}{{/* [second third] */}}
        {{slice $example 0 2}}{{/* [first second] */}}
        {{slice $example 3}}{{/* error (index out of range) */}}
        ```
#### Methods
- ```<templates.Slice>.Method ...args```
- `.Append` : add a new value in the list
    - Parameters/Argruments : 1
        - `list.Append arg`
        1. list : `templates.Slice`
        2. arg : `any`
    - Example(s)
        ```go
        {{$example := cslice 1 2 3}}
        {{$example.Append 4}}{{/* [1 2 3 4] */}}
        {{$example}}{{/* still [1 2 3] */}}

        {{$example := $example.Append 5}}
        {{$example}}{{/* [1 2 3 5] */}}

        {{(cslice "a" "b" "c").Append "d"}}{{/* [a b c d] */}}
        {{$example.Append (cslice 1)}}{{/* [1 2 3 5 [1]] */}}
        {{dict 1 "foo" 2 "bar"| $example.Append}}{{/* [1 2 3 5 map[1:foo 2:bar]] */}}
        ```
- `.AppendSlice` : join a new list in our current list
    - Parameters/Argruments : 1
        - `list.Append arg`
        1. list : `templates.Slice`
        2. arg : `templates.Slice`
    - Example(s)
        ```go
        {{$sequence := cslice.AppendSlice (seq 0 5)}}{{/* converting []int type into templates.Slice */}}
        {{$sequence}}{{/* [0 1 2 3 4] */}}
        {{$sequence.Append (cslice 5 6)}}{{/* [0 1 2 3 4 [5 6]] */}}
        {{$sequence.AppendSlice (cslice 5 6)}}{{/* [0 1 2 3 4 5 6] */}}
        {{cslice 7 8 9 10| $sequence.AppendSlice}}{{/* [0 1 2 3 4 7 8 9 10] */}}
        {{$sequence := cslice 7 8 9 10| $sequence.AppendSlice}}
        {{$sequence}}{{/* [0 1 2 3 4 7 8 9 10] */}}
        ```

#### Snippets
- Reverse
    ```go
    {{$array := cslice "first" "second" "third"}}
    {{$newArray := cslice}}
    {{range $index, $value := $array -}}
        {{- $position := sub (len $array) 1 $index -}}
        {{- $newArray = $newArray.Append (index $array $position) -}}
    {{- end}}
    {{$newArray}}{{/* [third second first] */}}
    ```
    - Shortcut
    ```go
    {{$out := cslice}}
    {{range $i, $_ := ($array := cslice "foo" "bar" "key" "val") -}}
        {{- $out = sub (len $array) 1 $i| index $array| $out.Append -}}
    {{end}}
    {{$out}}{{/* [val key bar foo] */}}
    ```
- Remove
    ```go
    {{$list := cslice "val1" "val2" "val3" "val4"}}
    {{$remove := 3}}{{$output :=cslice}}
    {{range $index, $value }}
    ```
