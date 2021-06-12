---
title: "Kata: Potter-Kata"
url: ./potter-kata/
date: 2021-06-12T12:25:27+01:00
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

Facturar ofertas puede ser un lío.

<!--more-->

![hair](../../../images/turk-hair.gif)

## [Problema](https://github.com/MikeDigitize/Potter-kata)

Crea un mini-programa que calcule el precio de cualquier cesta de la compra (ofreciendo el mayor descuento posible) compuesta por libros de la saga de Harry Potter.

### Condiciones

-   Consideramos 5 libros en la saga, identificados por su número.
-   Una copia de cualquier libro cuesta 8€.
-   Al comprar dos libros distintos, el cliente obtiene un 5% de descuento.
-   Al comprar tres libros distintos, el cliente obtiene un 10% de descuento.
-   Al comprar cuatro libros distintos, el cliente obtiene un 20% de descuento.
-   Al comprar los cinco libros, el cliente obtiene un 25% de descuento.
-   Al comprar cuatro libros, de los cuales tres no son repetidos, el cliente obtiene un 10% de descuento en esos tres. Sin embargo el cuarto libro costará 8€.
    -   Igualmente, al comprar cinco libros, de los cuales dos son repetidos, el cliente obtiene un 10% de descuento en los tres no repetidos pero pagará el precio completo de los dos repetidos (16€).

### Ejemplo

2 copias del primer libro, 2 copias del segundo libro, 2 copias del tercer libro, 1 copia del cuarto libro y 1 copia del quinto libro.

```
(5 * 8) - 25% [primero libro, segundo libro, tercer libro, cuarto libro, quinto libro] = 30
+
(3 * 8) - 10% [primero libro, segundo libro, tercer libro] = 21.60
=
51.60
```

### Solución

