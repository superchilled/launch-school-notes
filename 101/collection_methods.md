# Collection Methods

1. each can be used to iterate instead of using for and while loops
2. there are other iteration methods for specific purposes
..1. select or reject can identify items based on a condition
..2. detect can be used to find a specific item in a collection
..3. collect can build an array of results from logic in the block (map is equivalent to collect)
..4. inject can accumulate total or concatenate values together
3. sort and sort_by can be used to sort colections using logic in a block
4. any? can be used to see if  any objects in the collection meet the criteria 
5. all? can be used to see ifall objects in the collection meet the criteria
6. we can use collection methods on strings if we first convert them to an array

### ITERATION

#### Example 1: Iterating an array with each

```ruby
my_array = [1, 2, 3, 4, 5]

my_array.each { |number| puts number } # outputs each value in the array in order and returns the array
```

#### Example 2: Iterating a hash with each

```ruby
my_hash = {1 => 1, 2 => 2, 3 => 3, 4 => 4, 5 => 5}

my_hash.each { |_, number| puts number } # outputs each value in the hash in order and returns the hash
```

#### Example 3: iterating a string with each (we have to use split first to turn it into an array)
```ruby
my_string = 'Lorem ipsum dolor sit amet'

my_string.split.each { |word| puts word } # will output each word in the string and return an array of the words
```
### SEARCHING

#### Example 4: using select on an array
Select returns a new array containing items from the original array which  the block is true
```ruby
my_array.select { |number| number > 2 } # => [3, 4, 5] 
```
#### Example 5: using select on a hash
Select returns a new hash containing items from the original hash which match the criteria in the block
```ruby
my_hash.select { |_, number| number > 2 } # => {3=>3, 4=>4, 5=>5}
```
#### Example 6: using reject on an array
Reject returns a new array containing items from the original array for which the block is false
It's basically the opposite of select
```ruby
my_array.reject { |number| number > 2 } # => [1, 2] 
```
#### Example 7: using reject on a hash
Reject returns a new hash containing items from the original hash for which the block is false
```ruby
my_hash.reject { |_, number| number > 2 } # => {1=>1, 2=>2}
```

#### Example 8: using detect on an array
Detect returns the first object in the array for which the blockis not false. If no object matches returns nil

```ruby
my_array.detect { |number| number > 2 } # => 3
```
#### Example 9: using detect on a hash
Detect returns the first object in the hash as a new array for which the block is not false.
If no object matches returns nil
```ruby
my_hash.detect { |_, number| number > 2 } # => [3, 3]
```
#### Example 10: Using collect on an array
Collect returns an new array containing the results of running the block on each object in the original array
```ruby
my_array.collect { |number| number + 1 } # => [2, 3, 4, 5, 6]
```
#### Example 11: Using collect on a hash
Using collect on a has also returns an array of objects run through the block
```ruby
my_hash.collect { |_, number| "number #{number}" } # => ["number 1", "number 2", "number 3", "number 4", "number 5"] 
```
#### Example 12: Using inject on an array
Inject combines objects from the original array using logic in the block and returns the combined value
```ruby
my_array.inject { |total, number| total + number } # => 15
```
### SORTING

#### Example 13: Using sort on an array
Sort returns a new array of objects from the orignal array sorted either by default or logic in a block
```ruby
my_array.sort { |a, b| b <=> a } # => [5, 4, 3, 2, 1]
```
#### Example 14: Using sort_by on an array
Sort by sorts by using a set of keys generated by mapping the values of the original array through the block
```ruby
my_array.sort_by { |number| number % 2 } # [2, 4, 1, 3, 5]
```
### ANY? and ALL?

#### Example 15: Using any? on an array
```ruby
my_array.any? { |num| num > 3 } # => true

my_array.any? { |num| num > 5 } # => false
```
#### Example 16: Using all? on an array
```ruby
my_array.all? { |num| num > 3 } # => false

my_array.all? { |num| num > 0 } # => true
```
#### Example 17: Using sort_by on a string
```ruby
my_string = "This is a string containing different length words"

my_string.split(" ").sort_by { |word| word.length }.join(" ") # => "a is This words string length different containing" 
```
