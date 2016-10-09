# psscalejavaappconcor
###3
ExecutorService
```
ExecutorService es = Executors.newCachedThreadPool();
Executors.newFixedThreadPool(10)
Executors.newSingleThreadExecutor();
```
maintains a pool for rapid reuse.  

###11 the scatter-gather pattern
Identity pipeline: parallel workers (reads in a stream of identoty-related data)
- normalizes and verifies the contents of each identity
- merges and persists the identities in an in memory cache
- records aggregate identity statistics
- notifies an error queue if any of the above fails



###12 Scatter gather Futures
```
Future<Identity> identityHolder=pool.submit(()->reader.read(source));
Identity identity = identityHolder.get();
List<Future<Identity>> identitiesHolder = pool.invokeAll(listOfCallables);
```

###31 Throttling
```
ThreadPoolExecutor pool = new ThreadPoolExecutor(5,5,-1,TimeUnit.MILLISECONDS,new ArrayBlockingQueue<>(capacity),new RejectedExecutionHandler(){
  public void rejectedExecution(){
  }
});
```

###33 Throttling Using Semaphore
summary
- Semaphore has a given number of permits that can be issued on its lock
- Each client blocks at a 'ticket booth', waiting for a permit
- Only a given number of callers can invoke the method at a time


###34 Implementing Throttling
```
private Semaphore limiter;
public void verify(List<Address> address){
  try{
    limiter.acquire();
    try{
      delegate.verity(address);
    }finally{
      limiter.release();
    }
  }catch(InterruptedException e){
    Thread.currentThread.interrupt();
  }
}
```