[Spoiler!](http://unixmagick.xyz/es/potter-kata/#voilá)

## Proceso

A título personal, he querido evitar estructuras tipo `switch` y procurar que el código valiera para 5 libros posibles cómo para 300.
Finalmente he decidido que el funcionamiento se mueva alrededor de los descuentos disponibles (o no disponibles), de modo que ésto determinará la cantidad de libros y packs a considerar.

### Preparación

Ya que el [proyecto](https://github.com/MikeDigitize/Potter-kata) en el que me baso está en JavaScript y que tener variables no tipadas me produce urticaria, usaremos TypeScript para realizar la Kata.

De hecho podemos aprovechar que ya hay una batería de pruebas en el proyecto facilitado y desarrollar contra ésta.

TDD FTW!
<sub>No he programado demasiado con TypeScript por lo que seguramente haya un mejor refactor que se me escapa :^)</sub>

### Tests Primero

Creamos un `potter.ts` desde el que exportamos la clase Potter y nos copiamos los [tests del original](https://github.com/MikeDigitize/Potter-kata/blob/master/__tests__/potter.test.js) a un archivo `potter.tests.ts` desde el que haremos un `import { Potter } from './potter'`

Ésto debería producir un puñado de errores en en los tests, ya que habrá un par de métodos y una variable sin declarar.
El mismo IDE nos propondrá declararlos en automático y si accedemos tendremos algo así en `potter.ts`:

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

Fijándonos en la batería de tests podremos inferir ciertas características de `potter.ts` que nos facilitarán la vida:

-   Línea 8: `potter.addToBasket(potter.createBook(number))`

    -   Por lo que `addToBasket()` deberá recibir un libro por parámetro: `addToBasket(book: Book)`
    -   Podemos inferir que `basket` debe ser un array de libros: `basket: Book[] = []`

-   Línea 9: `expect(potter.checkout()).toBe(8)`

    -   Por lo que `checkout()` debe devolver un número:
        `checkout(): number { return -1 }`

-   Línea 20: `const book = potter.createBook(1)`

    -   Por lo que `createBook()` debe devolver un libro con su número correspondiente:
        `createBook(bookNumber: number): Book { return new Book(bookNumber) }`
    -   Ésto producirá un error en `Book`, nos encargaremos luego.

-   Línea 22: `expect(potter.basket).toHaveLength(1)`
    -   Confirma la suposición anterior sobre `basket` y podemos completar con
        `addToBasket(book: Book) { this.basket.push(book) }`

Por lo pronto deberíamos tener algo así:

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

Añadimos la clase `Book` que nos solicita el IDE:

```typescript
class Book {
    constructor(protected bookNumber: number) {}
}
```

Si ejecutamos los tests todo debería compilar sin problemas (y fallar por completo).
Ahora podemos comenzar a resolver la Kata, preocupándonos ante todo de poner los tests en verde.

### TDD

Hay unos cuantos tests.
La documentación de Jest sugiere que al establecer porqué falla un tests, lo suyo sería ejecutarlo en solitario añadiendo `.only` a la declaración del test en rojo.

Yo fui añadiendo el tag a los tests de uno en uno, de forma que al llegar al último estarían todos activados.

#### Test 1

Por lo pronto nos conformaremos con hacer que `checkout()` devuelva 8.

#### Test 2

En el mensaje de error podemos ver que el problema está en el nombre de la variable de `Book`:
`class Book { constructor(protected number: number) {} }`

#### Test 4

Siendo estrictos con el TDD, lo pondremos en verde así:

```typescript
checkout(): number {
	return this.price * this.basket.length
}
```

Aunque ya empieza a oler la cosa...

#### Test 5 - 8

Ahora si que comienza la fiesta.

Podríamos enfocarlo test a test (y tal vez sea lo mejor), pero éste grupo básicamente comprueba la misma funcionalidad pero con diferentes descuentos a aplicar.
Creo que de hacerlo así no haríamos más que mandar el problema más adelante, por lo que los consideraremos en su conjunto.

Una de las cosas que llama la atención es que **no estamos realmente interesados en la cantidad de libros, sino en la cantidad de packs y, en segundo lugar, la cantidad de libros por cada pack**.

Una manera de resolver el dilema podría ser obtener, a partir de `Book[]`, un array en el que el **índice** nos diga la **cantidad de libros** en el pack, mientras que el **valor** para dicho índice nos diga la **cantidad de packs** con esa cantidad de libros.
Algo así:

-   `[ 3, 0, 0, 0, 0 ]` implica **tres** packs de **un solo** libro. Es decir, tres veces el mismo libro, por lo que no hay descuento.
-   `[ 0, 1, 0, 0, 0 ]` implica **un solo** pack de **dos** libros. Es decir, dos libros distintos, por lo que hay un 5% de descuento.

Algo como ésto debería poder contemplar de un golpe los diferentes packs y sus descuentos, así como los libros sueltos (packs de uno).

Para ésto, tendremos que ordenar previamente los libros por cantidad de números repetidos:

```typescript
sortBooksByNumberQty(): number[] {

	let qtyOfBooksByNumber = [0, 0, 0, 0, 0]

	for (let i = 0; i < this.basket.length; i++) {

		qtyOfBooksByNumber[this.basket[i].number ] += 1
	}
	return qtyOfBooksByNumber.sort()
}
```

Por ejemplo:

-   `[ 3, 2, 1, 0, 0 ]` implica **tres** libros del mismo número (tres libros iguales), **dos** libros del mismo número, pero distinto al anterior y **un solo** libro '_del mismo número_' y distinto a los dos grupos anteriores (algo redundante, pero simplifica el cálculo de libros sueltos).

Hecho ésto, podremos volver a la idea previa:

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

Ahora, desde `checkout()`, llamaremos ambos métodos tal que:
`const packsByBooksIncluded = this.orderBooksInPacks(this.sortedBooksByNumberQty()) `

También tendremos que aplicar el descuento adecuado por cada pack, maximizando el descuento (simple, ya que nos llega el array ya ordenado).

Tener un array tipo `discounts = [0, 0.05, 0.1, 0.2, 0.25]` debería facilitarnos la vida. Usamos 0 como 'ningún descuento'.

Así, en `checkout()`:

```typescript
let total: number = 0;

for (let i = 0; i < packsByBooksIncluded.length; i++) {
    //para cada pack

    if (packsByBooksIncluded[i] > 0) {
        //si el pack contiene libros

        let numberOfBooksInPack = i + 1; //arrays comienzan en 0

        let applicableDiscount = this.discounts[i]; //selecciona descuento en función del tamaño del pack

        let discountedAmmount =
            numberOfBooksInPack * this.price * applicableDiscount; //cantidad a descontar

        let discountedPrice =
            numberOfBooksInPack * this.price - discountedAmmount; //aplica el descuento al precio completo del pack

        total += packsByBooksIncluded[i] * discountedPrice; //suma el resultado al total
    }
}
return total;
```

Habrá que hacer algo de refactor para facilitar la legibilidad del código y...

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

Sin quererlo ni beberlo, resulta que el resto de los tests también están verdes!
Viva!

![easy](../../../images/easy.gif)
