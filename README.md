Ruby Styleguide
===============

Coding Style
------------

* Use `UTF-8` as the source file encoding.
* Use two **spaces** per indentation level. No hard tabs.
* Keep lines fewer than **80** characters.
* Never leave trailing whitespace
* Use spaces around operators, after commas, colons and semicolons, around `{` and before `}`.
  
    ````Ruby
    sum = 1 + 2
    a, b = 1, 2
    ````

    The only exception, regarding operators, is the exponent operator:
    
    ````Ruby
    # bad
    e = M * c ** 2
    
    # good
    e = M * c**2
    ````

* No spaces after `(`, `[` or before `]`, `)`.

    ````Ruby
    some(arg).other
    [1, 2, 3].length
    ````

* Indent `when` as deep as `case`.

    ````Ruby
    case when song.name == "Misty"
      puts "Not again!"
    when song.duration > 120
      puts "Too Long!"
    else
      song.play
    end
    
    kind = case year
           when 1850..1889 then "Blues"
           when 1890..1909 then "Ragtime"
           else "Jazz"
           end
    ````

Syntax
------

* Use `def` with parentheses when there are arguments. Omit the parentheses when the method doesn't accept any arguments.

    ````Ruby
    def some_method
      # body omitted
    end
    
    def some_method_with_arguments(arg1, arg2)
      # body omitted
    end
    ````
    
