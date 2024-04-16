
# Things to Remember: 

Sender, Event, Delegate, Event Handler, Multicast delegate, Single cast delegate, Generic Events, Generic Delegates, `EventArgs`.

# Lambda expressions: 

you can access variable for the outer scope except ref and out variables, they are not reusable, delegates are a simplification/of anonymous methods, anonymous methods are really a delegate.

# Event and Delegate:
Delegate is a specialized class that points to one or multiple functions, when invoke the functions are invoked according to their order in the invocation list. For that reason an out variable of a delegate and a return value of a delegate are decided by the last function in the invocation list.
Delegates can be passed as parameters. Callback functions (functions that can be passed as parameters and called from within  another function) can only be achieved through delegates in c#.
Pertaining to events, delegates are the ones that allow the communication between the Sender(object that raises the event) and the method Handler (function that deals with the event)
Also pertaining to events, delegates are the ones that transfer the data from the event to the event handler in the form of a built-in `EventArgs` instance or a user-defined `EventArgs` instance.
Events always point to an underlying delegate that is invoked when the event is invoked.
Generic events are built in event's in .NET. They are generic (obviously kkkkk) and very commonly used. For example the built-in `EventHandler<T>`(`object` Sender, `EventArgs` e) event
Generic delegates are `Action`, `Func`, `Predicate`. `Action `and `Func` take up to 16 parameters, Action returns a user defined return type, `Func` returns void. Predicate returns a bool value. 
`Func` generic delegates are supposed to point to traditional functions that do somethin and return something.
`Action `generic delegates are supposed to point to specific functions, functions that work like actions: just do something and don't return anything.
`Predicate` delegates are supposed to point to functions that are checking to see if a condition is true or false and return the results.

# Other Very Relevant Information of The Topic

Sender is the object that raises the event. 