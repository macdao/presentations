# Getting Started with Ruby



## a programming language


### Ruby at a Glance

- https://www.ruby-lang.org
- a programming language
- dynamic
- open source
- focus on simplicity and productivity
- has an elegant syntax that is natural to read and easy to write


- public release in 1995
- by Yukihiro "Matz" Matsumoto
- Stable release 2.2.2 / April 13, 2015


### Hello, Ruby!

```ruby
puts "Hello World!"
```


### irb
- Interactive Ruby Shell
- an interactive command-line interpreter

```bash
$ irb
irb(main):001:0> puts "Hello, World"
Hello, World
=> nil
irb(main):002:0> 1+2
=> 3
```


### Object-Oriented

everything is an object

```ruby
5.times { print "We *love* Ruby -- it's outrageous!" }
```


### Arrays and Hashes

```ruby
a = [ 1, 'cat', 3.14 ]

puts "The first element is #{a[0]}"

a = %w{ ant bee cat dog elk }

inst_section = {'cello' => 'string', 'clarinet' => 'woodwind'}
```


### Symbols

```ruby
inst_section = {:cello => 'string', :clarinet => 'woodwind'}

inst_section = {cello: 'string', clarinet: 'woodwind'}

inst_section['cello'] # => nil

puts "An oboe is a #{inst_section[:oboe]} instrument"
```


### Class

```ruby
class Person
  def initialize(name)
    @name = name
  end
end

puts Person.new('Xi Qi') # => #<Person:0x007fc623086218>
```


```ruby
class Person
  attr_reader :name

  def initialize(name)
    @name = name
   end
end

puts Person.new('Xi Qi').name => Xi Qi
```

``` ruby
attr_accessor :name
```


### Open Classes/Monkeypatch

> In Ruby, classes are never closed

```ruby
class Person
  def to_s
    "Person #{@name}"
  end
end

puts Person.new("Xi Qi") # => Person Xi Qi
```


### Inheritance

```ruby
class Programmer < Person
  def code
    "puts 'Hello, World'"
  end
end

p Programmer.superclass # => Person
p Person.superclass # => Object
```


### Object

an object simply contains

- its instance variables and
- a reference to its class


### Instance Variables

```ruby
class Programmer
  def sleep
    @eyes = 'closed'
  end
end

programmer = Programmer.new('Xi Qi')
p programmer.instance_variables # => [:@name]

programmer.sleep
p programmer.instance_variables # => [:@name, :@eyes]
```


### Module

### module methods

```ruby
module MyModule
  def self.meth
    "module methods"
  end
end

p MyModule.meth # => "module methods"
```


### Namespaces

```ruby
module MyModule
  MyConstant = 'Outer constant'

  class MyClass
    MyConstant = 'Inner constant'
  end
end

p MyModule::MyConstant # => "Outer constant"
p MyModule::MyClass::MyConstant # => "Inner constant"
p MyModule::MyClass.new # => #<MyModule::MyClass:0x007f881a0a34b8>
```


### Mixins

```ruby
module Debug
  def who_am_i?
    "#{self.class.name} (id: #{self.object_id}): #{self.name}"
  end
end

class Phonograph
  include Debug
  attr_reader :name
  def initialize(name)
    @name = name
  end
end

ph = Phonograph.new("West End Blues")
p ph.who_am_i? # => "Phonograph (id: 70166168000260): West End Blues"
```


### Blocks

```ruby
%w( ant bee cat dog ).each {|animal| puts animal }
```

```ruby
def call_block
  puts "Start of method"
  yield
  yield
  puts "End of method"
end

call_block { puts "In the block" }
```


### To Ruby From Java

https://www.ruby-lang.org/en/documentation/ruby-from-other-languages/to-ruby-from-java/



## Ruby's fellows


### RubyGems

- a Package Manager
- part of the standard library from Ruby version 1.9
- 100,106 gems hosted on RubyGems.org on Apr 20, 2015
- https://rubygems.org/stats


#### gem command

  - `$ gem search rspec`
  - `$ gem install rspec`
  - `$ gem list`
  - `$ gem list rspec -d`
  - `$ gem uninstall rspec`
  - `$ gem environment gemdir`


#### What is a gem?

Inside a gems are the following components:

- Code (including tests and supporting utilities)
- Documentation
- gemspec


Each gem follows the same standard structure of code organization:

```bash
% tree freewill
freewill/
├── bin/
│   └── freewill
├── lib/
│   └── freewill.rb
├── test/
│   └── freewill_spec.rb
├── README
├── Rakefile
└── freewill.gemspec
```


### Rake

- a Make-like program implemented in Ruby
- written in the Ruby programming language and use Ruby syntax
- part of the standard library from Ruby version 1.9 onward


#### rake command

- `$ rake`

  run the "default" task in the Rakefile
- `$ rake -T`

  Display a list of the major tasks and their comments
- `$ rake -P`

  Display a list of all tasks and their immediate prerequisites
- `$ rake name`
- `$ rake name[Xi,Qi]`


