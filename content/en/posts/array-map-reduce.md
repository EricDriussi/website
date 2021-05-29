---
title: "List Tinkering with Streams: Map & Reduce"
url: ./array-map-reduce/
date: 2021-05-28T16:01:39+01:00
draft: true
image: /images/java-logo.png
categories:
    - howto
tags:
    - java
    - streams
    - map
    - reduce
---

To show off on CodeWars.

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

## Map

Also called '_Function Mapper_', **returns a Stream** with the results of applying the given function to the elements of the incoming Stream.

To put it simply: `inputStream > Map(myFunction) > outputStream`.
Where `outputStream` is the result of applying `myFunction` to `inputStream`.

Said function takes the form `x -> x.modificacion`.
In practice, following the previous example:

```Java
List<Integer> list = Arrays.asList(3, 6, 9, 12, 15);

list.stream()
	.map(num -> num * 3)
	.forEach(System.out::println);
```

`Output: 9 18 27 36 45`

We can also work with methods from the map:

```Java
List<String> list = Arrays.asList("this", "is", "a", "test");

list.stream()
	.map(str -> str.length())
	.forEach(System.out::println);
```

`Output: 4 2 1 4`

Since we are on Java 8 or above, we have access to method references (`::`):

```Java
List<String> list = Arrays.asList("this", "is", "another", "test");

List<String> answer = list.stream()
	.map(String::toUpperCase)
	.collect(Collectors.toList());

System.out.println(answer);
```

`Output: [THIS, IS, ANOTHER, TEST]`

Or we can throw the house out the window!

```Java
List<Person> person = Arrays.asList(
		new Person("Eric", 30),
		new Person("Jack", 27),
		new Person("Lawrence", 33)
);

List<Employee> employees = person.stream()
	.map(temp -> {

		Employee obj = new Employee();
		obj.setName(temp.getName());
		obj.setAge(temp.getAge());

		if ("Eric".equals(temp.getName())) {
			obj.setSalary(10000);
		}

		return obj;

	})
	.collect(Collectors.toList());

```

Given the classes `Person` and `Employee`, we can even **map** one object to another.

## Reduce

Produces one **single** result from a sequence of elements, by repeatedly applying a given combining operation to the elements in the incoming Stream.

There are **three** possible components in this operation:

-   **Identity** (optional): initial or default value, if the stream is empty.
-   **Accumulator**: function that takes two parameters:
    -   Partial result of the operation.
    -   Next element of the stream.
-   **Combiner** (optional): function used to combine the partial result of the reduction operation if under parallel execution or mismatch between the types of the accumulator.

Simply put: `inputStream > Reduce(myIdentity, myAccumulator, myCombiner) > result`.

Let's leave Identity and Combiner for later.

`myAccumulator` takes the form of `(x, y) -> x oepration y`, which is to say, a lambda.
In practice, with an example:

```Java
String[] array = { "Java", "Streams", "Rule" };

Optional<String> combined = Arrays.stream(array).reduce((str1, str2) -> str1 + "-" + str2);

if (combined.isPresent())
	System.out.println(combined.get());
```

`Output: Java-Streams-Rule`

By default, a Reduce will return an `Optional` of the type it finds in the incoming Stream.
This forces us to check that there is actually something in said `Optional` (using `isPresent()`) and then obtain it explicitly (with a `get()`).

One way to force the Reduce to return a primitive type is as follows:

```Java
int product = IntStream.range(2, 8)
		 .reduce((num1, num2) -> num1 * num2)
		 .orElse(-1);

System.out.println("The product is: " + product);
```

`Output: The product is: 5040`

Although more elegant and extensible would be to use the **Identity** component of the Reduce call.
Since it assigns a default value in case of null output, we can save ourselves a headache:

```Java
int product = IntStream.range(2, 8)
		 .reduce(0, (num1, num2) -> num1 * num2);

System.out.println("The product is: " + product);
```

`Output: The product is: 5040`

Regarding the **Combiner**, due to some quirks of the JVM when under parallel execution, we'll need it to combine the results of the substreams in one.
In the example we use a method reference as a **Combiner**. As we know, these are interchangeable with lambdas and viceversa since Java 8.

```Java
int sumAges = Arrays.asList(25, 30, 45, 28, 32)
			.parallelStream()
			.reduce(0, (a, b) -> (a + b), Integer::sum);

System.out.println(sumAges);
```

`Output: 160`

It's worth mentioning the fact that the **Combiner** will be also necessary if we manage different types in the Accumulator.
In the example, the Accumulator has an `int` as partial result, but a `User` as next element:

```Java
List<User> users = Arrays.asList( new User("Dacil", 30),
					new User("Gabriel", 35));
int result = users.stream()
		.reduce(0, (partialAge, user) -> (partialAge + user.getAge()), Integer::sum);
```

## Together but not mixed

```Java
class ReduceMe {

    public static void main(String[] args) {

    	List<Invoice> invoices = Arrays.asList(new Invoice("01", 5.69f, 2),
    			new Invoice("02", 10.99f, 15),
    			new Invoice("03", 14.34f, 10));

    	float sum = invoices.stream().map(x -> x.getQty() * (x.getPrice())) // map
    			.reduce(0f, Float::sum); // reduce

    	System.out.println(sum); // 31.02
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
