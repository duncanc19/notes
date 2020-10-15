# Ruby Notes

Uses snake_case

Ruby doesn’t care about whitespace, but convention is the block of code following an if etc should be indented two spaces.

### Comments

\# Single line comment

=begin
Multiline comment
=end


Initialise variable

my_num = 100

### Printing 

puts "Sup"
print "Hello" # doesn't start new line

### String methods

"I love espresso".length 
"Eric".reverse
"eric".upcase			# ERIC
puts "Eric".downcase 	# eric
"eric".capitalize		# Eric
text.split				# with no argument(delimiter), splits by any whitespace
the most common use of split is with no arguments; in this case, the default behavior is to split on whitespace (such as spaces, tabs, or newlines)
 "ant     bat\t\tcat\n    duck".split
=> ["ant", "bat", "cat", "duck"]

text.split(“,”)
“Hello”.include? “lo” 	# true
“Hello”.index(‘e)		# returns 1
“Hello”.index(/[aeiou]/, -3) # returns nil if not found, 2nd parameter gives where in string to begin search
s.replace “world” 

soliloquy[0] => "T"


name = "Dave"
puts name.downcase.reverse.upcase # EVAD

Get user input - gets

print "What's your first name? "
first_name = gets.chomp # get input from user

chomp
Ruby automatically adds a blank line (or newline) after each bit of input; chomp removes that extra line

String interpollation

monkey = "Curious George"
"I took #{monkey} to the zoo"
 \# "I took Curious George to the zoo"
 
! for auto assignment
 ! modifies the value contained within the variable itself
last_name = gets.chomp
last_name.capitalize! # changes last_name


### if elsif else

```ruby
if 2 > 1 
  print "It is!"
elsif 3>1
  print "That isn't but this is!"
else 
  print "No it's not!"
end
```

### unless

short hand if statement. It will do whatever you ask unless the condition is true

```ruby
hungry = false

unless hungry
  puts "I'm writing Ruby programs!"
else
  puts "Time to eat!"
end

problem = false
print "Good to go!" unless problem
```

### Comparison

```ruby
==
!=
< > <= >=
```

#### Checking for equality

```ruby
x = 'Lynda'      x == 'Lynda' # true checks for loose equality 

x = 1            x == 1 # true   x == 1.0. # true   x == "1" # false

.eql? # strict equality

x.eql?(1) # true    x.eql?(1.0) # false

.equal? # reference equality

x = 'Lynda'

x.equal?('Lynda') # false

# You can check references with x.object_id

```


Combined Comparison <=>

It returns 0 if the first operand (item to be compared) equals the second, 1 if the first operand is greater than the second, and -1 if the first operand is less than the second.

book_1 = "A Wrinkle in Time"
book_2 = "A Brief History of Time"

book_1 <=> book_2 # returns 1

def alphabetize(arr, rev=false)
  if rev
    arr.sort { |item1, item2| item2 <=> item1 }
  else
    arr.sort { |item1, item2| item1 <=> item2 }
  end
end



### Logical Operators
&& 
||
!

boolean_1 = (3 < 4 || false) && (false || true)
boolean_1 = true

#### ? methods

Ruby methods that end with ? evaluate to the boolean values true or false.)

.include? method evaluates to true if it finds what it’s looking for and false otherwise

if user_input.include? "s"
  user_input.gsub!(/s/, "th")
  
.gsub! method 
.gsub stands for global substitution.
uses regex

+=/-= operator

doesn't have ++/--


### Loops

#### while loop

counter = 1
while counter < 11
  puts counter
  counter = counter + 1
end

#### until loop

It’s sort of like a backward while

counter = 1
until counter > 10
  puts counter
  counter += 1
end


#### for loop

for num in 1...10 # exclude final number
  puts num
end

for num in 1..15 # include final number
  puts num
end

a = "honey badger"
a = a.split
for i in a do
    puts i
end


#### loop 

loop { print "Hello, world!" } # infinite loop

i = 0
loop do
  i += 1
  print "#{i}"
  break if i > 5	# break keyword 
end

#### next

The next keyword can be used to skip over certain steps in the loop. For instance, if we don’t want to print out the even numbers, we can write:

for i in 1..5
  next if i % 2 == 0
  print i
end


#### each iterator

object.each { |item| 
  \# Do something 
}

object.each do |item| 
  \# Do something 
end

The variable name between | | can be anything you like: it’s just a placeholder for each element of the object you’re using .each on.

odds = [1,3,5,7,9]