- `--rakelibdir rakelibdir (-R)`

  Auto-import any .rake files in RAKELIBDIR. (default is 'rakelib')


#### Rakefile

you must write a "Rakefile" file which contains the build rules


#### Simple Tasks

```ruby
task :name
```


#### Tasks with Prerequisites

```ruby
task :name => [:prereq1, :prereq2]
```


#### Simple Example

```ruby
task :default => [:test]

task :test do
  puts "Hello World!"
end
```


#### Tasks with Arguments

```ruby
task :name, [:first_name, :last_name] do |t, args|
  args.with_defaults(:first_name => "Xi", :last_name => "Qi")
  puts "First name is #{args.first_name}"
  puts "Last  name is #{args.last_name}"
end
```


#### Have Prerequisites

```ruby
task :name, [:first_name, :last_name] => [:pre_name]
```


#### Comments
```ruby
desc "Print 'Hello world!'"
task :hello do
	puts "Hello world!"
end
```


#### Namespaces
```ruby
namespace "main" do
  task :build do
    puts "main build"
  end
end

namespace "samples" do
  task :test => "main:build" do
    puts "samples test"
  end
end
```


### rbenv

- use different rubies in a manner that won't mess with your existing ruby install
- guarantee that your development environment matches production
- letting you run multiple different rubies in seperate terminals concurrently


#### How It Works

- Understanding PATH

- Understanding Shims

- Choosing the Ruby Version
  - `$ rbenv shell`
  - `$ rbenv local`
  - `$ rbenv global`


#### Installation

```bash
$ brew install rbenv ruby-build
```
Afterwards you'll still need to add eval "$(rbenv init -)" to your profile as stated in the caveats. You'll only ever have to do this once.


#### oh-my-zsh

```bash
$ subl ~/.zshrc
```

```
plugins=(git sublime rbenv)
```


#### rbenv command

- `$ rbenv install -l`
- `$ rbenv install 2.1.2`
- `$ rbenv local 2.1.2`
- `$ rbenv rehash`

  Run this command after you install a new version of Ruby, or install a gem that provides commands


#### rbenv which

- `$ rbenv versions`
- `$ rbenv which ruby`


### RVM

- Ruby Version Manager
- RVM is a command-line tool which allows you to easily install, manage, and work with multiple ruby environments from interpreters to sets of gems.


### chruby

Changes the current Ruby

```bash
$ brew install chruby
```


### Bundler

- manage your application's	 dependencies
- tracking and installing the exact gems and versions that are needed

```bash
$ gem install bundler

$ bundle init

$ bundle install
```


#### Gemfile

```ruby
source 'https://rubygems.org'

gem 'awesome_print'
gem 'rails', '3.0.0.beta3'
gem 'rack', '>=1.0'
gem 'thin', '~>1.1'
# gem 'thin', '>= 1.1', '< 1.2'

gem 'sinatra', :require => 'sinatra/base'
gem 'capybare-webkit', :require => false
```
```ruby
require 'awesome_print'

ap 'Hello World!'
```


#### Bundler.require

```ruby
require 'bundler/setup'
Bundler.require(:default)

ap "Hello World!"
```


#### Groups

```ruby
gem 'wirble', :group => :development
gem 'debugger', :group => [:development, :test]

group :test do
  gem 'rspec'
end
```

```bash
$ bundle install --without=development test
```


#### Gemfile.lock

```bash
$ git add Gemfile Gemfile.lock
```

ensures that other developers on your app, as well as your deployment environment, will all use the same third-party code that you are using now.


#### bundle exec

```bash
$ bundle exec rspec
```

>> rspec-core is not part of the bundle. Add it to Gemfile.


#### oh-my-zsh

```bash
$ subl ~/.zshrc
```

```
plugins=(git sublime rbenv bundler)
```


### RSpec

- RSpec is testing tool for the Ruby programming language
- Born under the banner of **Behaviour-Driven Development**
- it is designed to make **Test-Driven Development** a productive and enjoyable experience


features:

- a rich command line program (the rspec command)
- textual descriptions of examples and groups (rspec-core)
- flexible and customizable reporting
- extensible expectation language (rspec-expectations)
- built-in mocking/stubbing framework (rspec-mocks)


bowling_spec.rb

```ruby
require 'bowling'

Rspec.describe Bowling, "#score" do
  it "returns 0 for all gutter game" do
    bowling = Bowling.new
    20.times { bowling.hit(0) }
    expect(bowling.score).to be(0)
  end
end
```


bowling.rb
```ruby
class Bowling
  def hit(pins)
  end

  def score
    0
  end
end
```

```bash
$ rspec bowling_spec.rb
```



## Further Reading

- [Programming Ruby 1.9 & 2.0 (4th edition): The Pragmatic Programmers' Guide](http://pragprog.com/book/ruby4/programming-ruby-1-9-2-0)
- [Metaprogramming Ruby](http://book.douban.com/subject/4086938/)



## References

- http://rubylearning.com/satishtalim/ruby_open_classes.html
- https://en.wikipedia.org/wiki/Object-oriented_programming