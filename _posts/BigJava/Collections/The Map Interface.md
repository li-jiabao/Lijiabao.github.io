
# The Map Interface

A 
[`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html) is an object that maps keys to values. A map cannot contain duplicate keys: Each key can map to at most one value. It models the mathematical *function* abstraction. The `Map` interface includes methods for 
basic operations (such as `put`, `get`, `remove`, 
`containsKey`, `containsValue`, `size`, and `empty`),
bulk operations (such as `putAll` and `clear`), and
collection views (such as `keySet`, `entrySet`, and `values`).


```

public interface Map&lt;K,V&gt; {

    // Basic operations
    V put(K key, V value);
    V get(Object key);
    V remove(Object key);
    boolean containsKey(Object key);
    boolean containsValue(Object value);
    int size();
    boolean isEmpty();

    // Bulk operations
    void putAll(Map&lt;? extends K, ? extends V&gt; m);
    void clear();

    // Collection Views
    public Set&lt;K&gt; keySet();
    public Collection&lt;V&gt; values();
    public Set&lt;Map.Entry&lt;K,V&gt;&gt; entrySet();

    // Interface for entrySet elements
    public interface Entry {
        K getKey();
        V getValue();
        V setValue(V value);
    }
}

```

The Java platform contains three general-purpose `Map` implementations: 
[`HashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html), 
[`TreeMap`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html), and 
[`LinkedHashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashMap.html). Their behavior and performance are precisely analogous to `HashSet`, `TreeSet`, and `LinkedHashSet`, as described in 
[The Set Interface](set.html) section. 


The remainder of this page discusses the `Map` interface in detail. 
But first, here are some more examples of collecting to `Map`s using JDK 8 aggregate operations. Modeling real-world objects is a common task in object-oriented programming, so it is reasonable to think that some programs might, for example, group employees by department:


```

// Group employees by department
Map&lt;Department, List&lt;Employee&gt;&gt; byDept = employees.stream()
.collect(Collectors.groupingBy(Employee::getDepartment));

```


Or compute the sum of all salaries by department:


```

// Compute sum of salaries by department
Map&lt;Department, Integer&gt; totalByDept = employees.stream()
.collect(Collectors.groupingBy(Employee::getDepartment,
Collectors.summingInt(Employee::getSalary)));

```


Or perhaps group students by passing or failing grades:


```

// Partition students into passing and failing
Map&lt;Boolean, List&lt;Student&gt;&gt; passingFailing = students.stream()
.collect(Collectors.partitioningBy(s -&gt; s.getGrade()&gt;= PASS_THRESHOLD)); 

```


You could also group people by city:


```

// Classify Person objects by city
Map&lt;String, List&lt;Person&gt;&gt; peopleByCity
         = personStream.collect(Collectors.groupingBy(Person::getCity));

```


Or even cascade two collectors to classify people by state and city:


```

// Cascade Collectors 
Map&lt;String, Map&lt;String, List&lt;Person&gt;&gt;&gt; peopleByStateAndCity
  = personStream.collect(Collectors.groupingBy(Person::getState,
  Collectors.groupingBy(Person::getCity)))

```


Again, these are but a few examples of how to use the new JDK 8 APIs. For in-depth
coverage of lambda expressions and aggregate operations see the lesson entitled

[Aggregate Operations](../../collections/streams/index.html).


## Comparison to Hashtable

If you&#39;ve used `Hashtable`, you&#39;re already familiar with the general basics of `Map`. (Of course, `Map` is an interface, while `Hashtable` is a concrete implementation.) The following are the major differences:

- `Map` provides `Collection` views instead of direct support for iteration via `Enumeration` objects. `Collection` views greatly enhance the expressiveness of the interface, as discussed later in this section.
- `Map` allows you to iterate over keys, values, or key-value pairs; `Hashtable` does not provide the third option.
- `Map` provides a safe way to remove entries in the midst of iteration; `Hashtable` did not.

Finally, `Map` fixes a minor deficiency in the `Hashtable` interface. `Hashtable` has a method called `contains`, which returns `true` if the `Hashtable` contains a given *value*. Given its name, you&#39;d expect this method to return `true` if the `Hashtable` contained a given *key*, because the key is the primary access mechanism for a `Hashtable`. The `Map` interface eliminates this source of confusion by renaming the method `containsValue`. Also, this improves the interface&#39;s consistency &#151; `containsValue` parallels `containsKey`.

## Map Interface Basic Operations

The basic operations of `Map` (`put`, `get`, `containsKey`, `containsValue`, `size`, and `isEmpty`) behave exactly like their counterparts in `Hashtable`. The 
[`following program`](examples/Freq.java) generates a frequency table of the words found in its argument list. The frequency table maps each word to the number of times it occurs in the argument list.

```

import java.util.*;

public class Freq {
    public static void main(String[] args) {
        Map&lt;String, Integer&gt; m = new HashMap&lt;String, Integer&gt;();

        // Initialize frequency table from command line
        for (String a : args) {
            Integer freq = m.get(a);
            m.put(a, (freq == null) ? 1 : freq + 1);
        }

        System.out.println(m.size() + " distinct words:");
        System.out.println(m);
    }
}

```

The only tricky thing about this program is the second argument of the `put` statement. That argument is a conditional expression that has the effect of setting the frequency to one if the word has never been seen before or one more than its current value if the word has already been seen. Try running this program with the command:

```

java Freq if it is to be it is up to me to delegate

```

The program yields the following output.

```

8 distinct words:
{to=3, delegate=1, be=1, it=2, up=1, if=1, me=1, is=2}

```

Suppose you'd prefer to see the frequency table in alphabetical order. All you have to do is change the implementation type of the `Map` from `HashMap` to `TreeMap`. Making this four-character change causes the program to generate the following output from the same command line.

```

8 distinct words:
{be=1, delegate=1, if=1, is=2, it=2, me=1, to=3, up=1}

```

Similarly, you could make the program print the frequency table in the order the words first appear on the command line simply by changing the implementation type of the map to `LinkedHashMap`. Doing so results in the following output.

```

8 distinct words:
{if=1, it=2, is=2, to=3, be=1, up=1, me=1, delegate=1}

```

This flexibility provides a potent illustration of the power of an interface-based framework.

Like the 
[`Set`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)and 
[`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)interfaces, `Map` strengthens the requirements on the `equals` and `hashCode` methods so that two `Map` objects can be compared for logical equality without regard to their implementation types. Two `Map` instances are equal if they represent the same key-value mappings.