odds.each do |x|
  print x*2
end

#### .times iterator

10.times { print "Chunky bacon!" }

5.times do
  print "Blaaaa"
end




Curly brackets and do end

In Ruby, curly braces ({}) are generally interchangeable with the keywords do (to open the block) and end (to close it).

loop { print "Hello, world!" } # infinite loop

i = 0
loop do
  i += 1
  print "#{i}"
  break if i > 5
end


### Arrays

demo_array = [100, 200, 300, 400, 500]
print  demo_array[2]

2d arrays

my_2d_array = [[1,3],[5,7],["cat", 3]]

my_2d_array.each { |x| puts "#{x}\n" }

s = [["ham", "swiss"], ["turkey", "cheddar"], ["roast beef", "gruyere"]]

s.each do |sub_array| 
  sub_array.each do |food|
    puts food
  end
end

a = [“badger", 42, true]

a[-2] 	# 42
a[-1]		# true
a.last 	# same as [-1]

#### Slice 

slice - provide two arguments, an index and a number of elements, which returns the given number of elements starting at the given index
a = [42, 8, 17, 99]
a.slice(2, 2)	 => [17, 99]
a.slice(1..3)	 => [8, 17, 99]

Directly slicing with bracket notation
a[2, 2]		=> [17, 99]
>> a[1..3]		=> [8, 17, 99]

#### To array

def array_of_10
  puts (1..10).to_a
end

(1..10).to_a		=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
('a'..'z').to_a		=> ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n", "o",
    "p", "q", "r", "s", "t", "u", "v", "w", "x", "y", "z"]


### Hashes

Doesn’t have dot notation

my_hash = { "name" => "Eric",
  "age" => 26,
  "hungry?" => true
}

puts my_hash["name"]

pets = Hash.new   	# create new empty hash

pets["Jaffa"] = "cat"	# add to hash
puts pets["Jaffa"]

Using each with hashes

friends.each { |x| puts "#{x}" }		# array
family.each { |x, y| puts "#{x}: #{y}" }	# hash

restaurant_menu = {
  "noodles" => 4,
  "soup" => 3,
  "salad" => 2
}

restaurant_menu.each do |item, price|
  puts "#{item}: #{price}"
end

Set up default value in hash

frequencies = Hash.new(0) # default value is 0, can be string/anything

Hash example - word frequencies

puts "Text please: "
text = gets.chomp

words = text.split(" ")
frequencies = Hash.new(0)
words.each { |word| frequencies[word] += 1 }
frequencies = frequencies.sort_by {|a, b| b }
frequencies.reverse!
frequencies.each { |word, frequency| puts word + " " + frequency.to_s }




#### Methods

No arguments

def puts_1_to_10
  (1..10).each { |i| puts i }
end

puts_1_to_10 # call method with no arguments

With arguments

def prime(n)
  puts "That's not an integer." unless n.is_a? Integer
  is_prime = true
  for i in 2..n-1
    if n % i == 0
      is_prime = false
    end
  end
  if is_prime
    puts "#{n} is prime!"
  else
    puts "#{n} is not prime."
  end
end

prime(97)

? for boolean methods

def by_five?(n)
  return n % 5 == 0
end 

#### Splat arguments  *
Asterisk after parameter means method can receive one or more arguments

def what_up(greeting, *friends)
  friends.each { |friend| puts "#{greeting}, #{friend}!" }
end

what_up("What up", "Ian", "Zoe", "Zenas", "Eleanor")


Check object type 

is_a? Type
e.g. n.is_a? Integer



Blocks

Similar to anonymous functions in JavaScript or lambdas in Python

1.times do
  puts "I'm a code block!"
end

1.times { puts "As am I!" }


#### Sorting

sort

Sorts chronologically for numbers and alphabetically for strings

my_array.sort!
books.sort!


sort_by

Can’t use exclamation mark for assignment

colors = { 
  "blue" => 3,
  "green" => 1,
  "red" => 2
}
colors = colors.sort_by do |color, count|
  count
end
colors.reverse!


#### Rspec 

rspec —format documentation 	# give long explanation of tests
rspec -h 		# help 

You can put preferences in .rspec file 
e.g. put in the format doc line and it will automatically run in that mode when running tests


1. rbenv shell
2. rbenv local
3. rbenv global

ruby -v
ruby 1.9.3p484 (...)

$ rbenv shell 2.0.0-p353
$ ruby -v
ruby 2.0.0p353

rbenv local 2.5.0
$ ruby -v
ruby 2.5.0
