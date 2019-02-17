```                __
                  /\ \                                                       
 __  __    ___    \_\ \     __   _ __   ____    ___    ___   _ __    __	         __     ___
/\ \/\ \ /' _ `\  /'_  \  /'__`\/\  __\/ ,__\  / ___\ / __`\/\  __\/'__`\      /'_ `\  / __`\
\ \ \_\ \/\ \/\ \/\ \ \ \/\  __/\ \ \//\__, `\/\ \__//\ \ \ \ \ \//\  __/  __ /\ \L\ \/\ \L\ \
 \ \____/\ \_\ \_\ \___,_\ \____\\ \_\\/\____/\ \____\ \____/\ \_\\ \____\/\_\\ \____ \ \____/
  \/___/  \/_/\/_/\/__,_ /\/____/ \/_/ \/___/  \/____/\/___/  \/_/ \/____/\/_/ \/___L\ \/___/
                                                                                 /\____/
                                                                                 \_/__/
```

# Underscore.go
==========================================

like <a href="http://underscorejs.org/">underscore.js</a>, but for Go

## Installation

    $ go get github.com/ahl5esoft/golang-underscore

## Update
	$ go get -u github.com/ahl5esoft/golang-underscore

## Lack
	* to be continued...

## Documentation

### API
* [`All`](#all), [`AllBy`](#allBy)
* [`Any`](#any), [`AnyBy`](#anyBy)
* [`AsParallel`](#asParallel)
* [`Chain`](#chain)
* [`Clone`](#clone)
* [`Each`](#each)
* [`Find`](#find), [`FindBy`](#findBy)
* [`FindIndex`](#findIndex), [`FindIndexBy`](#findIndexBy)
* [`First`](#first)
* [`Group`](#group), [`GroupBy`](#groupBy)
* [`IsArray`](#isArray)
* [`IsMatch`](#isMatch)
* [`Keys`](#keys)
* [`Last`](#last)
* [`Map`](#map), [`MapBy`](#mapBy)
* [`Md5`](#md5)
* [`Object`](#object)
* [`Property`](#property), [`PropertyRV`](#propertyRV)
* [`Range`](#range)
* [`Reduce`](#reduce)
* [`Reject`](#reject), [`RejectBy`](#rejectBy)
* [`Reverse`](#reverse), [`ReverseBy`](#reverseBy)
* [`Size`](#size)
* [`Sort`](#sort), [`SortBy`](#sortBy)
* [`Take`](#take)
* [`Uniq`](#uniq), [`UniqBy`](#uniqBy)
* [`UUID`](#uuid)
* [`Value`](#value)
* [`Values`](#values)
* [`Where`](#where), [`WhereBy`](#whereBy)

<a name="all" />

### All(source, predicate)

__Arguments__

* `source` - array or map
* `predicate` - func(element, index or key) bool

__Return__

* bool - all the values that pass a truth test `predicate`

__Examples__

```go
arr := []TestModel{
	TestModel{1, "one"},
	TestModel{1, "two"},
	TestModel{1, "three"},
}
ok := All(arr, func(r TestModel, _ int) bool {
	return r.Id == 1
})
// ok == true
```

<a name="allBy" />

### AllBy(source, properties)

__Arguments__

* `source` - array or map
* `properties` - map[string]interface{}

__Return__

* bool

__Examples__

```go
arr := []TestModel{
	TestModel{1, "one"},
	TestModel{1, "two"},
	TestModel{1, "three"},
}
ok := AllBy(arr, nil)
// ok == true

ok = AllBy(arr, map[string]interface{}{
	"name": "a",
})
// ok == false

