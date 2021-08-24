---
title: "Kata: String Calculator"
url: ./kata-tdd/
date: 2021-08-23T16:49:39+01:00
draft: true
image: /images/kata.png
categories:
    - howto
tags:
    - kata
    - typescript
    - TDD
---

Easy start to TDD.

<!--more-->

![hair](../../../images/turk-hair.gif)

## [Problem](https://osherove.com/tdd-kata-1)

A rather simple calculator that takes a string as an input and returns a sum of the values in it.
Let's break it down test by test, following the [original post](https://osherove.com/tdd-kata-1).

## Conditions

### Take up to two numbers separated by commas, and return their sum. 

For input `""` return `0`.

We can easily think of a couple of tests to represent this rule:

```typescript
it('should return zero if input is empty', function () {
	expect(StringCalc.Add("")).toBe(0)
});
it('should return input if its only one number', function () {
	expect(StringCalc.Add("1")).toBe(1)
});
it('should return the sum if input has two numbers', function () {
	expect(StringCalc.Add("1,2")).toBe(3)
});
```

Baseline implementation might look something like this:

```typescript
static Add(numbers: string): number {
	if(numbers.length > 0) {
		let arrayOfNumbers = numbers.split(",");
		if(arrayOfNumbers.length == 1) return parseInt(arrayOfNumbers[0])
		else return parseInt(arrayOfNumbers[0]) + parseInt(arrayOfNumbers[1])
	}else return 0
}
```

After checking that our tests pass, we can refactor the code a bit:

```typescript
static Add(numbers: string): number {
	if(numbers.length == 0) return 0
	return numbers
		.split(",")
		.map(number => parseInt(number))
		.reduce((sum, number) => sum = number);
}
```

We might be getting ahead of ourselves here, since the first condition only contemplates up to two numbers but we are already implementing a reduce function.
That being said, given that we intend to implement a string calculator, I don't think it's outrageous to consider an unlimited amount of numbers at this stage.

### Handle new lines between numbers (instead of commas).

As in `“1\n2,3”` will equal `6`.
Write it as a test:

```typescript
it('should handle both commas and new-lines', function () {
	expect(StringCalc.Add("1,2\n3")).toBe(6)
});
```

We could try and process both cases independently but a simpler solution might be to replace line breaks with commas and just do as before.
Update the function:

```typescript
static Add(numbers: string): number {
	if(numbers.length == 0) return 0
	return numbers
		.replace(/\n/g, ",") // A little regex won't hurt
		.split(",")
		.map(number => parseInt(number))
		.reduce((sum, number) => sum = number);
}
```
### Support different delimiters 

The beginning of the string will contain a separate line that looks like `"//;\n1;2"` where the delimiter is `;`.
So following the example above:

```typescript
it('should handle weird delimiters', function () {
	expect(StringCalc.Add("//;\n1;2")).toBe(3)
});
```

This case already implies that there will be an unknown amount of possible delimiters.
That forces us to choose: Either we read the delimiter from our input using the first line break as reference, or...

```typescript
static Add(numbers: string): number {
	if(numbers.length == 0) return 0
	return numbers
		.replace(/[^\d]/g, ",") // Just replace any non-digit with commas
		.split(",")
		.map(number => parseInt(number))
		.reduce((sum, number) => sum = number);
}
```

We are building a string calculator, no way we are going to use non numbers for any logical operation. At least not for now.

### Calling Add with a negative number should throw an exception.

The exception should read `"negative not allowed: -given negative number/s-"`.
In case of multiple negative numbers, list them all in the message.

So in lay mans words, the function should break in a controlled fashion when the input contains negative numbers.
A couple of possible scenarios come to mind:

```typescript
it('should throw with one negative number', function () {
	expect(()=>StringCalc.Add("-3, 1, 2"))
		.toThrowError(Error("negatives not allowed: -3"))
});
it('should throw with ONLY one negative number', function () {
	expect(()=>StringCalc.Add("-3"))
		.toThrowError(Error("negatives not allowed: -3"))
});
it('should throw with multiple negative numbers', function () {
	expect(()=>StringCalc.Add("1, -3, 2, -4"))
		.toThrowError( Error("negatives not allowed: -3,-4"))
});
```

Here the delimiters from before don't really matter, so one simple fix to include negative numbers would be:

```typescript
static Add(numbers: string): number {
	if(numbers.length == 0) return 0
	return numbers
		.replace(/[^\d-]/g, ",") // It will now spare - signs
		.split(",")
		.map(number => parseInt(number))
		.reduce((sum, number) => sum = number);
}
```

Still, we'll have to implement two courses of action depending on the presence of negative numbers.

```typescript
static Add(numbers: string): number {
	if(numbers.length == 0) return 0
	let arrayOfNumbers = numbers
		.replace(/[^\d-]/g, ",")
		.split(",")
		.map(number => parseInt(number));
	let arrayOfNegativeNumbers = arrayOfNumbers.filter(number => number < 0);
		if (arrayOfNegativeNumbers.length > 0) {
			throw new Error("negatives not allowed: " + arrayOfNegativeNumbers)
		}
	let arrayOfPositiveNumbers = arrayOfNumbers.filter(number => number > 0);
	return arrayOfPositiveNumbers.reduce((sum, number) => sum = number)
}
```

Come to think of it, we can do without the first `if` simply by completing our reduce method in the return statement.

```typescript
static Add(numbers: string): number {
	let arrayOfNumbers = numbers
		.replace(/[^\d-]/g, ",")
		.split(",")
		.map(number => parseInt(number));
	let arrayOfNegativeNumbers = arrayOfNumbers.filter(number => number < 0);
		if (arrayOfNegativeNumbers.length > 0) {
			throw new Error("negatives not allowed: " + arrayOfNegativeNumbers)
		}
	let arrayOfPositiveNumbers = arrayOfNumbers.filter(number => number > 0);
	return arrayOfPositiveNumbers.reduce((sum, number) => sum = number, 0)
}
```

### Numbers bigger than 1000 should be ignored

As in `2 + 1001 = 2`.
Let's make a test out of the example:

```typescript
it('should discard numbers greater than 1000', function () {
	expect(StringCalc.Add("1, 3, 2, 1001")).toBe(6)
});
```

We could easily fit this case within our reduce method, making it so that it only applies to numbers lesser than or equal to `1000`:

```typescript
static Add(numbers: string): number {
	let arrayOfNumbers = numbers
		.replace(/[^\d-]/g, ",")
		.split(",")
		.map(number => parseInt(number));
	let arrayOfNegativeNumbers = arrayOfNumbers.filter(number => number < 0);
		if (arrayOfNegativeNumbers.length > 0) {
			throw new Error("negatives not allowed: " + arrayOfNegativeNumbers)
		}
	let arrayOfPositiveNumbers = arrayOfNumbers.filter(number => number > 0);
	return arrayOfPositiveNumbers
		.reduce((sum, number) => (number <= 1000) ? sum = number : sum, 0)
}
```

### All done!

There are a couple more conditions in the original post but we already covered them all with our implementation!
Cases like these should already work as expected:

```typescript
it('should handle VERY weird delimiters', function () {
	expect(StringCalc.Add("//[***]\n1***2***3")).toBe(6)
});
it('should handle various VERY weird delimiters', function () {
	expect(StringCalc.Add("//[*][%]\n1*2%3")).toBe(6)
});
```

All green all good!
