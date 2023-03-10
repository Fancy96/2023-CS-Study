# Stack / Queue

## π Stack

<img src="img/stack_and_queue_01.png" alt="stack">

> νλ§xμ€λ₯Ό μμλ‘ λ€μ΄λ³΄μ.  
> ν΅ μμ κ°μμΉ©μ΄ μμλλ‘ μ°¨κ³‘μ°¨κ³‘ μμ¬μλ€. νλ§xμ€λ₯Ό λ§λ€λλ ν΅ κ°μ₯ μλμλΆν° κ°μμΉ©μ νλμ© μ°¨κ³‘μ°¨κ³‘ λ΄λλ€.
> νλ§xμ€λ₯Ό μ° μ¬λμ λκ»μ μ΄κ³  μμμλΆν° νλμ©(λλ μ¬λ¬κ°) κΊΌλ΄λ¨Ήλλ€.

## νΉμ§

### νμμ μΆ(LIFO Last In First Out) κ΅¬μ‘°

`Stack`(μ΄ν 'μ€ν')μ΄λ λ°μ΄ν°λ₯Ό μμμ¬λ¦° ννμ μλ£κ΅¬μ‘°λ₯Ό λ»νλ€.  
λ°μ΄ν°λ₯Ό ν λ°©ν₯μΌλ‘λ§ μ μ₯ ν  μ μκ³  `μ΅μμΈ΅(Top)`μΌλ‘ μ ν κ³³μ μμΉν λ°μ΄ν°λ§ `μ½μ/μ‘°ν/μ­μ ` ν  μ μλ€.

## μ£Όμ λ©μλ

1. `isEmpty()`, `isFull()`
   - κ°κ° μ€νμ΄ λΉμ΄μλμ§, κ°λμ°Όλμ§λ₯Ό boolean ννλ‘ λ¦¬ν΄νλ λ©μλ.
2. `push()`
   - μ€νμ μλ‘μ΄ μμλ₯Ό μ½μνλ€. 
   - κ°λ μ°¨ μλ€λ©΄ μμΈλ₯Ό λμ§λ€.
3. `peek()`
   - μ΅μμΈ΅μ μμΉν λ°μ΄ν°λ₯Ό μ½μ΄μ¨λ€.
4. `pop()`
   - μ΅μμΈ΅μ μμΉν λ°μ΄ν°λ₯Ό μ½μ΄μ€κ³  ν΄λΉ λ°μ΄ν°λ₯Ό μ€νμμ μ κ±°νλ€.

## Java μ Stack κ΅¬ν

### μΈν°νμ΄μ€ μ μ

```
public interface MyStack<T> {
    boolean isEmpty();
    boolean isFull();
    void push(T element);
    T peek();
    T pop();
    void clear();
}
```

### ν΄λμ€ κ΅¬ν

```
public class MyStackImpl<T> implements MyStack<T> {

    private List<Optional<T>> myStack;
    private int limit;

    public MyStackImpl(int size) {
        this.myStack = new LinkedList<>();
        this.limit = size;
    }

    @Override
    public boolean isEmpty() {
        return this.myStack.isEmpty();
    }

    @Override
    public boolean isFull() {
        return this.myStack.size() == limit;
    }

    @Override
    public void push(T element) throws FullException {
        if (this.myStack.size() == limit) {
            throw new FullException();
        }
        this.myStack.add(Optional.ofNullable(element));
    }

    @Override
    public T peek() throws EmptyException {
        try {
            return this.myStack.get(myStack.size() - 1).orElseThrow(EmptyException::new);
        } catch (IndexOutOfBoundsException e) {
            throw new EmptyException();
        }
    }

    @Override
    public T pop() throws EmptyException {
        try {
            return myStack.remove(myStack.size() - 1).orElseThrow(EmptyException::new);
        } catch (IndexOutOfBoundsException e) {
            throw new EmptyException();
        }
    }

    @Override
    public void clear() {
        myStack.clear();
    }
}
```

### μμΈ μ²λ¦¬

```
public class FullException extends RuntimeException {

    public FullException() {}

    public FullException(String message) {
        super(message);
    }
}
```

```
public class EmptyException extends RuntimeException {

    public EmptyException() {}

    public EmptyException(String message) {
        super(message);
    }
}
```

## π‘ Queue

<img src="img/stack_and_queue_02.png" alt="queue">

