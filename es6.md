# ES5

#### dyrektywa 'use strict'

* Nie pozwala na używanie niezadeklarowanych zmiennych
* Nakłada parę restrykcji na eval
* Wyłącza przestarzałą funkcję with
* Wyłącza możliwość usuwania rzeczy, które nie powinny być usuwane 
* Nie pozwala na nadpisywanie słów kluczowych
* Umieszczamy ją na początku pliku/ funkcji

```javascript
'use strict'
x = 3.14    //błąd -- aby zadeklarować: this.x , let x, const x, var x itp
eval('var x = 2'); alert (x) // błąd - nie można używać zmiennych zadeklarowanych w eval w tym samym scopie
with(Math) { x = cos(2) } //błąd
delete Object.prototype //błąd
var let = 1500 //błąd
```
# ES6

### słowa kluczowe 'let' i 'const'

#### let

* Zmienne let mają zasięg blokowy
* Ponowna deklaracja zmiennej za pomocą słowa kluczowego let powoduje błąd składni

```javascript
'use strict'
function testVar() {
  for (var i = 0; i < 3; i += 1) { }
  console.log(i) // 3

  for (let x = 0; x < 3; x += 1) { }
  console.log(x) // ReferenceError: x is not defined
}

testVar()
```

#### const
* Działa jak let
* Wartość musi być przypisana w momencie deklaracji - wtedy i tylko wtedy
```javascript
'use strict'
const OBIAD = 'kebab'
OBIAD = 'pierogi' // błąd
```

