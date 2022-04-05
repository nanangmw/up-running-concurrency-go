# Up and Running with Concurrency in Go (Golang)

# Why learn concurrency in go?
- Improved performance (speed and scalability)
- Trends of increased latency and multicore CPUs create both a NEED FOR and OPPORTUNITY FOR concurrency
- Its what go was designed for.

# Understanding concurrency - trends, benefits and challenges
The potential for making progress on more than one task simultaneously or composition of independently executing processes is known as concurrency. Go has rich support for concurrency using goroutines and channels.

![concurrency 1](asset/img-1.png)
![concurrency 2](asset/img-2.png)

## Latency
![latency 1](asset/img-3.png)
![latency 2](asset/img-4.png)
![latency 3](asset/img-5.png)

## Multiple CPU cores
![cpu cores 1](asset/img-6.png)
![cpu cores 2](asset/img-7.png)

## Benefits and challenges
Benefits :
- Take advantages of latency in I/O-bound processes to improve performance on even a single processor
- Take advantages of multiple cores with CPU-bound processes for the fastest possible performance of your code on any hardware

Challenges :
- Not all code can be run concurrency
- Some additional complicatiob to write concurrency code
- Beware of intermittent bugs due to race conditions and other unexpected behavior

# Goroutine
A goroutine is a lightweight execution thread in the Go programming language and a function that executes concurrently with the rest of the program. Goroutines are incredibly cheap when compared to traditional threads as the overhead of creating a goroutine is very low.

Just add `go` in front of any functions and you are set!
> go doStuff()

# Understanding blocking code
- Blocking code does not allow its process to continue until blocking call finishes execution
- Blocking code does not allow its process to continue until blocking call finishes execution
- Blocking code allows execution to continue during waiting periods
- "blocking" code is like "normal" code in that it must complete before the process can continue
- Some goroutines can continue while others are blocked

Blocking code is generally used to :
- Force `func main()` to wait for other goroutines to complete (waitgroups)
- Synchronize goroutines at very specific points (channels)
- Prevent two goroutines from accessing a shared variable at the same time (mutexes, condition variables, atomic variables)

# Using waitgroups to track goroutines status
![wait groups 1](asset/img-8.png)

# Using channels to communicate with goroutines
Channels allow complex communication and synchronization between goroutines, inckuding `func main()`

![channels 1](asset/img-9.png)

# Understanding IO-bounnd vs CPU-bound processes
- I/O bound means the program is limited by I/O, or input/output, such as reading or writing to disk, network, etc
- CPU bound means the program is limited by the CPU, or central processing unit

Observations :
- Multiple cores dont help with sequential tasks
- Just one coree can see signficant speed improvements with concurrency in IO-bound code due latency
- CPU-bound code sees significant improvements up to the number of CPU cores. concurrency is no help with one core

|  | Sequential | IO-bound | CPU-bound |
| ------------- | ------------- | ------------- | ------------- |
| Single core | N/A | ✓ | N/A |
| Multi core | NO HELP | ✓ | ✓ |

# Race conditions
![race conditions 1](asset/img-10.png)
![race conditions 2](asset/img-11.png)
![race conditions 3](asset/img-12.png)

## Mutex syntax overview
```
mutex = sync.Mutex{}
mutex.Lock()
mutex.Unlock()
```

## Conditions variables syntax overview
```
var condition = sync.NewCond(&mutex)
condition.Signal()
condition.Broadcast()
condition.Wait() <-- blocking
```

## Atomic variables syntax overview
ex: 
> atomic.AddInt32(&inventory, 100)

same as :
```
mutex.Lock()
inventory += 100
mutex.Unlock()
```
