- implementation:
	- ```python
	  class RingBuffer:
			def __init__(self, capacity):
				self.buffer = [None] * capacity
				self.capacity = capacity
				self.size = 0
				self.head = 0
				self.tail = 0
			
			def add(self, value):
				if self._is_full():
					return False
				self.buffer[self.tail] = value # self.tail contains next open value
				self.tail = (self.tail + 1) % self.capacity
				self.size += 1
				return True
				
			def pop(self):
				if self.size == 0:
					return False
				value = self.buffer[self.head] # self.head contains first thing to evict
				self.head = (self.head + 1) % self.capacity
				self.size -= 1
				return value
			
			def peek(self):
				if self.size == 0:
					return False
				return self.buffer[self.head]
			
			def _is_full(self):
				return self.size == self.capacity	
	  ```