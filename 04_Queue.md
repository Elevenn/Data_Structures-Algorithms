* **队列**

  * **先进先出\(FIFO\) **是队列的特性
  * 队列分**顺序队列**和**链式队列** 它们有两个指针 **\*head \*tail**
  * 如果使用顺序队列 会涉及到**数据搬移**操作 这样出队的时间复杂度就会从原来的 **O\(1\) 变成 O\(n\)**
  * 链式队列不会涉及到数据搬移操作 所以 **enqueue\(\)** **dequeue\(\) **的时间复杂度都为 **O\(1\)**

* **循环队列**

  * 因为使用数组来实现队列会有数据搬移的操作, 这样入队操作性能就会受到影响, 所以就有了循环队列的产生
  * 循环队列长得像一个环 一个线性结构的数组首尾相连
  * 当队列有空间时才能存放数据 这样就能避免数据搬移操作 但是循环队列会浪费一个数组的存储空间

* **阻塞队列**

  * 阻塞队列在队列的基础上添加了阻塞操作, 简单的说就是在队列为空的时候 dequeue\(\) 时会被阻塞 因为此时还没有数据可取 直到队列中有了数据才能返回; 同理 队列已满时 enqueue\(\) 就会被阻塞 直到队列中有空闲位置后才能入队 然后再返回
  * 这种队列可以比方成时 「生产者 - 消费者模型」 为了有效的协调生产和消费的速度, 当「生产者」生产数据的速度过快, 「消费者」来不及消费时, 存储数据的队列很快就会满. 这个时候「生产者」就阻塞等待, 知道「消费者」消费了数据, 「生产者」才会被唤醒继续「生产」.
  * 其实 还可以基于阻塞队列, 协调「生产者」「消费者」的个数, 来提高数据的处理效率. 比如前面的例子, 配置多个「消费者」来对应一个「生产者」.

* **并发队列**

  * 并发队列是线程安全的队列

### Talk  is cheap, show you code

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
  //    结点
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
* ```Objective-C
  //    链表
  @implementation XXLinkedList

  - (instancetype)init {
      if (self = [super init]) {
          _head = nil;
          _tail = _head;
      }

      return self;
  }

  - (instancetype)initWithHead:(XXNode *)head {
      if (self = [super init]) {
          _head = head;
          _tail = _head;
      }

      return self;
  }

  - (void)append:(id)value {
      XXNode *tmp = [[XXNode alloc] initWithValue:value];

      if (self.head == nil) {
          self.head = tmp;
          self.tail = self.head;
      } else {
          tmp.next = self.tail.next;
          self.tail.next = tmp;
          self.tail = tmp;
      }
  }

  - (void)prepend:(id)value {
      XXNode *tmp = [[XXNode alloc] initWithValue:value];

      tmp.next = self.head;
      self.head = tmp;

      if (self.head.next == nil) {
          self.tail = self.head;
      }
  }

  - (void)deleteHeadNode {
      XXNode *tmp = self.head;
      self.head = self.head.next;
      tmp.next = nil;
  }

  @end
  ```

  ```Objective-C
  //    链式队列
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
* 循环队列
* ```Objective-C
  @implementation XXCircularQueue

  - (instancetype)initWithQueueSize:(NSUInteger)n {
      if (self = [super init]) {
          _n = n;
          _array = [NSMutableArray arrayWithCapacity:n];
          _head = 0;
          _tail = 0;

          for (int i = 0; i < n; ++ i) {
              [_array addObject:@(-1)];
          }
      }

      return self;
  }

  - (BOOL)enqueue:(id)obj {
      if ((self.tail + 1) % self.n == self.head) {
          NSLog(@"队已满");
          return NO;
      }

      self.array[self.tail] = obj;
      self.tail = (self.tail + 1) % self.n;

      return YES;
  }

  - (id)dequeue {
      if (self.tail == self.head) {
          NSLog(@"队已空");
          return nil;
      }

      id tmp = self.array[self.head];

      self.head = (self.head + 1) % self.n;

      return tmp;
  }

  @end
  ```



