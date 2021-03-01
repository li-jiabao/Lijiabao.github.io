
# Erasure of Generic Types


During the type erasure process, the Java compiler erases all type parameters and replaces each with its first bound if the type parameter is bounded, or <tt>Object</tt> if the type parameter is unbounded.


Consider the following generic class that represents a node in a singly linked list:

```

public class Node&lt;T&gt; {

    private T data;
    private Node&lt;T&gt; next;

    public Node(T data, Node&lt;T&gt; next) {
        this.data = data;
        this.next = next;
    }

    public T getData() { return data; }
    // ...
}

```


Because the type parameter <tt>T</tt> is unbounded, the Java compiler replaces it with <tt>Object</tt>:

```

public class Node {

    private Object data;
    private Node next;

    public Node(Object data, Node next) {
        this.data = data;
        this.next = next;
    }

    public Object getData() { return data; }
    // ...
}

```


In the following example, the generic <tt>Node</tt> class uses a bounded type parameter:

```

public class Node&lt;T extends Comparable&lt;T&gt;&gt; {

    private T data;
    private Node&lt;T&gt; next;

    public Node(T data, Node&lt;T&gt; next) {
        this.data = data;
        this.next = next;
    }

    public T getData() { return data; }
    // ...
}

```


The Java compiler replaces the bounded type parameter <tt>T</tt> with the first bound class, <tt>Comparable</tt>:

```

public class Node {

    private Comparable data;
    private Node next;

    public Node(Comparable data, Node next) {
        this.data = data;
        this.next = next;
    }

    public Comparable getData() { return data; }
    // ...
}

```
