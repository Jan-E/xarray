xarray
========================================================

The missing array functions you are looking for.


[![Build Status](https://travis-ci.org/c9s/xarray.svg)](https://travis-ci.org/c9s/xarray)


## Requirement

- PHP 5.5.x
- PHP 5.6.x

## Installation

    phpize
    ./configure
    make install

## Functions

### array\_is\_indexed

```php
bool array_is_indexed(array $array)
```

`array_is_indexed` function returns true when all the keys in the given array are
long integer keys. (0, 1, 3, 100).


### array\_is\_assoc

```php
bool array_is_assoc(array $array)
```

`array_is_assoc` function returns true when all the keys in the given array are
string type keys. ('foo', 'bar').


### array\_keys\_join

```php
string array_keys_join(array $array, [string $delim])
```

`array_keys_join` is useful when generating cache keys from array keys, this
reduces the overhead of using `join('-', array_keys($array));`.

##### Examples

```php
echo array_keys_join([ "foo" => 1, "bar" => 123], "-");
echo array_keys_join([ "a" => 1, "b" => 123, "c" => "kkk" ], "-");
```

Which outputs:

```
foo-bar
a-b-c
```

### array\_pluck

```php
string array_pluck(array $array, [long $key | string $key])
```

`array_pluck` is used for extracting element from array inside the given array.

##### Examples

Extracting element by string key:


```php
$a = array(
    array( 'name' => "John", 'phone' => '123456' ),
    array( 'name' => "Mary", 'phone' => '123457' ),
    array( 'name' => "Lisa", 'phone' => '123458' ),
);
$a = array_pluck($a, 'name');
print_r($a);
```

Outputs:

```
Array
(
    [0] => John
    [1] => Mary
    [2] => Lisa
)
```

Extracting element by index key:

```php
$a = array(
    array(2 => "John"),
    array(2 => "Mary"),
    array(2 => "Lisa"),
);
$a = array_pluck($a, 2);
print_r($a);
```

Outputs:

```
Array
(
    [0] => John
    [1] => Mary
    [2] => Lisa
)
```



### array\_first

```php
string array_first(array $array, callable $callable)
```

`array_first` returns the first element in an array passing a given truth test.

##### Examples

```php
$a = array(
    0 => "John",
    1 => "Mary",
    2 => "Lisa",
);
$a = array_first($a, function($key, $value) {
    if ($key !== 1) {
        return true;
    }
    return false;
});
print_r($a);
```

Outputs:

```
Array
(
    [0] => John
    [1] => Lisa
)
```



### array\_each

```php
string array_each(array $array, callable $builder)
```

`array_each` iterates the array keys and values through a callback.

##### Examples

```php
$a = array(
    0 => "John",
    1 => "Mary",
    2 => "Lisa",
);
$b = [];
$a = array_each($a, function($key, $value) use(&$b) {
    $b[$key] = $value;
});
print_r($b);
```

The example above will output:

```
Array
(
    [0] => John
    [1] => Mary
    [2] => Lisa
)
```










### array\_build

```php
string array_build(array $array, callable $builder)
```

`array_build` builds the new array with new keys and new values. 

##### Examples

```php
$a = array(
    array("name" => "John", "id" => "j"),
    array("name" => "Mary", "id" => "m"),
    array("name" => "Lisa", "id" => "l"),
);
$b = array();
array_build($a, function($key, $value) use(&$b) {
    return array($value['id'], $value['name']);
});
print_r($b);
```

The example above will output:

```
Array
(
    [j] => John
    [m] => Mary
    [l] => Lisa
)
```




### array\_key\_prefix

```php
string array_keys_prefix(array $array, string $prefix)
```

`array_keys_prefix` returns a new array by prepending prefix string from the original keys.

##### Examples

```php
$a = array(
    "name" => "John",
    "id" => "j",
    "phone" => "+886975123123",
);
$b = array_keys_prefix($a, "my_");
print_r($b);
```

The example above will output:

```
Array
(
    [my_nam] => John
    [my_i] => j
    [my_phon] => +886975123123
)
```




### array\_add

```php
string array_add(array $array, string|long $key, $elemnt)
```

`array_add` add the new element into the given array only when the key is not set.

##### Examples

```php
$a = array(
    "foo" => "a",
    "zoo" => "c",
);
array_add($a, "bar", "b");
print_r($a);
```

The example above will output:

```
Array
(
    [foo] => a
    [zoo] => c
    [bar] => b
)
```





### array\_remove

```php
string array_remove(array $array, callable $callback)
```

`array_remove` remove the element from the given array by the callback.

##### Examples

```php
$a = array(
    "foo" => "a",
    "bar" => "b",
    "zoo" => "c",
);
// delete "foo" key from $a
array_remove($a, function($key, $value) {
    return $value === "b";
});
print_r($a);
```

The example above will output:

```
Array
(
    [foo] => a
    [zoo] => c
)
```
















## FAQ

Q: Why `array_keys_join` and `array_keys_prefix` are named with `keys` in plural.

A: The `keys` means the operation works on multiple keys, not just one of them.

