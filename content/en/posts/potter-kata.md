---
url: ./potter-kata/
date: 2021-06-12T12:25:53+01:00
title: "Kata: Potter-Kata"
draft: true
image: /images/kata.png
categories:
    - howto
tags:
    - kata
    - typescript
    - array
    - TDD
---

Billing and discounts can get messy.

<!--more-->

![hair](../../../images/turk-hair.gif)

## [Problem](https://github.com/MikeDigitize/Potter-kata)

Create a mini-program that calculates the price of any conceivable shopping basket, giving as big a discount as possible.

### Conditions

-   We'll consider 5 books in the saga, identified by number.
-   One copy on whichever book costs 8€.
-   Buying two different books, the client gets a 5% discount.
-   Buying three different books, the client gets a 10% discount.
-   Buying four different books, the client gets a 20% discount.
-   Buying five different books, the client gets a 25% discount.
-   Buying four books, of which three are different from one another, the client gets a 10% discount in those three books. However, the fourth one would still cost 8€.
    -   Similarly, buying five books of which two are repeated, the client will get a 10% discount in the remaining three books but will pay full price for the repeated two (16€).

### Example

2 copies of the first book 2 copies of the second book 2 copies of the third book 1 copy of the fourth book 1 copy of the fifth book.

```
(5 * 8) - 25% [first book, second book, third book, fourth book, fifth book] = 30
+
(3 * 8) - 10% [first book, second book, third book] = 21.60
=
51.60
```

### Solution

