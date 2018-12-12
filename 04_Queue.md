* 顺序队列
* ```Objective-C
  @implementation XXArrayQueue

  - (instancetype)initWithArrayQueueCapacity:(NSUInteger)capacity {
      if (self = [super init]) {
          _array = [NSMutableArray arrayWithCapacity:capacity];
          _size = capacity;
          _head = 0;
          _tail = 0;
      }
    
      return self;
  }

  //  入队
  - (BOOL)enqueue:(id)obj {
      if (self.size == self.tail) {
          if (self.head == 0) {
              NSLog(@"队已满");
              return NO;
          }
        
          for (NSUInteger i = self.head; i < self.tail; ++i) {
              self.array[i - self.head] = self.array[i];
          }
        
          self.tail -= self.head;
          self.head = 0;
      }
    
      [self.array addObject:obj];
  //    self.array[self.tail] = obj;
      ++self.tail;
    
      return YES;
  }

  //  出队
  - (id)dequeue {
      if (self.tail == self.head) {
          NSLog(@"队已空");
          return NULL;
      }
    
      id tmp = self.array[self.head];
    
      ++self.head;
    
      return tmp;
  }

  @end
  ```