> λμ΄κ³΅μμ΄λ λ§€νμ λ± μ€μ μμ μ°¨λ‘λ‘ μλ¬΄λ₯Ό μ²λ¦¬νκ±°λ μΉ λΈλΌμ°μ μ λ€λ‘κ°κΈ° κΈ°λ₯μ μνν  λ λ±

## νΉμ§

### μ μμ μΆ(FIFO First In First Out) κ΅¬μ‘°

`Queue`(μ΄ν 'ν')λ λ°μ΄ν°λ₯Ό μμλλ‘ μ€μ μΈμ΄ ννμ μλ£κ΅¬μ‘°λ₯Ό λ»νλ€.  
`νλ‘ νΈ(Front)`λ‘ μ ν κ³³μμλ `μ‘°ν/μ­μ ` μ°μ°μ΄ μΌμ΄λκ³ , `λ¦¬μ΄(Rear)`λ‘ μ ν κ³³μμ `μ½μ` μ°μ° λ°μ.

## μ£Όμ λ©μλ

1. `isEmpty()`, `isFull()`
   - μμ μ€λͺν μ€νκ³Ό λμΌ, κ°κ° νκ° λΉμλμ§, κ°λ μ°Όλμ§λ₯Ό boolean ννλ‘ λ°ν.
2. `enqueue()`
   - νμ μλ‘μ΄ μμλ₯Ό μ½μνλ€.
   - κ°λ μ°¨ μλ€λ©΄ μμΈλ₯Ό λμ§λ€.
3. `peek()`
   - μ΅νμΈ΅μ μμΉν λ°μ΄ν°λ₯Ό μ½μ΄μ¨λ€.
4. `dequeue()`
   - μ΅νμΈ΅μ μμΉν λ°μ΄ν°λ₯Ό μ½μ΄λ‘κ³  ν΄λΉ λ°μ΄ν°λ₯Ό νμμ μ κ±°νλ€.

## Java μ Queue κ΅¬ν

### μΈν°νμ΄μ€ μ μ

```
public interface MyQueue<T> {
    boolean isEmpty();
    boolean isFull();
    void enqueue(T element);
    T peek();
    T dequeue();
    void clear();
}
```

### ν΄λμ€ κ΅¬ν

```
public class MyQueueImpl<T> implements MyQueue<T> {

    private List<Optional<T>> myQueue;
    private int limit;

    public MyQueueImpl(int size) {
        this.myQueue = new LinkedList<>();
        this.limit = size;
    }

    @Override
    public boolean isEmpty() {
        return this.myQueue.isEmpty();
    }

    @Override
    public boolean isFull() {
        return this.myQueue.size() == limit;
    }

    @Override
    public void enqueue(T element) throws FullException {
        if (isFull()) {
            throw new FullException();
        }
        myQueue.add(Optional.ofNullable(element));
    }

    @Override
    public T peek() throws EmptyException {
        try {
            return this.myQueue.get(0).orElseThrow(EmptyException::new);
        } catch (IndexOutOfBoundsException e) {
            throw new EmptyException();
        }
    }

    @Override
    public T dequeue() throws EmptyException {
        try {
            return this.myQueue.remove(0).orElseThrow(EmptyException::new);
        } catch (IndexOutOfBoundsException e) {
            throw new EmptyException();
        }
    }

    @Override
    public void clear() {
        this.myQueue.clear();
    }
}
```

### μμΈ μ²λ¦¬

```
public class FullException extends RuntimeException {

    public FullException() {}

    public FullException(String message) {
        super(message);
    }
}
```

```
public class EmptyException extends RuntimeException {

    public EmptyException() {}

    public EmptyException(String message) {
        super(message);
    }
}
```

# μμ μ§λ¬Έ

### Q1. μ€νμΌλ‘ νλ₯Ό κ΅¬ν ν  μ μλκ°? κ·Έ λ°λλ?

> μ€ν λκ°λ₯Ό μ¬μ© ν΄ κ΅¬ν κ°λ₯ν©λλ€.

### Q2. μ€νκ³Ό νμ νΉμ±μ λͺ¨λ μ¬μ©ν΄μΌνλ€λ©΄?

> `λ°ν¬(Deque)`μλ£κ΅¬μ‘°λ₯Ό μ¬μ© ν  μ μμ΅λλ€. μ½μ, μ­μ , μ‘°ν μ°μ°μ΄ λ°ν¬μ μ λλ¨μμλ§ μ΄λ£¨μ΄μ§λ κ΅¬μ‘°μλλ€.