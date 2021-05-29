---
url: ./array-map-reduce/
date: 2021-05-28T16:01:39+01:00
draft: true
title: "Work In Progress..."
---

Working on it...

<!--more-->

![logbook](../../../images/wip.gif)

<!--

title: "List Tinkering with Streams: Map & Reduce"

Introduced in Java 8, the Stream API is used to process collections of objects. A stream is a sequence of objects that supports various methods which can be pipelined to produce the desired result.
The features of Java stream are –

A stream is not a data structure instead it takes input from the Collections, Arrays or I/O channels.
Streams don’t change the original data structure, they only provide the result as per the pipelined methods.
Each intermediate operation is lazily executed and returns a stream as a result, hence various intermediate operations can be pipelined. Terminal operations mark the end of the stream and return the result.

More specifically, reduction stream operations allow us to produce one single result from a sequence of elements, by repeatedly applying a combining operation to the elements in the sequence.

Before we look deeper into using the Stream.reduce() operation, let's break down the operation's participant elements into separate blocks. That way, we'll understand more easily the role that each one plays.

Identity – an element that is the initial value of the reduction operation and the default result if the stream is empty
Accumulator – a function that takes two parameters: a partial result of the reduction operation and the next element of the stream
Combiner – a function used to combine the partial result of the reduction operation when the reduction is parallelized or when there's a mismatch between the types of the accumulator arguments and the types of the accumulator implementation

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
-->
