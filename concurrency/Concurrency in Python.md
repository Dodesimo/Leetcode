- threads
	- ```python
	  import threading
	  def worker(name):
		  print(f"{name} started")
		  print(f"{name} finished")
	  threads = []
	  for i in range(4):
		  thread = threading.Thread(target = worker, args = (i,))
		  thread.start()
		  threads.append(thread)
	  for thread in threads:
		  thread.join()
	  
	  thread.start() #begins execution
	  thread.join()  #waits for completion
	  thread.is_alive() #check for if the thread is running
	  ```
* locks:
	* serialize access to a critical section
	* ```python
	  import threading
	  counter = 0 
	  lock = threading.Lock()
	  def increment():
		  global counter
		  for i in range(1000):
			  with lock:
				  counter += 1
	  ```
* reentrant lock:
	* can be acquired multiple times by the same thread
	* allows for a synchronized method to call another synchronized method
		* ```python
		  class Account:
			  def __init__(self):
				  self.lock = threading.RLock()
				  self.balance = 0
			  def deposit(self, amount):
				  with self.lock:
					  self._update_balance(amount)
			  def _update_balance(self, amount):
				  with self.lock:
					  self.balance += amount
					
		  ```
* deadlocks: 
	* two threads acquire locks in different orders
	* avoid by acquiring locks in the same order, minimizing nested locks, using lock timeouts
* semaphore: gives up to N threads in a section simultaneously
	* ```python
	  import threading
	  import time
	  
	  semaphore = threading.Semaphore(3)
	  
	  def access_service(worker_id):
		  with semaphore:
			  print(f"{worker_id} entered")
			  time.sleep(1)
	  ```
* condition variable:
	* threads wait until a shared-state condition becomes true
	* ```python
	  import threading
	  condition = threading.Condition()
	  items = []
	  def consumer():
		  with condition:
			  while not items:
				  condition.wait()
		  item = items.pop()
		  print(item)
	  
	  def producer():
		  with condition:
			  items.append("task")
			  condition.notify()
	  ```
  * queue:
	  * thread safe multi producer multi consumer queue
	  * ```python
	    from queue import Queue
	    q = Queue(maxsize = 5)
	    q.put("Task 1") # add items to the queue (if at capacity, if blocking, then go to sleep else error)
	    q.get() # remove from the queue (if empty, if blocking, then go to sleep else error)
	    ```