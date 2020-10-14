# Ruby Versioning and Rspec

Ruby versions
Shows the versions 

Rbenv install version_number
Rbenv local sets the folder to that version

Gem - a standard of how we share ruby code in packages

Bundler provides a way to manage your gems
Projects can have multiple dependencies which become difficult to manage manually

Gem install rspec
Gem update rspec

Rspec —version
Rbenv rehash - tell rbenv to relink gems we’ve downloaded
Rspec —help

   
### Rspec Configuration

—color —no-color
—format progress. —format documentation. -f d/p shorthand
Progress shows dots and Fs as tests are completed, documentation gives a list of all the test names
—no-profile  —profile	gives info about the tests
—no-fail-fast. —fail-fast 
Fail-fast stops running tests as soon as it finds a failure
Whether it reports failures as soon as it encounters them or wait until end to display all
—order defined. —order random
Determines the order the tests are run

Rspec —color

### Default config
Global goes in user’s home directory - applies for all rspec files running on your computer
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

expect().to()
expect().to_not()
expect(@name).to eq(‘Kevin’)
expect(@name).to match(/K.v.n/)
expect(@visible)to be(true)
expect(@numbers).to match_array([4,5,2])
Expectations are simple statements
Matchers are used as arguments to to()


### Test doubles

An object that stands in or another object - like a body double for an actor
Used when the real object is difficult or expensive(e.g. responses are slow, alters a database) to work with
Having fixed, expected responses makes testing easier
Examples - sending out an email, want to test the act but not actually send an email

Double/mock: a simple object preprogrammed with expectations and responses as preparation for the calls it will receive

Stub: an instruction to an object to return a specific response to a method call

#### Double and stub examples

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

When multiple responses, if extra expect statements it will continue returning the last argument