* Never use `for`, unless you know exactly why. Most of the time iterators
  should be used instead. `for` is implemented in terms of `each` (so
  you're adding a level of indirection), but with a twist - `for`
  doesn't introduce a new scope (unlike `each`) and variables defined
  in its block will be visible outside it.

    ```Ruby
    arr = [1, 2, 3]

    # bad
    for elem in arr do
      puts elem
    end

    # good
    arr.each { |elem| puts elem }
    ```

* Never use `then` for multi-line `if/unless`.

    ```Ruby
    # bad
    if some_condition then
      # body omitted
    end

    # good
    if some_condition
      # body omitted
    end
    ```

* Favor the ternary operator(`?:`) over `if/then/else/end` constructs if the expressions are trivial.

    ```Ruby
    # bad
    result = if some_condition then something else something_else end

    # good
    result = some_condition ? something : something_else
    ```

* Use one expression per branch in a ternary operator. This
  also means that ternary operators must not be nested. Prefer
  `if/else` constructs in these cases.

    ```Ruby
    # bad
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # good
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```

* Use `!` instead of `not`.

    ```Ruby
    # bad - braces are required because of op precedence
    x = (not something)

    # good
    x = !something
    ```

* The `and` and `or` keywords are banned. It's just not worth
  it. Always use `&&` and `||` instead.

    ```Ruby
    # bad
    # boolean expression
    if some_condition and some_other_condition
      do_something
    end

    # control flow
    document.saved? or document.save!

    # good
    # boolean expression
    if some_condition && some_other_condition
      do_something
    end

    # control flow
    document.saved? || document.save!
    ```

* Avoid multi-line `?:` (the ternary operator); use `if/unless` instead.

* Favor modifier `if/unless` usage when you have a single-line
  body. Another good alternative is the usage of control flow `&&/||`.

    ```Ruby
    # bad
    if some_condition
      do_something
    end

    # good
    do_something if some_condition

    # another good option
    some_condition && do_something
    ```

* Never use `unless` with `else`. Rewrite these with the positive case first.

    ```Ruby
    # bad
    unless success?
      puts 'failure'
    else
      puts 'success'
    end

    # good
    if success?
      puts 'success'
    else
      puts 'failure'
    end
    ```

* Don't use parentheses around the condition of an `if/unless/while`,
  unless the condition contains an assignment (see "Using the return
  value of `=`" below).

    ```Ruby
    # bad
    if (x > 10)
      # body omitted
    end

    # good
    if x > 10
      # body omitted
    end

    # ok
    if (x = self.next_value)
      # body omitted
    end
    ```

* Omit parentheses around parameters for methods that are part of an
  internal DSL (e.g. Rake, Rails, RSpec), methods that have
  "keyword" status in Ruby (e.g. `attr_reader`, `puts`) and attribute
  access methods. Use parentheses around the arguments of all other
  method invocations.

    ```Ruby
    class Person
      attr_reader :name, :age

      # omitted
    end

    temperance = Person.new('Temperance', 30)
    temperance.name

    puts temperance.age

    x = Math.sin(y)
    array.delete(e)

    bowling.score.should == 0
    ```

* Prefer `{...}` over `do...end` for single-line blocks.  Avoid using
  `{...}` for multi-line blocks (multiline chaining is always
  ugly). Always use `do...end` for "control flow" and "method
  definitions" (e.g. in Rakefiles and certain DSLs).  Avoid `do...end`
  when chaining.

    ```Ruby
    names = ['Bozhidar', 'Steve', 'Sarah']

    # bad
    names.each do |name|
      puts name
    end

    # good
    names.each { |name| puts name }

    # bad
    names.select do |name|
      name.start_with?('S')
    end.map { |name| name.upcase }

    # good
    names.select { |name| name.start_with?('S') }.map { |name| name.upcase }
    ```

    Some will argue that multiline chaining would look OK with the use of {...}, but they should
    ask themselves - is this code really readable and can the blocks' contents be extracted into
    nifty methods?


* Avoid `return` where not required for flow of control.

    ```Ruby
    # bad
    def some_method(some_arr)
      return some_arr.size
    end

    # good
    def some_method(some_arr)
      some_arr.size
    end
    ```
    
* Use spaces around the `=` operator when assigning default values to method parameters:

    ````Ruby
    # bad
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end
    
    # good
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end
    ````

* Avoid `self` where not required. (It is only required when calling a self write accessor.)

    ```Ruby
    # bad
    def ready?
      if self.last_reviewed_at > self.last_updated_at
        self.worker.update(self.content, self.options)
        self.status = :in_progress
      end
      self.status == :verified
    end

    # good
    def ready?
      if last_reviewed_at > last_updated_at
        worker.update(content, options)
        self.status = :in_progress
      end
      status == :verified
    end
    ```
 
 * As a corollary, avoid shadowing methods with local variables unless they are both equivalent.

    ```Ruby
    class Foo
      attr_accessor :options

      # ok
      def initialize(options)
        self.options = options
        # both options and self.options are equivalent here
      end

      # bad
      def do_something(options = {})
        unless options[:when] == :later
          output(self.options[:message])
        end
      end

      # good
      def do_something(params = {})
        unless params[:when] == :later
          output(options[:message])
        end
      end
    end
    ```

* Don't use the return value of `=` (an assignment) in conditional expressions.

    ```Ruby
    # bad (+ a warning)
    if (v = array.grep(/foo/))
      do_something(v)
      ...
    end

    # bad (+ a warning)
    if v = array.grep(/foo/)
      do_something(v)
      ...
    end

    # good
    v = array.grep(/foo/)
    if v
      do_something(v)
      ...
    end
    ```

* Use `||=` freely to initialize variables.

    ```Ruby
    # set name to Bozhidar, only if it's nil or false
    name ||= 'Bozhidar'
    ```

* Don't use `||=` to initialize boolean variables. (Consider what
would happen if the current value happened to be `false`.)

    ```Ruby
    # bad - would set enabled to true even if it was false
    enabled ||= true

    # good
    enabled = true if enabled.nil?
    ```

* Never put a space between a method name and the opening parenthesis.

    ```Ruby
    # bad
    f (3 + 2) + 1

    # good
    f(3 + 2) + 1
    ```

* If the first argument to a method begins with an open parenthesis,
  always use parentheses in the method invocation. For example, write
`f((3 + 2) + 1)`.


Naming
------

* Name identifiers in English.

* Use `snake_case` for symbols, methods and variables.

 * Use `CamelCase` for classes and modules.  (Keep acronyms like HTTP,
  RFC, XML uppercase.)

    ```Ruby
    # bad
    class Someclass
      ...
    end

    class Some_Class
      ...
    end

    class SomeXml
      ...
    end

    # good
    class SomeClass
      ...
    end

    class SomeXML
      ...
    end
    ```

* Use `SCREAMING_SNAKE_CASE` for other constants.

 * The names of predicate methods (methods that return a boolean value)
  should end in a question mark.
  (i.e. `Array#empty?`).

* The names of potentially *dangerous* methods (i.e. methods that
  modify `self` or the arguments, `exit!` (doesn't run the finalizers
  like `exit` does), etc.) should end with an exclamation mark if
  there exists a safe version of that *dangerous* method.



Classes
-------
* Avoid the usage of class (`@@`) variables due to their "nasty" behavior
in inheritance.

    ```Ruby
    class Parent
      @@class_var = 'parent'

      def self.print_class_var
        puts @@class_var
      end
    end

    class Child < Parent
      @@class_var = 'child'
    end

    Parent.print_class_var # => will print "child"
    ```

    As you can see all the classes in a class hierarchy actually share one
    class variable. Class instance variables should usually be preferred
    over class variables.


* Use `def self.method` to define singleton methods. This makes the code
  easier to refactor since the class name is not repeated.

    ```Ruby
    class TestClass
      # bad
      def TestClass.some_method
        # body omitted
      end

      # good
      def self.some_other_method
        # body omitted
      end

      # Also possible and convenient when you
      # have to define many singleton methods.
      class << self
        def first_method
          # body omitted
        end

        def second_method_etc
          # body omitted
        end
      end
    end
    ```
  
* Indent the `public`, `protected`, and `private` methods as much the
  method definitions they apply to. Leave one blank line above the
  visibility modifier
  and one blank line below in order to emphasize that it applies to all
  methods below it.

    ```Ruby
    class SomeClass
      def public_method
        # ...
      end

      private

      def private_method
        # ...
      end

      def another_private_method
        # ...
      end
    end
    ```
  
* Assign proper visibility levels to methods (`private`, `protected`)
in accordance with their intended usage. Don't go off leaving
everything `public` (which is the default).


Exception
---------
* Don't use exceptions for flow of control.

    ```Ruby
    # bad
    begin
      n / d
    rescue ZeroDivisionError
      puts 'Cannot divide by 0!'
    end

    # good
    if d.zero?
      puts 'Cannot divide by 0!'
    else
      n / d
    end
    ```

* Avoid rescuing the `Exception` class.  This will trap signals and calls to
  `exit`, requiring you to `kill -9` the process.

    ```Ruby
    # bad
    begin
      # calls to exit and kill signals will be caught (except kill -9)
      exit
    rescue Exception
      puts "you didn't really want to exit, right?"
      # exception handling
    end

    # good
    begin
      # a blind rescue rescues from StandardError, not Exception as many
      # programmers assume.
    rescue => e
      # exception handling
    end

    # also good
    begin
      # an exception occurs here

    rescue StandardError => e
      # exception handling
    end

    ```
    
  Collections
  -----------
  
* Prefer literal array and hash creation notation (unless you need to
pass parameters to their constructors, that is).

    ```Ruby
    # bad
    arr = Array.new
    hash = Hash.new

    # good
    arr = []
    hash = {}
    ```

* Prefer `%w` to the literal array syntax when you need an array of
words(non-empty strings without spaces and special characters in them).
Apply this rule only to arrays with two or more elements.

    ```Ruby
    # bad
    STATES = ['draft', 'open', 'closed']

    # good
    STATES = %w(draft open closed)
    ```

* Use `Set` instead of `Array` when dealing with unique elements. `Set`
  implements a collection of unordered values with no duplicates. This
  is a hybrid of `Array`'s intuitive inter-operation facilities and
  `Hash`'s fast lookup.

* Use symbols instead of strings as hash keys

    ````Ruby
    # bad
    hash = { "one" => 1, "two" => 2, "three" => 3 }
    
    # good
    hash = { one: 1, two: 2, three: 3 }

* Use the hash literal syntax when your hash keys are symbols.

    ```Ruby
    # bad
    hash = { :one => 1, :two => 2, :three => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```
    
Strings
-------

* Prefer string interpolation instead of string concatenation:

    ```Ruby
    # bad
    email_with_name = user.name + ' <' + user.email + '>'

    # good
    email_with_name = "#{user.name} <#{user.email}>"
    ```

* Consider padding string interpolation code with space. It more clearly sets the
  code apart from the string.

    ```Ruby
    "#{ user.last_name }, #{ user.first_name }"
    ```

* Avoid using `String#+` when you need to construct large data chunks.
  Instead, use `String#<<`. Concatenation mutates the string instance in-place
  and is always faster than `String#+`, which creates a bunch of new string objects.

    ```Ruby
    # good and also fast
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

     