ok = AllBy(arr, map[string]interface{}{
	"id": 1,
})
// ok == true
```

<a name="any" />

### Any(source, predicate)

__Arguments__

* `source` - array or map
* `predicate` - func(element or value, index or key) bool

__Return__

* bool - any of the values that pass a truth test `predicate`

__Examples__

```go
arr := []TestModel{
	TestModel{1, "one"},
	TestModel{2, "two"},
	TestModel{3, "three"},
}
ok := Any(arr, func(r TestModel, _ int) bool {
	return r.Id == 0
})
// ok == false
```

<a name="anyBy" />

### AnyBy(source, properties)

__Arguments__

* `source` - array or map
* `properties` - map[string]interface{}

__Return__

* bool

__Examples__

```go
arr := []TestModel{
	TestModel{1, "one"},
	TestModel{2, "two"},
	TestModel{3, "three"},
}
ok := AnyBy(arr, map[string]interface{}{
	"Id": 0,
})
// ok == false

ok = AnyBy(arr, map[string]interface{}{
	"id":   arr[0].Id,
	"name": arr[0].Name,
})
// ok == true
```

<a name="asParallel" />

### Chain(source).AsParallel()...

__Support__
* `Each`
* `Object`

__Examples__

```go
arr := []int{ 1, 2, 3 }
Chain(arr).AsParallel().Each(func (n, i int) {
	// code
})
```

<a name="chain" />

### Chain(source)

__Arguments__

* `source` - array or map

__Return__

* IQuery - a wrapped object, wrapped objects until value is called

__Examples__

```go
res := make(map[string][]int)
Chain([]int{1, 2, 1, 4, 1, 3}).Uniq(nil).Group(func(n, _ int) string {
	if n%2 == 0 {
		return "even"
	}

	return "old"
}).Value(&res)
// len(res) == 2 && ok == true
```

<a name="clone" />

### Clone()

__Return__

* interface{}

__Examples__

```go
arr := []int{1, 2, 3}
duplicate := Clone(arr)
ok := All(duplicate, func(n, i int) bool {
	return arr[i] == n
})
// ok == true
```

<a name="each" />

### Each(source, iterator)

__Arguments__

* `source` - array or map
* `iterator` - func(element or value, index or key)

__Examples__

```go
arr := []TestModel{
	TestModel{1, "one"},
	TestModel{1, "two"},
	TestModel{1, "three"},
}
Each(arr, func (r TestModel, i int) {
	// coding
})
```

<a name="find" />

### Find(source, predicate, match)

__Arguments__

* `source` - array or map
* `predicate` - func(element or value, index or key) bool
* `match` - result

__Examples__

```go
arr := []TestModel{
	TestModel{1, "one"},
	TestModel{2, "two"},
	TestModel{3, "three"},
}
var item TestModel
Find(arr, func(r TestModel, _ int) bool {
	return r.Id == 1
}, &item)
// item == arr[0]
```

<a name="findBy" />

### FindBy(source, properties, match)

__Arguments__

* `source` - array or map
* `properties` - map[string]interface{}
* `match` - result

__Examples__

```go
arr := []TestModel{
	TestModel{1, "one"},
	TestModel{2, "two"},
	TestModel{3, "three"},
}
var item TestModel
FindBy(arr, map[string]interface{}{
	"id": 2,
}, &item)
// item == arr[1]
```

<a name="findIndex" />

### FindIndex(source, predicate)

__Arguments__

* `source` - array or map
* `predicate` - func(element or value, index or key) bool

__Return__

* int - index

__Examples__

```go
arr := []TestModel{
	TestModel{1, "one"},
	TestModel{1, "two"},
	TestModel{1, "three"},
}
i := FindIndex(arr, func(r TestModel, _ int) bool {
	return r.Name == arr[1].Name
})
// i == 1
```

<a name="findIndexBy" />

### FindIndexBy(source, properties)

__Arguments__

* `source` - array or map
* `properties` - map[string]interface{}

__Return__

* int - index

__Examples__

```go
arr := []TestModel{
	TestModel{1, "one"},
	TestModel{2, "two"},
	TestModel{3, "three"},
}
i := FindIndexBy(arr, map[string]interface{}{
	"id": 1,
})
// i == 0
```

<a name="first" />

### First(source, first)

__Arguments__

* `source` - array or map
* `first` - first element of `source`

__Examples__

```go
arr := []int{ 1, 2, 3 }
var n int
First(arr, &n)
if n != 1 {
	//wrong
}
```

<a name="group" />

### Group(source, keySelector, result)

__Arguments__

* `source` - array or map
* `keySelector` - func(element or value, index or key) anyType
* `result` - map[anyType][](element or value)

__Examples__

```go
dic := make(map[string][]int)
Group([]int{1, 2, 3, 4, 5}, func(n, _ int) string {
	if n%2 == 0 {
		return "even"
	}
	return "odd"
}, &dic)
if len(dic["even"]) != 2 {
	t.Error("wrong")
}
```

<a name="groupBy" />

### GroupBy(source, property, result)

__Arguments__

* `source` - array or map
* `property` - property name
* `result` - map[property type][](element or value)

__Examples__

```go
arr := []TestModel{
	TestModel{ID: 1, Name: "a"},
	TestModel{ID: 2, Name: "a"},
	TestModel{ID: 3, Name: "b"},
	TestModel{ID: 4, Name: "b"},
}
dic := make(map[string][]TestModel)
GroupBy(arr, "name", &dic)
if len(dic) != 2 {
	t.Error("wrong")
}
```

<a name="index" />

### Index(source, indexSelector, result)

__Arguments__

* `source` - array or map
* `indexSelector` - func(element or value, index or key) anyType
* `result` - map[anyType](element or value)

__Examples__

```go
res := make(map[string]string)
Index([]string{"a", "b"}, func(r string, _ int) string {
	return r
}, &res)
if res["a"] != "a" {
	// error
}
```

<a name="indexBy" />

### IndexBy(source, property, result)

__Arguments__

* `source` - array or map
* `property` - string
* `result` - map[propertyType](element or value)

__Examples__

```go
arr := []TestModel{
	TestModel{ID: 1, Name: "a"},
	TestModel{ID: 2, Name: "a"},
	TestModel{ID: 3, Name: "b"},
	TestModel{ID: 4, Name: "b"},
}
res := make(map[string]TestModel)
IndexBy(arr, "Name", &res)
if len(res) != 2 {
	// error
}
```

<a name="isArray" />

### IsArray(element)

__Arguments__

* `element` - object

__Return__

* bool

__Examples__

```go
if !IsArray([]int{}) {
	// wrong
}

