---
title: "List Tinkering with Streams: Filter & Find"
url: ./array-filter-find/
date: 2021-06-04T18:05:45+01:00
draft: true
image: /images/java-logo.png
categories:
    - howto
tags:
    - java
    - streams
    - filter
    - find
---

To show off (even more) on CodeWars.

<!--more-->

## Java Streams

![stream](../../../images/stream.gif)

Introduced in Java 8, the Stream API is used to process collections of objects.
It's a system formed by a sequence of methods which can be pipelined to produce a desired result.

-   It's not a data structure although it takes input from them.
-   They don't change the input, they only provide the result as per the pipelined methods.
-   Each operation returns a stream as a result, which allows intermediate operations to be pipelined.
-   Terminal operations end the stream and return the result.
-   Examples of intermediate operators: map, filter, sorted.
-   Examples of terminal operators: collect, foreach, reduce.

Example:

```Java
List<Integer> list = Arrays.asList(3, 6, 9, 12, 15);
list.stream().haveAtIt();
```

## Filter

**Returns a Stream** with the results of applying the given filter to the elements of the incoming Stream.

To put it simply: `inputStream > Filter(myFilter) > outputStream`.
Where `outputStream` is the result of applying `myFilter` to `inputStream`.

Said function takes the form `x -> x.filter`.
In practice, following the previous example:

```Java
List<Integer> list = Arrays.asList(3, 6, 9, 12, 15);

list.stream()
	.map(num -> num > 5)
	.forEach(System.out::println);
```

`Output: 9 12 15`

We can also work with methods from the filter:

```Java
List<String> list = Arrays.asList("this", "is", "a", "test");

list.stream()
	.filter(str -> str.length() > 3)
	.forEach(System.out::println);
```

`Output: this test`

Plus, there's the possibility of filtering by more than one criteria:

```Java
List<String> list = Arrays.asList("this", "is", "another", "test");

List<String> answer = list.stream()
	.filter(str -> str.length() > 3 && str.startsWith("a"))
	.collect(Collectors.toList());

System.out.println(answer);
```

`Output: [another]`

## Find

Returns **only one result** from within the elements of the incoming Stream.

There's **two** variantes of the Find algorithm in Java:

-   **findFirst**: returns precisely the first element of the Stream, given that there is an encounter order. Else, it acts like **findAny**.
    According to the java.util.streams package documentation, “Streams may or may not have a defined encounter order. It depends on the source and the intermediate operations.”

-   **findAny**: returns an element of the Stream without considering its order.

    -   **Within a non-parallel operation, it will probably return the first element in the Stream, but there is no guarantee.**

Simply put: `inputStream > Find > result`.

Since `findFirst` is more precise and predictable than `findAny`, especially in asynchronous processes, we'll use it as the example going forward.

```Java
String[] array = { "Stream", "Java", "Rule" };

Optional<String> combined = Arrays.stream(array).sorted().findFirst();

if (combined.isPresent())
	System.out.println(combined.get());
```

`Output: Java`

By default, `findFirst` will return an `Optional` of the type it finds in the incoming Stream.
This forces us to check that there is actually something in said `Optional` (using `isPresent()`) and then obtain it explicitly (with a `get()`).

One way to force the return of a primitive type is as follows:

```Java
int first = IntStream.range(2, 8)
	.findFirst()
	 .orElse(-1);

System.out.println("The lesser is: " + first);
```

`Output: The lesser is: 2`

## Together but not mixed

```Java
class ReduceMe {

    public static void main(String[] args) {

    	List<Invoice> invoices = Arrays.asList(new Invoice("01", 5.69f, 2),
    			new Invoice("02", 10.99f, 15),
    			new Invoice("03", 14.34f, 10));


        Invoice minInvoiceQtyGreaterThanFive = invoices.stream()
                .filter(x -> x.getQty() > 5) // filter
                .sorted((greaterQtyInvoice, lesserQtyInvoice) ->
                                (lesserQtyInvoice.getQty() < greaterQtyInvoice.getQty()) ? 1 : -1)
                .findFirst() // find
                .orElse(new Invoice("00", 0f, -1));

        System.out.println(minInvoiceQtyGreaterThanFive.qty); // 10
    }

}

class Invoice {

    String id;
    float price;
    int qty;

    public Invoice(String id, float price, int qty) {
    	this.id = id;
    	this.price = price;
    	this.qty = qty;
    }

    public float getPrice() {
    	return price;
    }

    public int getQty() {
    	return qty;
    }

}
```
