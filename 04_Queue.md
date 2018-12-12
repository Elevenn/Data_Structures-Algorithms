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
* 链式队列

* ```Objective-C
  @implementation XXNode

  - (instancetype)init {
      if (self = [super init]) {
          _value = nil;
          _next = nil;
      }

      return self;
  }

  - (instancetype)initWithValue:(id)value {
      if (self = [super init]) {
          _value = value;
          _next = nil;
      }

      return self;
  }

  @end
  ```
* 
* ```Objective-C
  @implementation XXStackQueue

  - (instancetype)init {
      if (self = [super init]) {
          _linkedList = [[XXLinkedList alloc] init];
      }

      return self;
  }

  - (BOOL)enqueue:(id)obj {
      [self.linkedList append:obj];

      return YES;
  }

  - (id)dequeue {
      [self.linkedList deleteHeadNode];

      return nil;
  }

  @end
  ```



