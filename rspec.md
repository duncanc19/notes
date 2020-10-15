# Ruby Versioning and Rspec

```ruby
ruby versions
```
Shows the versions 

```ruby
rbenv install version_number
rbenv local # sets the folder to that version
```

Gem - a standard of how we share ruby code in packages

Bundler provides a way to manage your gems
Projects can have multiple dependencies which become difficult to manage manually

```ruby
gem install rspec
gem update rspec

rspec —version
rbenv rehash # tell rbenv to relink gems we’ve downloaded
rspec —help
```
   
### Rspec Configuration

Configuration flags you can use, you can run them in the terminal or put them in your default config files

```ruby
--color —no-color
--format progress. --format documentation. -f d/p shorthand
Progress shows dots and Fs as tests are completed, documentation gives a list of all the test names
--no-profile  --profile	gives info about the tests
--no-fail-fast. --fail-fast 
# Fail-fast stops running tests as soon as it finds a failure - whether it reports failures as soon as it encounters them or wait until end to display all
--order defined. --order random
# Determines the order the tests are run

rspec --color
```



### Default config

Global 
goes in user’s home directory - applies for all rspec files running on your computer
~/.rspec

Project
./.rspec in the project’s root directory
Shared in source control(in git) so everyone can have the same configuration

Local lets you have your own config - ignored by source control
./.rspec-local 

### Initialise rspec

Rspec --init
Creates two files - the .rspec file(which is the project version as it’s in your project’s root directory) and the spec_helper.rb file

In lib directory -  car.rb
In spec directory - car_spec.rb

In spec file - require ‘car’

describe
You can have a top level describe for the name of the class - then it’s good to have a describe block for each method - although down to personal preference

You can use context too, which is an alias for describe, but is often used to group contexts - e.g. all tests relate to a user whose billing is overdue

### Rspec hierarchy

Spec file		car_spec.rb
  Example group	describe
    Nested group	  describe/context
      Example		    it
	expectations	      expect().to()

### Rspec Naming Convention

Class method .method_name
Method #method_name

When running rspec, can give a line number to run just test on that line, e.g.
rspec spec/car_spec.rb:7

### Pending and skipping examples
#### Pending - shows as asterisk when spec run

omit the block - don’t write do end
Use pending inside the block
When test is pending, code is still run, if it passes, it shows up as failing to make it clear it doesn’t need to be pending

#### Skipping

Use xdescribe or xit

Use skip inside the block - can take argument

skip(“”)


### Expectations

One expectation per example, with more than one in an it block, only the first failure will come up
```ruby
expect().to()
expect().to_not()
expect(@name).to eq(‘Kevin’)
expect(@name).to match(/K.v.n/)
expect(@visible)to be(true)
expect(@numbers).to match_array([4,5,2])
Expectations are simple statements
Matchers are used as arguments to to()
```

#### Equivalence Matchers

```ruby
.to eq() # loose comparison
.to eql() # strict comparison
.to equal() # reference comparison
.to be() # same as to equal
```

#### Truthiness Matchers

```ruby
expect(1 < 2).to be true
expect(1 < 2).to be(true) # same as above
expect(1 > 2).to be(false)

expect('some string').to be_truthy
expect(nil).to be_falsey
expect(0).not_to be_falsey	# in ruby, 0 evaluates to true - different from some other languages
expect(nil).to be_nil
expect(false).not_to be_nil
```

#### Numeric Comparison Matchers

```ruby
expect(100).to eq(100)		expect(100).to be > 99
expect(100).to be < 101		expect(100).to be >= 99
expect(100).to be <= 100	expect(5).to be_between(3,5).inclusive

expect(5).not_to be_between(3,5).exclusive
expect(100).to be_within(5).of(105)
expect(1..10).to cover(3)
```

### Test doubles

An object that stands in or another object - like a body double for an actor
Used when the real object is difficult or expensive(e.g. responses are slow, alters a database) to work with
Having fixed, expected responses makes testing easier
Examples - sending out an email, want to test the act but not actually send an email

Double/mock: a simple object preprogrammed with expectations and responses as preparation for the calls it will receive

Stub: an instruction to an object to return a specific response to a method call

#### Double and stub examples

