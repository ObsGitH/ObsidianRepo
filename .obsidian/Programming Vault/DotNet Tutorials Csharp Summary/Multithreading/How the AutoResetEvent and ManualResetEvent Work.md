
The object of these classes can have a signaled(true) or unsignaled(false) initial state passed as a parameter to their respective constructors.

The Manual/AutoResetEvent.WaitOne() makes the current thread wait for the state of the Manual/AutoResetEvent instance to become signaled, stopping the executions of those threads while the ResetEvent object is in unsignaled state.

The Manual/AutoResetEvent.Set() changes the state of the Manual/AutoResetEvent instance to become signaled allowing the executions of those threads. 

The Manual/AutoResetEvent.Reset() will aways  set the state of the Manual/AutoResetEvent object to unsignaled.


The difference between the Auto and Manul is that:  with the autoResetEvent instance only allow one waiting thread to resume execution, that is because right after a thread resumes his execution the autoResetEvent object's state becomes unsignaled again, if you want another waiting thread to resume execution .Set() needs to be called again from the autoResetEvent object.

In the ManualResetEvent once Set() is called and the state of it's instance becomes signaled, it will only go back to unsignaled once you call Reset(). Consequentially, all threads that are waiting for a change in the state of a ManualResetEvent instance will resume execution simultaneously when Set() is called .

