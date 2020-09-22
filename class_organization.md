## Class Organization

In Ruby, Classes are processed from top to bottom. Therefore, order matters (most of the time). For example, sometimes included Modules will need to refer to constants in the including Class. So constants should be defined before Module inclusions are specified. Otherwise we’re free to organize our Classes how we want but thoughtful organization makes the story a Class tells easier to understand.

In general, let the following set of rules be your guide for class organization in Ruby Classes:

1. Put more general / more important things at the top of a Class.
2. Put more specific / less important things next.
3. Group collaborating methods in close proximity using a top-down flow.
   ```ruby
   def my_composed_method
     a
     b
   end

   def a
     # ...
   end

   def b
     # ...
   end
   ```
    1. **Note**: If it is proving difficult to group collaborating methods  in close proximity then that may be an indicator that the Class is doing too much. Look for secondary responsibilities that can be broken out into a new Object.


```ruby
require "my_class"           # Require any functionality needed for the whole Class

# MyNamespace is a context where multiple classes either interact with each other or are used in the same domain context.
# MyClass is a ...                 # Document the purpose of this Class
class MyNamespace::MyClass < MyNamespace::MyBaseClass  # Class definition with fully qualified Superclass (when applicable)
  MY_CONSTANT = "MyConstantValue".freeze  # Constants; May apply to included modules as well so must come first

  Error = Class.new(StandardError)  # Other types of Constants like StandardError derivatives; May also be used by included modules

  extend MyExtension                  # Extensions

  include MyMixin                 # Mixins (Less specific than Extensions)

  as_abstract_class                   # Important macros

  before_validation                 # Callbacks — in callback order
  before_save
  after_create

  attr_accessor                       # Virtual Attributes
  attr_writer
  attr_reader

  belongs_to :my_associated_object  # Associations

  has_one :my_other_related_object  # Less "important" Object associations

  validates :my_attribute,
            presence: true          # Validations

  delegate :some_method,
           to: :my_associated_obj # Delegations

  scope :my_scope, → { }          # Scopes

  def initialize...                 # Initialize Method

  def self.my_class_method...         # Class Methods

  concerning :<SomeConcern>s do
    # ...
  end

  def display_name...                 # Display Methods

  def my_important_method...          # Important Instance Methods → Less Important Instance Methods

  def my_risky_method
    # Code that may raise a StandardError
  rescue => ex                  # De-dented to method level — doesn't require begin/end; Also helps encourage Single Responsibility Principle
    # Log exception, etc.
  end

  private

  def inspect...                  # Inspect Methods

  def my_private_method...          # Private Methods

  class MySubClass          # Classes only intended for use by the current Class
  end
end
```

## Coding Style

This is our team style guide. It is impartial and generally accepted by the Ruby community. So be it for us.
- Note: there will be rare exceptions on some of the guidelines. That is OK.

Also, consider the [Rails Style Guide](https://github.com/bbatsov/rails-style-guide).
We don't necessarily follow these 100%. But they're useful guides when in doubt.