By convention, all general-purpose `Map` implementations provide constructors that take a `Map` object and initialize the new `Map` to contain all the key-value mappings in the specified `Map`. This standard `Map` conversion constructor is entirely analogous to the standard `Collection` constructor: It allows the caller to create a `Map` of a desired implementation type that initially contains all of the mappings in another `Map`, regardless of the other `Map`'s implementation type. For example, suppose you have a `Map`, named `m`. The following one-liner creates a new `HashMap` initially containing all of the same key-value mappings as `m`.

```

Map&lt;K, V&gt; copy = new HashMap&lt;K, V&gt;(m);

```

## Map Interface Bulk Operations

The `clear` operation does exactly what you would think it could do: It removes all the mappings from the `Map`. The `putAll` operation is the `Map` analogue of the `Collection` interface's `addAll` operation. In addition to its obvious use of dumping one `Map` into another, it has a second, more subtle use. Suppose a `Map` is used to represent a collection of attribute-value pairs; the `putAll` operation, in combination with the `Map` conversion constructor, provides a neat way to implement attribute map creation with default values. The following is a static factory method that demonstrates this technique.

```

static &lt;K, V&gt; Map&lt;K, V&gt; newAttributeMap(Map&lt;K, V&gt;defaults, Map&lt;K, V&gt; overrides) {
    Map&lt;K, V&gt; result = new HashMap&lt;K, V&gt;(defaults);
    result.putAll(overrides);
    return result;
}

```

## Collection Views

The `Collection` view methods allow a `Map` to be viewed as a `Collection` in these three ways:

- `keySet` &#151; the `Set` of keys contained in the `Map`.
- `values` &#151; The `Collection` of values contained in the `Map`. This `Collection` is not a `Set`, because multiple keys can map to the same value.
- `entrySet` &#151; the `Set` of key-value pairs contained in the `Map`. The `Map` interface provides a small nested interface called `Map.Entry`, the type of the elements in this `Set`.

The `Collection` views provide the *only* means to iterate over a `Map`. This example illustrates the standard idiom for iterating over the keys in a `Map` with a `for-each` construct:

```

for (KeyType key : m.keySet())
    System.out.println(key);

```

and with an `iterator`:

```

// Filter a map based on some 
// property of its keys.
for (Iterator&lt;Type&gt; it = m.keySet().iterator(); it.hasNext(); )
    if (it.next().isBogus())
        it.remove();

```

The idiom for iterating over values is analogous. Following is the idiom for iterating over key-value pairs.

```

for (Map.Entry&lt;KeyType, ValType&gt; e : m.entrySet())
    System.out.println(e.getKey() + ": " + e.getValue());

```