[Spoiler!](http://unixmagick.xyz/en/potter-kata/#voilá)

## Process

On a personal note, I wanted to avoid `switch` type structures and try to make the code capable of handling 5 books or 300 just the same.
Finally I decided to make the logic dance around the available discounts (or lack there of), so that this will dictate the amount of books and packs to consider.

### Preparation

Since the [project](https://github.com/MikeDigitize/Potter-kata) which I'm basing this on is written in JavaScript and since having loosely typed variables gives me nightmares, we'll use TypeScript for this one.

In fact, we can utilize the tests included in said project to develop against them!

TDD FTW!
<sub>I'm no TypeScript expert so there's probably a better refactor I'm missing :^)</sub>

### Tests First

Make a `potter.ts` from which to export the Potter class and copy the [tests from the original](https://github.com/MikeDigitize/Potter-kata/blob/master/__tests__/potter.test.js) to a `potter.tests.ts` file, from which we'll `import { Potter } from './potter'`

This should create a bunch of errors in the tests, since we are missing a couple of methods and a variable.
The IDE will suggest declaring them automagically and if we go for it we'll have somthing like this in our `potter.ts`:

```
basket: any;
checkout(): any {
	throw new Error('Method not implemented.');
}
createBook(number: number): any {
	throw new Error('Method not implemented.');
}
addToBasket(arg0: any) {
	throw new Error('Method not implemented.');
}
```

Looking closely to the tests we'll be able to infer some of `potter.ts` characteristics, which will make our lives a bit easier.

-   Line 8: `potter.addToBasket(potter.createBook(number))`

    -   So `addToBasket()` will recieve a book: `addToBasket(book: Book)`
    -   We can infer that `basket` must be an array of books: `basket: Book[] = []`

-   Line 9: `expect(potter.checkout()).toBe(8)`

    -   So `checkout()` must return a number:
        `checkout(): number { return -1 }`

-   Line 20: `const book = potter.createBook(1)`

    -   So `createBook()` must return a book with it's corresponding number:
        `createBook(bookNumber: number): Book { return new Book(bookNumber) }`
    -   This will throw an error in `Book`, we'll deal with it later.

-   Line 22: `expect(potter.basket).toHaveLength(1)`
    -   Confirms the previous assumption about `basket` so we can complete with
        `addToBasket(book: Book) { this.basket.push(book) }`

So far we should have something like this:

```typescript
export class Potter {
    basket: Book[] = [];

    checkout(): number {
        return -1;
    }

    createBook(bookNumber: number): Book {
        return new Book(bookNumber);
    }

    addToBasket(book: Book) {
        this.basket.push(book);
    }
}
```

Add the `Book` class:

```typescript
class Book {
    constructor(protected bookNumber: number) {}
}
```

If we run the tests everything should compile correctly (and fail completely).
Now we can start solving the Kata, worrying above all else about getting those tests green.

### TDD

There's quite a few tests.
Jest documentation suggests that if a test is failing, one should run it by itself. This is achieved by appending `.only` to the red test declaration.

I just kept adding the tag to each test one by one, so that by the end of it, they were all running.

#### Test 1

For now, well make do by having `checkout()` return 8.

#### Test 2

The error message tells us that the issue es related to the variable name from `Book`:
`class Book { constructor(protected number: number) {} }`

#### Test 4

Being strict with TDD, we'll make it pass by adding:

```typescript
checkout(): number {
	return this.price * this.basket.length
}
```

Although it is looking a bit off...

#### Test 5 - 8

Now we are talking.

We could look at these tests one at the time (and it might be better that way), but this group of tests basically checks the same functionality but for different discounts to be applied.
I reckon that we would just be kicking the problem foreward, so we'll take them into account as a whole.

One thing that comes to my attention is that we are **not actually interested in the quantity of books, but rather in the quantity of packs and, secondly, in the quantity of books per pack**.

One way to solve the issue might be to obtain, from `Book[]`, an array where the **index** tells us the **quantity of books** in the pack, while the **value** for that index tells the **quantity of packs** with such an amount of books.
Something like:

-   `[ 3, 0, 0, 0, 0 ]` implies **three** packs of only **one** book. As in three times the same book, so no discount.
-   `[ 0, 1, 0, 0, 0 ]` implies only **one** pack of **two** books. As in two different books, so 5% discount.

Something like that should be able to manage both different packs and discounts, as well as 'orphaned' books (packs of one).

For this, we'll have to first sort the books by quantity of repeating numbers:

```typescript
sortBooksByNumberQty(): number[] {

	let qtyOfBooksByNumber = [0, 0, 0, 0, 0]

	for (let i = 0; i < this.basket.length; i++) {

		qtyOfBooksByNumber[this.basket[i].number ] += 1
	}
	return qtyOfBooksByNumber.sort()
}
```

For example:

-   `[ 3, 2, 1, 0, 0 ]` implies **three** books of the same number (three identical books), **two** books of the same number, but different from the previous one and **one** book '_of the same number_' and different from the previous two groups (a bit redundant, but will simplify managing orphaned books).

Now, we can get back to the previous idea:

```typescript
sortBooksInPacks(orderedBooksByQtyOfEachNumber: number[]): number[] {

	let qtyOfPacksByBooksIncluded: number[] = [0, 0, 0, 0, 0]

	for (let i = 0; i < orderedBooksByQtyOfEachNumber.length; i++) {

		if (orderedBooksByQtyOfEachNumber[i] > 0) {
			qtyOfPacksByBooksIncluded[i] = orderedBooksByQtyOfEachNumber[i]
			orderedBooksByQtyOfEachNumber = orderedBooksByQtyOfEachNumber
				.map((b) => { return (b > 0) ? b - orderedBooksByQtyOfEachNumber[i] : b })
		}
	}
	return qtyOfPacksByBooksIncluded.reverse()
}
```

Now, from `checkout()`, we'll call both methods like so:
`const packsByBooksIncluded = this.orderBooksInPacks(this.sortedBooksByNumberQty()) `

We have to apply the adequate discount for each pack, maximizing the discount (should be easy, since the array is already sorted).

Having an array like `discounts = [0, 0.05, 0.1, 0.2, 0.25]` should make our lives easier. Use 0 as 'no discount'.

So, in `checkout()`:

```typescript
let total: number = 0;

for (let i = 0; i < packsByBooksIncluded.length; i++) {
    //for each pack available

    if (packsByBooksIncluded[i] > 0) {
        //if there are books included in said sized pack

        let numberOfBooksInPack = i + 1; //arrays start at 0

        let applicableDiscount = this.discounts[i]; //select discount given size of pack

        let discountedAmmount =
            numberOfBooksInPack * this.price * applicableDiscount; //amount to be discounted

        let discountedPrice =
            numberOfBooksInPack * this.price - discountedAmmount; //apply it to the full price of the pack

        total += packsByBooksIncluded[i] * discountedPrice; //and sum the result to the total
    }
}
return total;
```

There will be a bit of refactoring to do to make the code actually readable and...

#### Voilá!

```typescript
export class Potter {
    price = 8;
    discounts = [0, 0.05, 0.1, 0.2, 0.25];
    basket: Book[] = [];

    checkout(): number {
        const qtyOfPacksBySize = this.sortBooksInPacks(
            this.sortBooksByQtyOfRepeatingNumbers()
        );
        let total = 0;

        for (let pack = 0; pack < qtyOfPacksBySize.length; pack++) {
            if (qtyOfPacksBySize[pack] > 0) {
                let numberOfBooksInPack = pack + 1;

                let applicableDiscount = this.discounts[pack];

                let subtotal = numberOfBooksInPack * this.price;

                let discountedPriceForPack =
                    subtotal - subtotal * applicableDiscount;

                total += qtyOfPacksBySize[pack] * discountedPriceForPack;
            }
        }
        return total;
    }

    sortBooksInPacks(booksByQtyOfNumbers: number[]): number[] {
        let packs: number[] = new Array(this.discounts.length + 1).fill(0);

        for (let qty = 0; qty < booksByQtyOfNumbers.length; qty++) {
            if (booksByQtyOfNumbers[qty] > 0) {
                packs[qty] = booksByQtyOfNumbers[qty];
                booksByQtyOfNumbers = booksByQtyOfNumbers.map((q) => {
                    return q > 0 ? q - booksByQtyOfNumbers[qty] : q;
                });
            }
        }
        return packs.reverse();
    }

    sortBooksByQtyOfRepeatingNumbers(): number[] {
        let qtyOfBooksByNumber: number[] = new Array(
            this.discounts.length + 1
        ).fill(0);

        this.basket.forEach((b) => {
            qtyOfBooksByNumber[b.number] += 1;
        });
        return qtyOfBooksByNumber.sort();
    }

    createBook(bookNumber: number): Book {
        return new Book(bookNumber);
    }

    addToBasket(book: Book) {
        this.basket.push(book);
    }
}

class Book {
    constructor(public number: number) {}
}
```

Without even wanting to, the rest of the tests are now green!
Hurray!

![easy](../../../images/easy.gif)