if IsArray(map[string]int{}) {
	// wrong
}
```

<a name="isMatch" />

### IsMatch(element, properties)

__Arguments__

* `element` - object
* `properties` - map[string]interface{}

__Return__

* bool

__Examples__

```go
m := TestModel{ 1, "one" }
ok := IsMatch(nil, nil)
if ok {
	// wrong
}

ok = IsMatch(m, nil)
if ok {
	// wrong
}

ok = IsMatch(m, map[string]interface{}{
	"id": m.Id,
	"name": "a",
})
if ok {
	// wrong
}

ok = IsMatch(m, map[string]interface{}{
	"id": m.Id,
	"name": m.Name,
})
if !ok {
	// wrong
}
```

<a name="keys" />

### Keys(keys)

__Arguments__

* `source` - map
* `keys` - []keyType

__Examples__

```go
dict := map[int]string{
	1: "a",
	2: "b",
	3: "c",
	4: "d",
}
res := make([]int, 0)
Keys(dict, &res)
if len(res) != len(dict) {
	// wrong
}
```

<a name="last" />

### Last(source, last)

__Arguments__

* `source` - array or map
* `last` - last element of `source`

__Examples__

```go
arr := []int{1, 2, 3}
var n int
Last(arr, &n)
if n != 3 {
	// wrong
}

