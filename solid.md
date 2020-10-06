# SOLID Principles

## Liskov Substitution Principle

Subtype objects can replace parent types without breaking anything

They should not throw exceptions if they’re not true, e.g.

### Bird Example

Penguin inherits from bird which has a fly method and throws an exception for it because penguins can’t fly
Penguin shouldn’t inherit any methods which it can’t implement, it could be broken up into a separate SwimmingBird class which it inherits from

This can cause problems with birds like ducks which can fly and swim, but can’t inherit from both - an example of why composition can be better than inheritance



### Subtypes should not have stronger or weaker constraints on their methods that they override

You can't make big changes to how the base class works - causes unexpected behaviour. 

You can't return new exceptions, you can't change the return type of a method.

A method in a subclass which takes an int argument shouldn’t throw an error when the number is negative, if it doesn’t in the superclass. 
If it does, you have a case where the program will always run when the parent is called but not always if substituted with the subclass. If you're not handling that error when you call the method based on the parent class, it can cause unhandled errors in your program. 

## Interface Segregation Principle

A client should not be forced to depend on interfaces that they don't use.
A class should not implement an interface if it doesn't use all of the variables/methods of the interface.
E.g. in a game there is an entity interface which all objects and characters inherit from. Turret and wall implement this interface, but wall shouldn't implement the method move or attack and turret shouldn't use move. This means they are not suitable for implementing this interface, the interface needs to be changed, as not all entities have those variables/methods. 

Break down complex interfaces into smaller ones where all classes which inherit from them have to implement all of the variables and functions.

An alternative solution is composition - where the correct methods and variables get added to a class, e.g. turret is an attacker so assign attack.

### Library example

You can't have everything in a LibraryItem interface because different items have different fields. Here is an example of how to separate into interfaces:

public interface ILibraryItem

public interface IBook : ILibraryItem

public interface IBorrowable

public interface IBorrowableBook : IBorrowable, IBook

public interface IAudioBook : ILibraryItem

public interface IBorrowableAudioBook : IAudioBook, IBorrowable


public class Book : IBorrowableBook (implements everything you have in IBorrowable, IBook and ILibraryItem)

public class ReferenceBook : IBook (can't borrow reference books so only implements the IBook interface)


Be careful not to break things up too much - e.g. Audiobook and DVD both have a runtime, so you may be tempted to break that out into an interface they can inherit from, but they're not really related in any way despite both having a runtime.

You want small interfaces which can be built up to make larger interfaces, but at the core they are still modular so you can be more flexible, not violate the Open Closed principle and implement DRY(Don't Repeat Yourself).
