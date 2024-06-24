# Concurrency and Parallelism
> This blog is a brief summary of the Chapter 10 Concurrency (and Parallelism) of "Rust for Rustaceans".

## Mental Model for Concurrency (High-level Design)
There are three most widely used mental model when writing a concurrency program: shared memory, worker pools, and actors.

### Shared Memory
Every threads read and write the same variables with the use of Mutex. Since they access the same variables, this model is called shared memory.

The variables can be states, counters, and other information that needs to shared among threads.

### Worker Pools
Worker pools are like thread pools. The spawn and termination of a worker is managed by the worker pools, so users only need to provide the code snippets that want to be ran and the data to the worker pools. Work stealing is an important implementation of worker pools.

Instead of a brand new mental model than shared memory, worker pools are based on shared memory but providing more convenience to the programmers. 

The best use cases for the worker pools are those jobs which apply the same functionalities to different segments of the same data. Increasing each element in an array by 1 is an example of such use cases - the array can be splitted into multiple segments and passed to different workers. It sounds like the pattern of Map-reduce, too.

### Actors
While worker pools model mainly does the same operations on the same data, though on different segements, each actor in the actors model **owns** the data that it operates on. The states exchange is done by sending messages among actors. Because the actor owns the data and other actors cannot access it directly, locks (or mutex) are needless in this model.