dict := map[string]string{
	"a": "aa",
	"b": "bb",
}
var str string
Last(dict, &str)
if !(str == "aa" || str == "bb") {
	// wrong
}
```

<a name="map" />

### Map(source, selector, result)

__Arguments__

* `source` - array or map
* `selector` - func(element, index or key) anyType
* `result` - an array of anyType

__Examples__

```go
arr := []string{"11", "12", "13"}
res := make([]int, 0)
Map(arr, func(s string, _ int) int {
	n, _ := strconv.Atoi(s)
	return n
}, &res)
if len(res) != len(arr) {
	// wrong
}
```

<a name="mapBy" />

### MapBy(source, property, result)

__Arguments__

* `source` - array
* `property` - string
* `result` - an array of property type

__Examples__

```go
arr := []TestModel{
	TestModel{ID: 1, Name: "one"},
	TestModel{ID: 2, Name: "two"},
	TestModel{ID: 3, Name: "three"},
}
res := make([]string, 0)
MapBy(arr, "name", &res)
if len(res) != len(arr) {
	// wrong
}

for i := 0; i < 3; i++ {
	if res[i] != arr[i].Name {
		// wrong
	}
}
```

<a name="md5" />

### Md5(plaintext)

__Arguments__

* `plaintext` - string

__Return__

* string - md5 string

__Examples__

```go
if Md5("123456") != "e10adc3949ba59abbe56e057f20f883e" {
	// wrong
}	
```

<a name="object" />

### Object(arr, result)

__Arguments__

* `arr` - array
* `result` - map

__Examples__

```go
arr := []interface{}{
	[]interface{}{"a", 1},
	[]interface{}{"b", 2},
}
dic := make(map[string]int)
Object(arr, &dic)
if len(dic) != 2 {
	// wrong
}

if v, ok := dic["a"]; !(ok && v == 1) {
	// wrong
}

if v, ok := dic["b"]; !(ok && v == 2) {
	// wrong
}
```

<a name="property" />

### Property(name)

__Arguments__

* `name` - property name

__Return__

* func(interface{}) (interface{}, error)

__Examples__

```go
item := TestModel{ 1, "one" }

getAge := Property("age")
_, err := getAge(item)
if err == nil {
	// wrong
}

getName := Property("name")
name, err := getName(item)
if !(err == nil && name.(string) == item.Name) {
	// wrong
}
```

<a name="propertyRV" />

### Property(name)

__Arguments__

* `name` - property name

__Return__

* func(interface{}) (reflect.Value, error)

__Examples__

```go
item := TestModel{ 1, "one" }

getAgeRV := PropertyRV("age")
_, err := getAgeRV(item)
if err == nil {
	// wrong
}

getNameRV := PropertyRV("name")
nameRV, err := getNameRV(item)
if !(err == nil && nameRV.String() == item.Name) {
	// wrong
}
```

<a name="range" />

### Range(start, stop, step)

__Arguments__

* `start` - int
* `stop` - int
* `step` - int

__Return__

* IQuery - a wrapped object, wrapped objects until value is called

__Examples__

```go
arr := make([]int, 0)
Range(0, 0, 1).Value(&arr)
if len(arr) != 0 {
	// wrong
}

Range(0, 10, 0).Value(&arr)
if len(arr) != 0 {
	// wrong
}

Range(10, 0, 1).Value(&arr)
if len(arr) != 0 {
	// wrong
}

Range(0, 2, 1).Value(&arr)
if !(len(arr) == 2 && arr[0] == 0 && arr[1] == 1) {
	// wrong
}

Range(0, 3, 2).Value(&arr)
if !(len(arr) == 2 && arr[0] == 0 && arr[1] == 2) {
	// wrong
}
```

<a name="reduce" />

### Reduce(source, iterator)

__Arguments__

* `source` - array
* `iterator` - func(memo, element or value, key or index) memo
* `memo` - anyType

__Return__

* interface{} - memo

__Examples__

```go
v := Reduce([]int{ 1, 2 }, func (memo []int, n, _ int) []int {
	memo = append(memo, n)
	memo = append(memo, n + 10)
	return memo
}, make([]int, 0))
res, ok := v.([]int)
if !(ok && len(res) == 4) {
	// wrong
}