### gruba strzałka () => {}
* this zależy od sposobu deklaracji, a nie sposobu w jaki [wywoła się funkcje](http://book.mixu.net/node/single.html#4-1-gotcha-1-this-keyword)
* Upraszcza zapis
```javascript
() => { ... } // bez parametru
x => { ... } // 1 parametr
(x, y) => { ... } // więcej niż jeden parametr
x => { return x * x }  // zapis jako blok
x => x * x  // to samo co wyżej, preferowany zapis dla krótkich funckji
```

### Klasy
* chyba nie trzeba tłumaczyć
* dodatkowo get i set wspieraja [lazy evaluation](https://www.quora.com/What-is-lazy-evaluation)
```javascript
'use strict'
class Prostokat {
  constructor(x, y) {
    this.x = x
    this.y = y
    this._wzorNaPole = ''
  }

  policzPole() {
    return this.x * this.y
  }

  get wzorNaPole() {
    return this._wzorNaPole
  }

  set wzorNaPole(wzorNaPoleProstokata) {
    console.log('wywolanie settera')
    this._wzorNaPole = wzorNaPoleProstokata
  }

  static statycznaMetoda () {
    console.log('statyczna metoda')
  }

}

Prostokat.statycznaMetoda()
let prostokat = new Prostokat(1, 2)
console.log(prostokat.policzPole())
prostokat.wzorNaPole = 'x * y'
console.log(prostokat.wzorNaPole)
```

##### Dziedziczenie za pomoca extends

```javascript
class Kwadrat extends Prostokat {
  constructor(x) {
    console.log('wywolanie z kwadratu')
    super(x, x)
  }

  policzObwod () {
    return this.x * 4
  }

}

let kwadrat = new Kwadrat(3)
console.log(kwadrat.policzPole())
console.log(kwadrat.policzObwod())
```

### Nowy zapis stringów ``
```javascript
` string na wiele
linii`

let x = 'interpolowanie'
`${x} stringu`
```

### Destrukturyzacja
* szybkie przypisanie wielu zmiennych z tablicy/ obiektu itp
* wyciagniecie np pierwszego i ostatniego elementu itp
```javascript
'use strict'
let [a, b, c] = [1, 2, 3]
let { foo, bar } = { foo: "lorem", bar: "ipsum" }
let [first, , third] = [1, 2, 3, 4, 5]
```
### Rozpakowanie
* przydatne gdy mamy tablice a funkcja przyjmuje np dwa argumenty itp
* kombinacje z destrukturyzacją
* szybkie kopiowanie tablic
```javascript
'use strict'
let func = (x, y) => x * y
func(...[2, 3]) // 6

let [a, ...tablicaBezPierwszegoElementu] = [1, 2, 3, 4, 5]

let arr1 = [1, 2, 3]
let arr2 = [...arr1]
```

### Iteratory
* obiekty podobne do funkcji zapamietujace swoj stan
* [lazy evaluation](https://www.quora.com/What-is-lazy-evaluation)
* wydajne
```javascript
  function* idMaker() {
    let index = 0
    while(true) {
      yield index++
    }
  }

  let gen = idMaker();
  console.log(gen.next().value) // 0
  console.log(gen.next().value) // 1
  console.log(gen.next().value) // 2
```
### Importowanie i eksportowanie
```javascript
// lib/math.js
export function sum(x, y) {
  return x + y
}
export var pi = 3.141593

// app.js
import * as math from "lib/math"
alert("2π = " + math.sum(math.pi, math.pi))

// otherApp.js
import {sum, pi} from "lib/math"
alert("2π = " + sum(pi, pi)
```

```javascript
// lib/mathplusplus.js
export * from "lib/math"
export var e = 2.71828182846
export default function(x) {
  return Math.log(x)
}

// app.js
import ln, {pi, e} from "lib/mathplusplus"
alert("2π = " + ln(e)*pi*2)
```

### Nowa pętelka for of
* iteruje po obiektach wspierających [protokół iterowania](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)
```javascript
let arr = [3, 5, 7]
arr.foo = "hello"

for (let i in arr) {
   console.log(i) // "0", "1", "2", "foo"
}

for (let i of arr) {
   console.log(i) //  "3", "5", "7"
}
```
### Nowe struktury danych Map, Set i inne
* Set - zbiór matematyczny
* Map - obiekt którego kluczami może być praktycznie wszystko
```javascritp
var s = new Set()
s.add("hello").add("goodbye").add("hello")
s.size === 2
s.has("hello") === true

// Maps
var m = new Map()
m.set("hello", 42)
m.set(s, 34)
m.get(s) == 34
```
### Nowe funkcje/ metody/ stałe
```javascript
Number.EPSILON
Number.isInteger(Infinity) // false
Number.isNaN("NaN") // false

Math.acosh(3) // 1.762747174039086
Math.hypot(3, 4) // 5
Math.imul(Math.pow(2, 32) - 1, Math.pow(2, 32) - 2) // 2

"abcde".includes("cd") // true
"abc".repeat(3) // "abcabcabc"

Array.from(document.querySelectorAll('*')) // Zamienia pseudo tablice na tablice
[0, 0, 0].fill(7, 1) // [0,7,7]
[1, 2, 3].find(x => x == 3) // 3
[1, 2, 3].findIndex(x => x == 2) // 1
[1, 2, 3, 4, 5].copyWithin(3, 0) // [1, 2, 3, 1, 2]
["a", "b", "c"].entries() // iterator [0, "a"], [1,"b"], [2,"c"]
["a", "b", "c"].keys() // iterator 0, 1, 2
["a", "b", "c"].values() // iterator "a", "b", "c"
```

### NATYWNE PROMISY
* alternatywa dla callbacków
* [chainowanie](https://mdn.mozillademos.org/files/8633/promises.png) asynchronicznych operacji!!
* Promise może być w jednym z trzech stanów
  * pending
  * fulfilled
  * rejected

* Metody:
  * Promise.all	-- nowy promise który przyjmuje promisy i zwraca nowego gdy wszystkie sie wykonaja
  * Promise.prototype.catch -- wywolywane po rejectie	
  * Promise.prototype.then	-- wywoływane po then
  * Promise.race	-- nowy promise z promisa który wykona się jako pierwszy
  * Promise.reject	-- wywołuje schainowanego catcha
  * Promise.resolve -- wyowołuje schainowanego thena

```javascript
let myFirstPromise = new Promise((resolve, reject) => {
    setTimeout(function(){
        resolve('po setTimeout')
    }, 1000)
})

myFirstPromise.then((successMessage) => {
    console.log(`Wykonałem się ${successMessage}`)
})
```
```javascript
\\...
new Nigthmare({show: true})
  .goto('https://ppuslugi.mf.gov.pl/_/')
  .wait('.SidebarLink.SidebarLinkChNIP')
  .click('.SidebarLink.SidebarLinkChNIP')
  .wait('#b-6')
  .click('#b-6')
  .wait('input[name="b-6"]')
  .click('#b-6')
  .type('#b-6', nip)
  .click('#b-7')
  .wait('#caption2_b-9')
  .wait('.EnabledLink.HideInvalidLink')
  .wait('#caption2_b-a')
  .wait( () => document.querySelector('#caption2_b-9').innerHTML != 'False' )
  .evaluate( () => document.querySelector('#caption2_b-9').innerHTML )
  .end()
  .then((result) => {
    this.message = result
    this.emit('nipIsReady')
  })
  .catch((error) => {
    console.error(error)
    this.message = error
    this.emit('scanningError')
  }) 
\\...
```
# Styl

* 'use strict'
* średniki są niepotrzebne
* camelCase chyba że:
```javascript
const Namespace = {}
const SOME_MAGIC_NUMBER = 1337
class Class {
  _privateMethod() {
    ...
  }

  publicMethod() {
    ...
  }
}
```
* [] , {} zamiast new Array, new Object
* nie uzywac var tylko let i const
* spacje obok operatorow np x += 1 
* używać klas
* słówko new raczej tylko przy klasach
* 2 spacje na wciecie
* 1 spacja przed otwierajaca klamerka


