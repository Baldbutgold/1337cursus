 Rather    than    asynchronously
       catching  a  signal via a signal
       handler, it is possible to  syn‐
       chronously  accept  the  signal,
       that is, to block execution  un‐
       til  the signal is delivered, at
       which point the  kernel  returns
       information  about the signal to
       the caller.  There are two  gen‐
       eral ways to do this:

## Pending Signals

There is a brief period of time between the time a signal is generated and the time a signal is delivered (i.e. the action for the signal is taken). If another signal in generated during this time problems can arise. For instance you might wish to reset the signal handler but before the signal handler is reset another signal could arrive.

# bit wise operators

# concepts
- **signal:** handles the signal 
- **Kill**: send a signal to a process
- **getpid**: get process id, and returns a pidid_t
- **pause**: wait for a signal
- **exit**: cause the shell to exit