At first, many people worry that these idioms may be slow because the `Map` has to create a new `Collection` instance each time a `Collection` view operation is called. Rest easy: There's no reason that a `Map` cannot always return the same object each time it is asked for a given `Collection` view. This is precisely what all the `Map` implementations in `java.util` do.

With all three `Collection` views, calling an `Iterator`'s `remove` operation removes the associated entry from the backing `Map`, assuming that the backing `Map` supports element removal to begin with. This is illustrated by the preceding filtering idiom.

With the `entrySet` view, it is also possible to change the value associated with a key by calling a `Map.Entry`'s `setValue` method during iteration (again, assuming the `Map` supports value modification to begin with). Note that these are the *only* safe ways to modify a `Map` during iteration; the behavior is unspecified if the underlying `Map` is modified in any other way while the iteration is in progress.

The `Collection` views support element removal in all its many forms &#151; `remove`, `removeAll`, `retainAll`, and `clear` operations, as well as the `Iterator.remove` operation. (Yet again, this assumes that the backing `Map` supports element removal.)

The `Collection` views *do not* support element addition under any circumstances. It would make no sense for the `keySet` and `values` views, and it's unnecessary for the `entrySet` view, because the backing `Map`'s `put` and `putAll` methods provide the same functionality.

## Fancy Uses of Collection Views: Map Algebra

When applied to the `Collection` views, bulk operations (`containsAll`, `removeAll`, and `retainAll`) are surprisingly potent tools. For starters, suppose you want to know whether one `Map` is a submap of another &#151; that is, whether the first `Map` contains all the key-value mappings in the second. The following idiom does the trick.

```

if (m1.entrySet().containsAll(m2.entrySet())) {
    ...
}

```

Along similar lines, suppose you want to know whether two `Map` objects contain mappings for all of the same keys.

```

if (m1.keySet().equals(m2.keySet())) {
    ...
}

```

Suppose you have a `Map` that represents a collection of attribute-value pairs, and two `Set`s representing required attributes and permissible attributes. (The permissible attributes include the required attributes.) The following snippet determines whether the attribute map conforms to these constraints and prints a detailed error message if it doesn't.

```

static &lt;K, V&gt; boolean validate(Map&lt;K, V&gt; attrMap, Set&lt;K&gt; requiredAttrs, Set&lt;K&gt;permittedAttrs) {
    boolean valid = true;
    Set&lt;K&gt; attrs = attrMap.keySet();

    if (! attrs.containsAll(requiredAttrs)) {
        Set&lt;K&gt; missing = new HashSet&lt;K&gt;(requiredAttrs);
        missing.removeAll(attrs);
        System.out.println("Missing attributes: " + missing);
        valid = false;
    }
    if (! permittedAttrs.containsAll(attrs)) {
        Set&lt;K&gt; illegal = new HashSet&lt;K&gt;(attrs);
        illegal.removeAll(permittedAttrs);
        System.out.println("Illegal attributes: " + illegal);
        valid = false;
    }
    return valid;
}

```

Suppose you want to know all the keys common to two `Map` objects.

```

Set&lt;KeyType&gt;commonKeys = new HashSet&lt;KeyType&gt;(m1.keySet());
commonKeys.retainAll(m2.keySet());

```

A similar idiom gets you the common values.

All the idioms presented thus far have been nondestructive; that is, they don't modify the backing `Map`. Here are a few that do. Suppose you want to remove all of the key-value pairs that one `Map` has in common with another.

```

m1.entrySet().removeAll(m2.entrySet());

```

Suppose you want to remove from one `Map` all of the keys that have mappings in another.

```

m1.keySet().removeAll(m2.keySet());

```

What happens when you start mixing keys and values in the same bulk operation? Suppose you have a `Map`, `managers`, that maps each employee in a company to the employee's manager. We'll be deliberately vague about the types of the key and the value objects. It doesn't matter, as long as they're the same. Now suppose you want to know who all the "individual contributors" (or nonmanagers) are. The following snippet tells you exactly what you want to know.

```

Set&lt;Employee&gt; individualContributors = new HashSet&lt;Employee&gt;(managers.keySet());
individualContributors.removeAll(managers.values());

```

Suppose you want to fire all the employees who report directly to some manager, Simon.

```

Employee simon = ... ;
managers.values().removeAll(Collections.singleton(simon));

```

Note that this idiom makes use of `Collections.singleton`, a static factory method that returns an immutable `Set` with the single, specified element.

Once you've done this, you may have a bunch of employees whose managers no longer work for the company (if any of Simon's direct-reports were themselves managers). The following code will tell you which employees have managers who no longer works for the company.