if !(res[0] == 1 && res[1] == 11 && res[2] == 2 && res[3] == 12) {
	// wrong
}
```

<a name="reject" />

### Reject(source, predicate, result)

__Arguments__

* `source` - array or map
* `predicate` - func(element or value, index or key) bool
* `result` - an array of all the values that without pass a truth test `predicate`

__Examples__

```go
arr := []int{ 1, 2, 3, 4 }
v := Reject(arr, func (n, i int) bool {
	return n % 2 == 0
})
res, ok := v.([]int)
if !(ok && len(res) == 2) {
	// wrong
}

if !(res[0] == 1 && res[1] == 3) {
	// wrong
}
```

<a name="rejectBy" />

### RejectBy(source, properties, result)

__Arguments__

* `source` - array or map
* `properties` - map[string]interface{}
* `result` - an array of all the values that without pass a truth test `properties`

__Examples__

```go
arr := []TestModel{
	TestModel{ 1, "one" },
	TestModel{ 2, "two" },
	TestModel{ 3, "three" },
}
v := RejectBy(arr, map[string]interface{}{
	"Id": 1,
})
res, ok := v.([]TestModel)
if !(ok && len(res) == 2) {
	// wrong
}
```

<a name="reverse" />

### Reverse(source, selector, result)

__Arguments__

* `source` - array or map
* `selector` - func(element, key or index) anyType
* `result` - an array of `source` that reversed

__Examples__

```go
arr := []TestModel{
	TestModel{ID: 2, Name: "two"},
	TestModel{ID: 1, Name: "one"},
	TestModel{ID: 3, Name: "three"},
}
res := make([]TestModel, 0)
Reverse(arr, func(n TestModel, _ int) int {
	return n.ID
}, &res)
if len(res) != len(arr) {
	// error
}

if !(res[0].ID == 3 && res[1].ID == 2 && res[2].ID == 1) {
	// error
}
```

<a name="reverseBy" />

### ReverseBy(source, selector, result)

__Arguments__

* `source` - array or map
* `property` - string
* `result` - an array of `source` that reversed

__Examples__

```go
arr := []TestModel{
	TestModel{ID: 2, Name: "two"},
	TestModel{ID: 1, Name: "one"},
	TestModel{ID: 3, Name: "three"},
}
res := make([]TestModel, 0)
ReverseBy(arr, "id", &res)
if len(res) != len(arr) {
	// error
}

if !(res[0].ID == 3 && res[1].ID == 2 && res[2].ID == 1) {
	// error
}
```

<a name="size" />

### Size(source)

__Arguments__

* `source` - array or map

__Return__

* int

__Examples__

```go
dict := map[string]int{
	"a": 1,
	"b": 2,
	"c": 3,
}
if Size(dict) != len(dict) {
	// wrong
}
```

<a name="sort" />

### Sort(source, selector, result)

__Arguments__

* `source` - array or map
* `selector` - func(element, key or index) anyType
* `result` - an array of `source` that sorted

__Examples__

```go
arr := []TestModel{
	TestModel{ID: 2, Name: "two"},
	TestModel{ID: 1, Name: "one"},
	TestModel{ID: 3, Name: "three"},
}
res := make([]TestModel, 0)
Sort(arr, func(n TestModel, _ int) int {
	return n.ID
}, &res)
if len(res) != len(arr) {
	// error
}

if !(res[0].ID == 1 && res[1].ID == 2 && res[2].ID == 3) {
	// error
}
```

<a name="sortBy" />

### SortBy(source, property, result)

__Arguments__

* `source` - array or map
* `property` - string
* `result` - an array of `source` that sorted

__Examples__

```go
arr := []TestModel{
	TestModel{ID: 2, Name: "two"},
	TestModel{ID: 1, Name: "one"},
	TestModel{ID: 3, Name: "three"},
}
res := make([]TestModel, 0)
SortBy(arr, "id", &res)
if len(res) != len(arr) {
	// error
}

