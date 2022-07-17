# ES6+

- `let` и `const` => [[Область видимости]]
- Шаблонные строки 
    ```js
    const name = 'Andrey'
    console.log(`Hello, ${name}!`) // Hello, Andrey!
    ```
- Unicode поддержка (возможность вставлять символы)
- `String.prototype.replaceAll(value, newValue)` -  замена `value` на `newValue` во всей строке
- `Arrow function` 
    ```js
    const greeting = name => `Hello, ${name}!`
    
    greeting('Andrey') // Hello, Andrey!
    ```
- Параметры по умолчанию 
    ```js
    const greeing = (name: 'Andrey') => `Hello, ${name}!`
    
    greeting() // Hello, Andrey!
    
    greeting('Vasya') // Hello, Vasya!
    ```
- Операторы логического присваивания 
    ```js
    // присвоить если значение null | undeined
    let a = null;
    a ??= 'Привет' // Привет
    
    // присвоить если значение слева truthy
    let a = 'test'
    a &&= 'test2' // test2
    ```
- Разделители разрядов 
    ```js
    const number = 100_000_000 // 100000000
    ```
- `spread` и `rest`
    ```js
    const spread = (a,b,c) => console.log(a, b, c) // 1 2 3
    test(...[1,2,3]) 
    
    const rest = (...args) => console.log(args) // [1,2,3]
    rest(1,2,3)
    ```
- Дополнительные методы объекта `Object.is(value1, value2)`
    ```js
    Object.is(1,1) // true
    Object.is(1, '1') // false
    ```
- Деструктуризация 
    ```js 
    const arr = [1,2,3]
    const [a,b,c] = arr;
    console.log(a,b,c) // 1,2,3
    
    const obj = {
        a: 1,
        b: 2,
        c: 3,
    }
    const {a,b,c} = obj;
    console.log(a,b,c) // 1,2,3
    
    ```
- `for...in` и `for...of` 
    ```js
    const arr = [1,2,3]
    
    for (const value in arr) {
        console.log(value) // 1,2,3
    }
    
    const obj = {
        a: 1,
        b: 2,
        c: 3,
    }
    
    for (const value of obj) {
        console.log(value) // a,b,c
        console.log(obj[value]) // 1,2,3
    }
    ```
- ООП
- Новый тип данных Symbol
    Основные преимущества
    - всегда уникальны
    - не преобразуются в строки (нужно явно вызывать `toString()`)
    - игнорируются циклом `for...in`, но  `Object.assign` увидит такие свойства
- Promise
- Итераторы и генераторы
    Итераторы позволяют использовать любой объект в цикле `for...of` или перебирать элементв через специальные методы, например `next`

    - `value` - значение массива
    - `done` - `boolean` значение, которое показывает последний ли это элемент в массиве.

    ```js
    const arr = [1,2,3]
    const itr = arr[Symbol.iterator]();
    
    itr.next(); // {value: 1, done: false}
    itr.next(); // {value: 2, done: false}
    itr.next(); // {value: 3, done: false}
    
    itr.next(); // {value: undefined, done: true}
    ```
    Пример с получение массива в с диапазоном чисел:
    ```js
    const range = {
      from: 1,
      to: 5,
    }
    
    range[Symbol.iterator] = function() {
        return {
            current: this.from,
            last: this.to,
            
            next() {
                if (this.current <= this.last) {
                    return {done: false, value: this.current++}
                } else {
                    return {done: true}
                }
            }
        }
    }
    
    for (const num of range) {
        console.log(num) // 1,2,3,4,5
    }

    ```

    Генератор - специальный тип функции, которая использует паттерн "Фабрика" для генерации итераторов.

    Если разработчику нужно использовать ряд фибоначи от `100000`, будет съедать много ресурсов.
    Генераторы помогут получить итератор, который будет пересчитывать каждое следующее число в момент запроса следующего элемента, а не сразу весь ряд для числа `100000`

    ```js
    function* fibonacci(n) {
        const infinity = !n && n!== 0;
        
        let current = 0;
        let next = 1;
        
        while (infinity || n--) {
            yield current;
            [current, next] = [next, current + next]
        }
    }
    
    const [...first10] = fibonacci(10);
    
    console.log(first10) // [ 0, 1, 1, 2, 3, 5, 8, 13, 21, 34 ]
    ```
- `async` и `await`
    Это ключевые слова делают асинхронный код синхронным и помогают улучшить читаемость.
- Коллекции `Map` и `Set`
- Оператор возведения в степень (появился в ES7)
    ```js
    console.log(2**2) // 4
    console.log(2**3) // 8
    ```
- Полифиллы
    Полифилл - библиотека, которая добавляет в старые браузеры функциональность современных браузеров
- Транспайлинг
    Транспайлинг - компиляция кода, написанного на одном языке программирования, в другой. Иначе говоря, это изменение вида кодировки
    ```js
    //до
    const f = num => `Число ${num}`
    
    // после
    var f = function (num) {
        return 'Число' + num
    }
    ```
    Самый мощный компилятор JS - Babel
    Упрощенно транспайлинг работает так:
    1. Алгоритм парсит код и стоит синтаксическое дерево
    2. В процессе трансформации современный синтаксис заменятся на эквивалентный, приемлемый для выбранного бразуера
    3. Преобразовывает полученное дерево в новый транспилированный код