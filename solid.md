# SOLID Principles

## Liskov Substitution Principle

Subtype objects can replace parent types without breaking anything

They should not throw exceptions if they’re not true, e.g.

### Bird Example

Penguin inherits from bird which has a fly method and throws an exception for it because penguins can’t fly
Penguin shouldn’t inherit any methods which it can’t implement, it could be broken up into a separate SwimmingBird class which it inherits from

This can cause problems with birds like ducks which can fly and swim, but can’t inherit from both - an example of why composition can be better than inheritance



### Subtypes should not have stronger constraints on their methods that they override

A method in a subclass which takes an int argument shouldn’t throw an error when the number is negative, if it doesn’t in the superclass. 
If it does, you have a case where the program will always run when the parent is called but not always if substituted with the subclass.