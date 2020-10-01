# Domain Driven Design

### Create several models


First of all, create different models for the domain/problem you are trying to solve. Don’t just think of one and present it to the client because they will probably just say yes.
Think of it like buying a car, you might find a car which has all your requirements but you won’t buy it straight away, you’ll test drive it, have a look at others and decide which is the best fit for you.

It doesn’t have to be a UML model, just a representation of the domain. 

### Select the model which fits best

With several models to look at, you and the client will have to think more about the tradeoffs of each one, enabling you to choose the one which is most appropriate for the problem faced. You can come up with different models over several short sessions and then choose one in a separate one to give time to digest.
 
### Remember the model is to solve a specific problem

You may then have to solve a different problem in the same domain. It may seem easiest to tack the features onto your existing classes/model, but sometimes it’s best to have different models for different problems
Example - shipping
Changes to the final destination when itinerary is underway 
Having the model set up as legs (from one port to another) makes it easier to keep the destination of the first leg the same but then replace the remaining legs of the journey to get to the new destination
If you used stops instead of legs, it would be harder to change the itinerary, because you’d have to change the change the incoming and outgoing fields of every stop instead of replacing legs.

In another case, there might be a lot of information about what needs to be done at the port - in this case stops would be more useful rather than lumping the information into a leg class.


### Use the domain language

It’s important to try to use the domain specific language as much as possible. It means that the client who doesn’t know about software can always understand what’s going on, e.g. don’t go into the nitty gritty of databases and translate it into what it’s doing for the client in language they understand.