if !(res[0].ID < res[1].ID && res[1].ID < res[2].ID) {
	// error
}
```

<a name="take" />

### Take(source, count, result)

__Arguments__

* `source` - array or map
* `count` - int
* `result` - array

__Examples__

```go
arr := []int{1, 2, 3}
res := make([]int, 0)
Take(arr, 1, &res)
if len(res) != 1 || res[0] != 1 {
	// wrong
}
```

<a name="uniq" />

### Uniq(source, selector)

__Arguments__

* `source` - array
* `selector` - nil or func(element or value, index or key) anyType

__Return__

* interface{} - only the first occurence of each value is kept

__Examples__

```go
v := Uniq([]int{ 1, 2, 1, 4, 1, 3 }, func (n, _ int) int {
	return n % 2
})
res, ok := v.([]int)
if !(ok && len(res) == 2) {
	// wrong
}
```

<a name="uniqBy" />

### UniqBy(source, property)

__Arguments__

* `source` - array
* `property` - string

__Return__

* interface{}

__Examples__

```go
arr := []TestModel{
	TestModel{ 1, "one" },
	TestModel{ 2, "one" },
	TestModel{ 3, "one" },
}
v := UniqBy(arr, "Name")
res, ok := v.([]TestModel)
if !(ok && len(res) == 1) {
	// wrong
}
```

<a name="uuid" />

### UUID()

__Return__

* string - uuid string

__Examples__

```go
uuid := UUID()
//1a40272540e57d1c80e7b06042219d0c
```

<a name="value" />

### Value()

__Return__

* interface{} - Chain final result

__Examples__

```go
res := make(map[string][]int)
Chain([]int{1, 2, 1, 4, 1, 3}).Uniq(nil).Group(func(n, _ int) string {
	if n%2 == 0 {
		return "even"
	}

	return "old"
}).Value(&res)
if len(res) == 2 {
	// wrong
}
```

<a name="values" />

### Values(source, values)

__Arguments__

* `source` - map
* `values` - an array of `source`'s values

__Examples__

```go
dict := map[int]string{
	1: "a",
	2: "b",
	3: "c",
	4: "d",
}
res := make([]string, 0)
Values(dict, &res)
if len(res) != len(dict) {
	// wrong
}
```

<a name="where" />

### Where(source, predicate, result)

__Arguments__

* `source` - array or map
* `predicate` - func(element or value, index or key) bool
* `result` - an array of all the values that pass a truth test `predicate`

__Examples__

```go
arr := []TestModel{
	TestModel{ID: 1, Name: "one"},
	TestModel{ID: 2, Name: "two"},
	TestModel{ID: 3, Name: "three"},
	TestModel{ID: 4, Name: "three"},
}
res := make([]TestModel, 0)
Where(arr, func(r TestModel, i int) bool {
	return r.ID%2 == 0
}, &res)
if !(len(res) == 2 && res[0].ID == 2 && res[1].ID == 4) {
	// wrong
}
```

<a name="whereBy" />

### WhereBy(source, properties, result)

__Arguments__

* `source` - array or map
* `properties` - map[string]interface{}
* `result` - an array of all the values that pass a truth test `properties`

__Examples__

```go
arr := []TestModel{
	TestModel{ID: 1, Name: "one"},
	TestModel{ID: 2, Name: "one"},
	TestModel{ID: 3, Name: "three"},
	TestModel{ID: 4, Name: "three"},
}
res := make([]TestModel, 0)
WhereBy(arr, map[string]interface{}{
	"Name": "one",
}, &res)
if !(len(res) == 2 && res[0] == arr[0] && res[1] == arr[1]) {
	// wrong
}
```