```

Map&lt;Employee, Employee&gt; m = new HashMap&lt;Employee, Employee&gt;(managers);
m.values().removeAll(managers.keySet());
Set&lt;Employee&gt; slackers = m.keySet();

```

This example is a bit tricky. First, it makes a temporary copy of the `Map`, and it removes from the temporary copy all entries whose (manager) value is a key in the original `Map`. Remember that the original `Map` has an entry for each employee. Thus, the remaining entries in the temporary `Map` comprise all the entries from the original `Map` whose (manager) values are no longer employees. The keys in the temporary copy, then, represent precisely the employees that we're looking for.

There are many more idioms like the ones contained in this section, but it would be impractical and tedious to list them all. Once you get the hang of it, it's not that difficult to come up with the right one when you need it.

## Multimaps

A *multimap* is like a `Map` but it can map each key to multiple values. The Java Collections Framework doesn't include an interface for multimaps because they aren't used all that commonly. It's a fairly simple matter to use a `Map` whose values are `List` instances as a multimap. This technique is demonstrated in the next code example, which reads a word list containing one word per line (all lowercase) and prints out all the anagram groups that meet a size criterion. An *anagram group* is a bunch of words, all of which contain exactly the same letters but in a different order. The program takes two arguments on the command line: (1) the name of the dictionary file and (2) the minimum size of anagram group to print out. Anagram groups containing fewer words than the specified minimum are not printed.

There is a standard trick for finding anagram groups: For each word in the dictionary, alphabetize the letters in the word (that is, reorder the word's letters into alphabetical order) and put an entry into a multimap, mapping the alphabetized word to the original word. For example, the word **bad** causes an entry mapping **abd** into **bad** to be put into the multimap. A moment's reflection will show that all the words to which any given key maps form an anagram group. It's a simple matter to iterate over the keys in the multimap, printing out each anagram group that meets the size constraint.


[`The following program`](examples/Anagrams.java) is a straightforward implementation of this technique.

```

import java.util.*;
import java.io.*;

public class Anagrams {
    public static void main(String[] args) {
        int minGroupSize = Integer.parseInt(args[1]);

        // Read words from file and put into a simulated multimap
        Map&lt;String, List&lt;String&gt;&gt; m = new HashMap&lt;String, List&lt;String&gt;&gt;();

        try {
            Scanner s = new Scanner(new File(args[0]));
            while (s.hasNext()) {
                String word = s.next();
                String alpha = alphabetize(word);
                List&lt;String&gt; l = m.get(alpha);
                if (l == null)
                    m.put(alpha, l=new ArrayList&lt;String&gt;());
                l.add(word);
            }
        } catch (IOException e) {
            System.err.println(e);
            System.exit(1);
        }

        // Print all permutation groups above size threshold
        for (List&lt;String&gt; l : m.values())
            if (l.size() &gt;= minGroupSize)
                System.out.println(l.size() + ": " + l);
    }

    private static String alphabetize(String s) {
        char[] a = s.toCharArray();
        Arrays.sort(a);
        return new String(a);
    }
}

```

Running this program on a 173,000-word dictionary file with a minimum anagram group size of eight produces the following output.

```

9: [estrin, inerts, insert, inters, niters, nitres, sinter,
     triens, trines]
8: [lapse, leaps, pales, peals, pleas, salep, sepal, spale]
8: [aspers, parses, passer, prases, repass, spares, sparse,
     spears]
10: [least, setal, slate, stale, steal, stela, taels, tales,
      teals, tesla]
8: [enters, nester, renest, rentes, resent, tenser, ternes,
     treens]
8: [arles, earls, lares, laser, lears, rales, reals, seral]
8: [earings, erasing, gainers, reagins, regains, reginas,
     searing, seringa]
8: [peris, piers, pries, prise, ripes, speir, spier, spire]
12: [apers, apres, asper, pares, parse, pears, prase, presa,
      rapes, reaps, spare, spear]
11: [alerts, alters, artels, estral, laster, ratels, salter,
      slater, staler, stelar, talers]
9: [capers, crapes, escarp, pacers, parsec, recaps, scrape,
     secpar, spacer]
9: [palest, palets, pastel, petals, plates, pleats, septal,
     staple, tepals]
9: [anestri, antsier, nastier, ratines, retains, retinas,
     retsina, stainer, stearin]
8: [ates, east, eats, etas, sate, seat, seta, teas]
8: [carets, cartes, caster, caters, crates, reacts, recast,
     traces]


```

Many of these words seem a bit bogus, but that's not the program's fault; they're in the dictionary file. 
Here's the
[`dictionary file`](examples/dictionary.txt) we used.
It was derived from the Public Domain ENABLE benchmark reference word list.

