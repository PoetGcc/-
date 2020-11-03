## ArrayBlockingQueue

```java
public class ArrayBlockingQueueTest {
    public static void main(String[] args) {
        test();
    }

    private static void test() {
        // 创建一个长度为 5 的阻塞队列
        ArrayBlockingQueue<String> queue = new ArrayBlockingQueue<>(5);

        // 新创建一个线程执行入列
        new Thread(() -> {
            System.out.println(new Date() + " | ========> For start.");

            for (int i = 0; i < 10; i++) {
                try {
                    queue.put(String.valueOf(i));
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

                System.out.println(new Date() + " | ArrayBlockingQueue Size:" + queue.size());
            }

            System.out.println(new Date() + " | ========> For End.");
        }).start();

        // 一个线程执行出列
        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    TimeUnit.SECONDS.sleep(1);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

                if (queue.isEmpty()) {
                    return;
                }

                try {
                    String v = queue.take();
                    System.out.println(new Date() + " | 出列一个 " + v);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }
}

```

***

## LinkedBlockingQueue

```java
public class LinkedBlockingQueueTest {
    public static void main(String[] args) {
        test();
    }

    private static void test() {
        LinkedBlockingQueue<String> queue = new LinkedBlockingQueue<>();
        for (int i = 0; i < 5; i++) {
            queue.offer("This is " + i);
        }

        while (!queue.isEmpty()) {
            System.out.println(queue.poll());
        }
    }
}
```

***

## LinkedBlockingDeque

```java
public class LinkedBlockingDequeTest {
    public static void main(String[] args) {
        test();
    }

    private static void test() {
        LinkedBlockingDeque<String> deque = new LinkedBlockingDeque<>();
        deque.offer("offer");
        deque.offerFirst("offerFirst");
        deque.offerLast("offerLast");
        while (!deque.isEmpty()) {
            // 从头开始遍历
            System.out.println(deque.poll());
        }
    }
}
```

***

## PriorityQueue

```java
public class PriorityQueueTest {

    public static void main(String[] args) {
        test();
    }

    private static void test() {
        // 优先队列的出队是不考虑入队顺序的，它始终遵循的是优先级高的元素先出队
        PriorityQueue<Viper> queue = new PriorityQueue<>(10, new Comparator<Viper>() {
            @Override
            public int compare(Viper v1, Viper v2) {
                // 设置优先级规则（倒序，等级越高权限越大）
                return v2.getLevel() - v1.getLevel();
            }
        });

        for (int i = 0; i < 5; i++) {
            Viper v = new Viper(i, "This is " + i, i);
            queue.offer(v);
        }

        while (!queue.isEmpty()) {
            Viper vip = queue.poll();
            System.out.println(vip.toString());
        }
    }

    private static class Viper {
        private int id;
        private String name;
        private int level;

        public Viper(int id, String name, int level) {
            this.id = id;
            this.name = name;
            this.level = level;
        }

        public int getId() {
            return id;
        }

        public void setId(int id) {
            this.id = id;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public int getLevel() {
            return level;
        }

        public void setLevel(int level) {
            this.level = level;
        }

        @Override
        public String toString() {
            return "Viper{" +
                    "id=" + id +
                    ", name='" + name + '\'' +
                    ", level=" + level +
                    '}';
        }
    }
}
```

***

## DelayQueue

```java
public class DelayQueueTest {

    public static void main(String[] args) {
        test();
    }

    private static void test() {
        DelayQueue<MyDelay> queue = new DelayQueue<>();
        // 生产者: 入队
        producer(queue);
        // 消费者：出列
        consumer(queue);
    }

    private static void producer(DelayQueue<MyDelay> queue) {
        queue.put(new MyDelay(1000, "消息1==>1000"));
        queue.put(new MyDelay(3000, "消息2==>3000"));
    }

    private static void consumer(DelayQueue<MyDelay> queue) {
        System.out.println("开始执行时间：" + DateFormat.getDateTimeInstance().format(new Date()));

        while (!queue.isEmpty()) {
            try {
                MyDelay delay = queue.take();
                System.out.println(delay);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        System.out.println("结束执行时间：" + DateFormat.getDateTimeInstance().format(new Date()));
    }

    private static class MyDelay implements Delayed {

        private long delayTime;
        private String msg;

        public MyDelay(long delayTime, String msg) {
            this.delayTime = System.currentTimeMillis() + delayTime;
            this.msg = msg;
        }

        @Override
        public long getDelay(TimeUnit unit) {
            // 获取剩余时间
            return unit.convert(delayTime - System.currentTimeMillis(), TimeUnit.MILLISECONDS);
        }

        @Override
        public int compareTo(Delayed o) {
            return Long.compare(getDelay(TimeUnit.MILLISECONDS), o.getDelay(TimeUnit.MILLISECONDS));
        }

        public long getDelayTime() {
            return delayTime;
        }

        public void setDelayTime(long delayTime) {
            this.delayTime = delayTime;
        }

        public String getMsg() {
            return msg;
        }

        public void setMsg(String msg) {
            this.msg = msg;
        }

        @Override
        public String toString() {
            return "MyDelay{" +
                    "msg='" + msg + '\'' +
                    '}';
        }
    }
}
```

## SynchronousQueue

```java
public class SynchronousQueueTest {

    public static void main(String[] args) {
        test();
    }

    private static void test() {
        SynchronousQueue<String> queue = new SynchronousQueue<>();
        // 数量
        AtomicInteger count = new AtomicInteger(5);
        int v = count.get();
        // 入队
        new Thread(() -> {
            for (int i = 0; i < v; i++) {
                String time = DateFormat.getDateTimeInstance().format(new Date());
                System.out.println(time + " | " + i + " 入队");
                try {
                    queue.put("Data ==> " + i);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();

        // 出列
        new Thread(() -> {
            while (count.get() > 0) {
                try {
                    TimeUnit.SECONDS.sleep(1);
                    String time = DateFormat.getDateTimeInstance().format(new Date());
                    System.out.println(time + " | " + queue.take() + " 出队 " + queue.isEmpty());
                    count.getAndDecrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }
}
```