```ruby
it “allows setting responses” do
  dbl = double(“Chant”)
  allow(dbl).to receive(:hey!) {“Ho!”}
  expect(dbl.hey!).to eq(“Ho!”)
end

it “allows setting responses” do
  dbl = double(“Chant”)
  allow(dbl).to receive(:hey!).and_return(“Ho!”)
  expect(dbl.hey!).to eq(“Ho!”)
end

To receive_messages in general not recommended because it has some limitations
it “allows setting responses” do
  dbl = double(“Person”)
  allow(dbl).to receive_messages(
     :full_name => “Mary Smith”,
     :initials => “MTS”
  )
  expect(dbl.full_name).to eq(“Mary Smith”)
  expect(dbl.initials).to eq(“MTS”)
end

Better to put hash in the double arguments, sets up allow statements for you for each hash key
it “allows setting responses” do
  dbl = double(“Person”, :full_name => “Mary Smith”,
     :initials => “MTS”)
  expect(dbl.full_name).to eq(“Mary Smith”)
  expect(dbl.initials).to eq(“MTS”)
end

it “allows setting multiple responses” do
  die = double(“Die”)
  allow(die).to receive(:roll).and_return(1,5,2,6)
  expect(die.roll).to eq(1)
  expect(die.roll).to eq(5)
  expect(die.roll).to eq(2)
  expect(die.roll).to eq(6)
  expect(die.roll).to eq(6)
end
```

When multiple responses, if extra expect statements it will continue returning the last argument

### Partial Test Doubles

You can test a regular object but add some features with a test double

You can stub a value over an expected value 

The first example shows that the underlying functionality still exists, you can see it in to_s, but for this one case when you call the year, it will now return 1975.

The second example shows even if you set a variable in the test, if you put a stub over it using the allow statement, it will return the stub value.

The third shows a class method.

```ruby
it "stubs instance methods on real objects" do
  time = Time.new(2010, 1, 1, 12, 0, 0)
  allow(time).to receive(:year).and_return(1975)
  
  expect(time.to_s).to eq('2010-01-01 12:00:00 -5000')
  expect(time.year).to eq(1975)
  
it "stubs instance methods on real objects" do
  hero = SuperHero.new
  hero.name = 'Superman'
  
  allow(hero).to receive(:name).and_return('Clark Kent')
  expect(hero.name).to eq('Clark Kent')
end

it "stubs class methods on real objects" do
  fixed = Time.new(2010,1,1,12)
  allow(Time).to receive(:now).and_return(fixed)
  expect(Time.now.year).to eq(2010)
end
```

This can be very useful for database calls or sending messages
```ruby
it "can stub database calls" do
  dbl = double('Mock Customer')
  allow(dbl).to receive(:name).and_return('Bob')
  allow(Customer).to receive(:find).and_return(dbl)
  
  customer = Customer.find
  expect(customer.name).to eq('Bob')
end
```

### Message Expectations

With allow, when that method is called it will pass out whatever you tell it to, but it's not obliged to do it, if the method isn't called in the test it isn't a problem.

If you use a message expectation, it will only pass if the method is used. Without the last line the test would fail, whereas with allow it wouldn't be a problem. 

```ruby
it "expects a call and allows a response"
  dbl = double("Chant")
  expect(dbl).to receive(:hey!).and_return("Ho!")
  dbl.hey!
end
```
Order is not important if there's more than one expect, as long as they're both called, but you can specify it using ordered:

```ruby
it "works with #ordered when order matters" do
  dbl = double("Multi_step")
  expect(dbl).to receive(:step1).ordered
  expect(dbl).to receive(:step2).ordered
  
  dbl.step1
  dbl.step2
end
```

### Message Argument Constraints

The default when you use expect or allow for a method is .with(any_args)- a matcher for any arguments passed in

If you want to pass in an argument you use .with

```ruby
it "allows constraints on arguments" do
  dbl = double('Customer List')
  expect(dbl).to receive(:sort).with('name')
  dbl.sort('name')
end
```

Can also use:
```ruby
with(no_args)
with ('Rspec', anything)
with(boolean)
with(hash_including(:verbose => true)
with(array_including(3))
```


### Message Count Constraints

Constraints on how many times a method will be called.

```ruby
once
twice
exactly(n).times

at_least(:once)
at_least(n).times

at_most(:twice)

expect(post).to receive(:like).at_least(3).times
```

### Spies

With doubles, you have to set up the expectation before you call the method, telling the mock object to be ready for a request.

With spies, they keep track of the messages received so you can look back at them after.

Spies are looser than other mock objects - you don't have to stub methods to make them available(using allow). They will let you send in any message without raising any objection. 

```ruby
# Double
it “allows setting responses” do
  dbl = double(“Chant”)
  allow(dbl).to receive(:hey!) {“Ho!”}
  expect(dbl.hey!).to eq(“Ho!”)
end

# Spy
it “expects a call after it is received” do
  dbl = spy(“Chant”)
  # allow(dbl).to receive(:hey!).and_return(“Ho!”) you don't have to stub it
  dbl.hey!
  dbl.hey!
  expect(dbl).to have_received(:hey!).twice
end
```

If you want to use to have_received with a partial test double(on a real object), you have to use an allow statement.

```ruby
it "expects a call after it is received" do
  customer = Customer.new
  allow(customer).to receive(:send_invoice)
  customer.send_invoice
  expect(customer).to have_received(:send_invoice)
```

With spies, the focus is on checking that a method has been called rather than what it returns
