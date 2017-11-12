---
layout: post
title:  "Javascript - 1.Getting Start"
date:   2017-09-12 14:03:45 +0900
categories: javascript play-with-javascript 
---

> ### 시작에 앞 서...
> Javascript로 그 동안 밥을 빌어 먹었으니, 책을 써 볼까 합니다. 책 이름은 '가장 얇은 Javascript'입니다. Douglas의 *Javascript: The Good Part*에 영향을 받은 것 맞습니다. 그 외에도 좋은 책들이 많았으나, 저는 겉멋 든 개발자들에게 어울릴 가장 얇은 책을 적어볼까 합니다.
> - [1. Getting Start]({{ site.baseurl }}{% link _posts/2017-09-12-getting-start.md %})
{:.intro}


너무 지엽적인 부분에 집착하면, 숲을 보는 지혜를 잊기 쉽습니다. 가야할 길이 멀다면, 정상을 바라보는 것만으로도 강한 동기가 되어줄 수 있습니다. 디테일에 치중하기 보다는, 우리의 여정을 먼저 간략히 살펴보고 갈까 합니다.

## Contents

1. Basic data types
1. Variable & Scope
1. Function 
1. Object
1. This
1. Array
1. Generator
1. Proxy
1. Async

## Basic data types

다른 언어와 마찬가지로 기본적으로 제공되는 타입이 제법 있습니다. number, string, boolean, null, undefined와 같은 기초적인 타입과 object, function 등이 제공됩니다.

**number** : 정수형와 부동소수형의 구분이 없습니다. number라는 하나의 타입만 제공됩니다.

```js
var x = 10;
console.log(x + 2);    // 12
console.log(x * 5);    // 50
console.log(x % 3);    // 1
console.log(x ** 3);   // 1000, 지수연산(exponentiation)은 chrome, firefox외에는 확인 필요
```
**boolean** : true와 false값이 제공됩니다. 

```js
var b = true;
console.log(b);     
console.log(!b);
console.log()
```

**string** : 문자열을 만들 때는 "(double quotation, 쌍따옴표)나 '(single quotation, 혿따옴표)로 묶어주면 됩니다. es6에서는 \`(back-quote)를 이용해 여러 줄에 걸쳐 문자열을 만들 수 있습니다.

```js
var str = "Hello";
console.log(str + " world!");     // "Hello world!", +는 concatenation
console.log(str.concat(" world!")); // "Hello world!", 위와 동일
console.log(str.length);        // 5, 문자열 길이
console.log(`it supports
multilines`);                   // backquote를 이용해서, 여러 줄에 걸쳐 작성할 수도 있습니다.
```

**undefined와 null** : undefined는 아직 값이 설정되지 않은 상태를 뜻합니다. null은 값이 없는 상태입니다. 

```js
var x;
console.log(x);       // undefined
var y = null;
console.log(y);       // null
```

## Variables

const, let 그리고 var를 이용해서 variable(변수)를 선언합니다. 

```js
const pi = 3.14;      
pi = 3.1415;          // error - const는 값을 변경할 수 없습니다. 

let e = 2.71;
e = 2.718;            // 2.718, no error
let e = 2.71828;      // error - 선언은 한번만 

var variable = "old-style";       
var variable = "terrible override, nobody use" // no error
```

const와 let은 es6에 추가되었습니다. const와 let의 가장 큰 특징은 block-scope을 지원한다는 점입니다.

```js
var nonBlock = "original";
{
  var nonBlock = "updated";
  console.log(nonBlock);              // updated
}
console.log(nonBlock);                // updated

let block = "outer";
{
  let block = "inner";
  console.log(block);                 // inner
}
console.log(block);                   // outer
``` 

> ### scope 
> scope(스코프)는 변수가 유효한 범위를 뜻합니다. function scope라고 하면, 함수 내에서 사용하는 변수들이 유효한 범위입니다. function이 수행 중에만 내부 변수들을 사용할 수 있으므로, function scope은 함수가 실행되면 시작하고 종료시에는 소멸된다고 할 수 있습니다. 내부 변수들 역시 scope과 같이 생성과 소멸되므로, scope는 변수의 life cycle과 관련있습니다. 
> ### block scope 
> function scope가 function과 관련이 있는 것처럼, block scope(블럭 스코프)는 block과 관련이 있습니다. block은 `{`와 `}`으로 이루어진 코드 묶음입니다. while문이나 if문에서 내부적으로만 사용하는 변수를 만들 수 있게된 것이죠.
{:.explain}


## Function

Function(함수)는 프로그램의 근간을 이루는 타입입니다. function은 first-class object(1급객체)로 다양하게 사용할 수 있습니다. 또 closure를 통해 function의 상태를 가질 수 있습니다.

> ### first-class object 
> first-class object라는 표현은, 숫자나 문자열과 마찬가지로 사용될 수 있다는 뜻 입니다. 변수에 대입하거나 매개 변수의 인자값으로 함수를 전달하는 것들이 가능합니다. 또한, 함수를 반환값으로 사용할 수도 있습니다. 별로 큰 일이 아닌것 같지만, 이를 통해서 더욱 자유롭게 함수를 사용할 수 있게 됩니다.
{:.explain}

```js
function consoleLogger(){
  var no = 0;
  return function(msg){
    console.log(`${no++} - ${msg}`);
  };
}

var logger = consoleLogger();
logger("first message");            // "1 - first message"
logger("it has a number state");    // "2 - it has a number state"

```

## Object

Javascript에서는 모든 type이 object라고 해도 틀린 말이 아닙니다. 기본적인 몇 가지 type외에는 모두 object의 상속을 받습니다. 가장 기본이 되는 type이 되겠습니다. Object를 생성하는 방법은 무척 간단합니다. `{}`는 Object를 의미합니다.

```js
const emptyObject = {};
```

Object는 key/value를 쌍으로 가지는 type입니다. 간단한 dictionary라고 볼 수 있는데, key를 통해서 value를 선언하거나 참조하게 됩니다. key와 value는 `:`(colon)을 통해 구분되며, value는 문자, 숫자 그리고 function이 될 수 있으며, 다른 Object를 가질 수도 있습니다. 이런 특징 때문에 Object는 data를 담는 용도로도 많이 사용합니다. 대표적인 예가, JSON이 되겠습니다. 

```js
const option = {
  lang: "euc-kr",                 // key: "lang", value: "euc-kr" 
  timezone: "seoul"
};

console.log(option.lang);         // "euc-kr"
console.log(option["lang"]);      // "euc-kr"

option.summerTime = true;         // 새로운 key/value를 추가합니다.

console.log(option.summerTime);   // true
```

또한 Object는 클래스와 비슷하게 사용되기도 합니다. property는 물론, function도 가질 수 있기 때문입니다. 

```js
const country = {
  supportedTimezone : ["seoul", "tokyo"],
  set Timezone(val) {
    if (!this.supportedTimezone.includes(val))
      throw new Error(`'${val}' is not supported`);
      
    this.timezone = val;
  },
  get Timezone() {
    if (!this.timezone)
      throw new Error("'Timezone' is not initialized");
      
    return this.timezone;
  },
  Load : function(option) {
    Object.assign(this, option);
  },
  Clone() {
    return Object.create(this);
  }
}

country.Timezone = "bangkok";     // Error
country.Load({lang:"UTF-8"});     // country { lang:"UTF-8" }

const another = country.Clone();
another.Timezone = "tokyo"        // another { timezone: "tokyo" }
console.log(another.lang);        // "UTF-8"
```

예제 코드가 조금 복잡하지만, 하나씩 살펴보겠습니다. `set`이라는 키워드를 이용해서, `Timezone`이라는 property를 만들었습니다. 지원하는지 않는 timezone에 대해서, template string을 이용해서 error mesage를 만들어 Exception을 발생시킵니다. `get`또한 마찬가지여서, 값이 없는 경우에 Javascript는 `undefiend`를 반환하지만, 이번에는 에러처리하도록 작성했습니다. `Load`와 `Clone`은 둘 다 함수로서, `Load`의 경우에는,  `Clone`의 경우에는 익명함수 대신에 간단한 표기(shorthand)로 선언하였습니다. 

`Object.assign()`과 `Object.create()`는 Object를 만들 때 사용합니다. 특징은 조금 다른데요, `assign`의 경우는 mix-in이라는 표현을 사용하고, `create`의 경우에는 prototype inheritance라고 합니다.

- **mix-in** : 합집합을 구하는 것이라고 할 수 있습니다. A와 B가 서로 다른 Object이지만, mix-in을 하게 되면, A의 모든 key와 B의 모든 key를 합친 하나의 Object를 만들게 됩니다. 겹치는 key에 대해선, 덮어쓰게 되므로 object의 순서에 주의를 해야 합니다.

- **prototype** : prototype은 Javascript에서 inheritance(상속)을 표현하는 방법입니다. prototype이라는 용어가 낯설 수도 있지만, 부모 타입 정도로 생각하시면 되겠습니다. 

## This

`this`는 OOP에서는 instance 자신을 뜻하는 용어로 사용되나, javascript는 좀 애매하게 작동합니다. 간단히 말하면, `.`연산자의 앞의 object를 의미합니다. 이게 도대체 무슨 말인고 하니,

```js
const math = {
  pi: 3.14,
  dir: function() {
    console.log(this);
  }
};

math.dir();		// {pi: 3.14, dir: function(){...}}

const dir = math.dir;
dir();			// Window{....}
```

참 이상한 결과가 아닐 수 없습니다. 같은 함수인데, 어떻게 저런 결과가 나올 수가 있을까요. 혹시, 함수 내부에서 `this`를 사용해서 그런걸까요? 맞습니다. `this`는 함수가 수행될 때 결정됩니다. 그리고 그 결정은 호출할 때 이뤄지게 됩니다. 처음에 함수를 호출한 경우에는, `math`에 있는 `dir()`를 호출한 셈입니다. 두 번째 호출은 함수만 호출했기 때문에, `this`는 사실 없어야 합니다. 하지만, 결과를 보면 전역 객체(global object)인 `Window`가 나오는 것을 볼 수 있습니다. 

> ### global scope vs local scope 
> global(전역)과 local(지역)으로 나뉘는데, local scope은 일반적인 함수를 생각하시면 되고, global scope은 웹브라우저에는 window객체가 됩니다. local scope은 생성과 소멸을 반복하지만 global scope은 프로그램 실행시에 1번 생성되어 종료시까지 유지되게 됩니다. 즉, global scope은 잘 관리되지 않으면 global namespace pollution(오염)에 노출될 수 있습니다. 
{:.explain}

## Generator

Generator를 한번에 수행하지 않고 나눠서 수행하는 function입니다. 이게 도대체 무슨 말인고 하니, 일반적으로 function은 한번 수행하기 시작하면 return을 만나거나 body(함수 몸체)를 모두 수행하게 됩니다. 이에 반해, generator는 여러 번에 걸쳐 수행될 수 있는 특수한 function입니다. `function *`으로 선언하고 `yield`를 통해 return value를 반환하고 실행 범위까지 표시하게 됩니다.

```js
function* adder() {
  let called = 0;
  yield called;             // (1)
  called += 1;
  yield;                    // (2)
  called += 3;
  yield* sub_generator();
  called += yield called;   // (5)
  yield called;             // (6)
}

function* sub_generator() {
  yield "from sub";         // (3)
  yield "still in sub";     // (4)
}

```

예제가 조금 길지만, 이게 generator의 전부라고 할 수 있습니다. `generator()`를 살펴보면, `function*`으로 **generator**라고 선언했습니다. 내부를 보면, `called`라는 local variable을 선언하고 0이라는 값을 주었습니다. 그리고 바로 `yield`구문을 이용해서 실행을 멈추고 `called`를 반환하였습니다. 다시 말하면, function을 수행하다가 `yield`를 만나면 실행을 멈추고, 이후에 있는 식을 반환하게 됩니다. 다음 실행은 마찬가지로 다음 `yield`인 *(2)*까지 수행됩니다. 이번에는 반환값을 따로 표시하지 않았습니다. 그리고 수행되다가 `yield*`이라는 구문을 만나게 됩니다. `yield*`이라는 표현에 당황하실 수도 있는데, 여러 개의 `yield`를 처리해야 한다는 뜻으로 생각하시면 됩니다. generator 내부에 또 다른 generator를 호출하는 경우에 사용합니다. 예제 코드에서도 `sub_generator()`라는 또 다른 generator를 호출하고 있습니다. 다른 generator이지만, 동작은 동일하게 이뤄집니다. `yield`를 만날 때 까지 수행하고 반환값을 넘겨주게 됩니다. 지금까지 `yield`는 줄의 처음에 단독으로 사용되었습니다. *(5)*는 조금 특이하게 식의 일부로 사용되고 있습니다. generator 내부로 매개 변수를 넘겨주고 싶을 때 이렇게 식의 일부로 `yield`를 사용하시면 되겠습니다. 기억하셔야 할 점은 `yield called`가 수행되고 `called += {매개 변수}`가 수행된다는 점입니다. 마지막 *(6)*을 통해서 `called`를 반환하고, generator는 수행을 마치게 됩니다.

이번에는 호출하는 코드를 살펴 보겠습니다.
```js
var s = adder();            // adder-generator 생성

console.log(s.next());      // { value: 0, done: false }
console.log(s.next());      // { value: undefined, done: false }
console.log(s.next());      // { value: 'from sub', done: false }
console.log(s.next());      // { value: 'still in sub', done: false }
console.log(s.next());      // { value: 4, done: false }
console.log(s.next(12));    // { value: 16, done: false }
console.log(s.next());      // { value: undefined, done: true }
```
일반 function과는 다르게 generator 생성해야 합니다. 단순히 호출하면 되는데, 실제 실행은 `next()`를 통해서 하게 됩니다. 한 번 `next()`를 호출할 때 마다, yield까지 수행되게 됩니다. `next()`의 반환값은 보시다시피, Object입니다. 특이한 점은 `done`이라는 flag고 있다는 점입니다. 이 flag는 generator가 모두 수행되었는지 알려주게 됩니다. *(2)*와 같이 반환 값이 없는 경우에, 수행이 완료되어서 반환 값이 없는 건지 아니면 코드 상에 반환값을 주지 않아서 없는 건지 모호한 경우가 생깁니다. 이런 경우를 분명하게 하기 위해서, `done`이라는 flag가 있습니다. 그리고 *(5)*의 경우는 `next(parameter)`를 주어, generator에 전달하고 있음을 알 수 있습니다. 그리고 마지막까지 수행을 하게 되면, `done`이 true로 나옴을 알 수 있습니다. function은 호출할 때 마다 새롭게 수행되지만, generator는 한번 수행을 마치면 새로 생성해야만 합니다.

## Symbol

es6에 새롭게 추가된 타입으로 symbol이 있습니다. symbol은 어떤 상태나 property를 가리키는 경우 유용합니다. 예를 들어,  

## Proxy

## Async


`{}`을 이용해서, 빈 object를 만들었습니다. 주로, 여러 변수를 묶는 용도로 사용되기도 하고, 함수등을 value로 할당하여 namespace와 같이 사용하기도 합니다.

```js
const option = {
  lang: 'kr',
  timezone: 'seoul'
};

const Logger = {
  error: msg => console.log('ERROR: ' + msg)
  debug: msg => console.log('DEBUG: ' + msg)};

logger.error("Lost");

```






variable은 변수라고 번역되나, 변수라는 단어는 자주 사용하는 말이 아니니 통 무슨 뜻인지 감이 오지 않습니다. 차라리, 이름을 정해주는 일이라고 보셔도 괜찮을 것 같습니다. '3.14를 pi라고 부르겠다'라고 선언하는 일입니다. 제가 선언이라는 표현을 사용했는데, 사실 우리가 쓰는 선언이라는 단어는 혁명이나 크나큰 행사에서 새로운 의미를 정하고 공표하는 일입니다. 여기서는, 작은 의미의 선언이라고 생각하시면 좋겠습니다. 컴퓨터에게 나는 'pi'를 쓴다고 알려주는 일을 선언이라고 표현합니다. 정확히는, declaration이라고 합니다. declaration의 예를 들어보면,

```js
var pi = 3.14;
```

위에 구문을 선언문이라고 하는데, 원래는 declaration statement라고 합니다. declaration이 담고 있는 하나의 문장 정도로 생각하시면 됩니다. 문장을 구분하기 위해서, semicolon(;)을 이용하여 문장이 끝났음을 알려줍니다.

```js
var a=3, b=5;
```


#### var

변수를 선언할 때에는 var를 사용합니다. var라는 키워드 뒤에 변수 이름과 값을 표기하면 되겠습니다. 예를 들면,

```js
var hello = "world";
```

hello라는 변수를 만들고, 거기에 "world"라는 문자열을 넣었습니다. 그렇습니다. 2가지 일을 한 셈입니다. 나눠보면,

```js
var hello;
hello = "world";
```

동일한 구문입니다. 단지 여기서는, declaration(선언)과 assignment(대입)을 구분하여 작성하였습니다. 즉, 첫번째 문장은 합쳐서 작성한 형태이고 두 번째 구문은 나눠서 작성했을 뿐, 동일하게 작동합니다. 일반적으로 declaration 한 번만 일어나며, assignment는 여러 번 수행될 수 있습니다.

```
var hello; // hello -> undefined
hello = "world"; // hello -> "world"
hello = "universe"; // hello -> "universe"
```
assignment가 일어나기 전에는, 당연히 undefined가 됩니다. 그리고 그 이후에는 그 값이 계속 바뀌게 됩니다.

> 선언하기도 전에 변수에 접근할 수 있나요?

파일이 커지다 보면, 흔히 하는 실수 중에 하나였습니다. 선언도 하기 전에, 변수를 참조하는 경우가 종종 생깁니다. 예를 들면,
`console.log(hello);var hello = "world";`와 같은 구문이 있습니다. hello가 선언되기도 전에, log함수에서 hello를 참조합니다. 보통의 프로그래밍 언어라면, 당연히 오류가 보여줄테지만, 'undefined'라고 로그가 찍히게 됩니다. 변수의 선언만을 먼저 처리하기 때문에 이런 일이 발생됩니다. 이해하기 쉽게 구문을 바꿔보자면, `var hello; console.log(hello); hello = "world";`와 같이 수행됩니다.

> 여러 번 선언이 가능할까요?

같은 변수를 여러 번 선언하면 당연히 안되겠죠. 그럼에도,

### Scope

앞서, variable을 만드는 법을 배웠습니다. variable을 마구 만들 수 있게 됨 셈인데, 그렇다면 우리는 몇 개까지 만들 수 있을까요? 아님, 어떻게 하면 만들어진 variable을 없앨 수 있을까요?

예전에는 변수를 선언하는데, var만 사용했습니다. 충분했죠. javascript 프로그램이 커지면서, 여러 요구 사항이 나타나기 시작했습니다. 프로그래밍 언어는 우리의 언어(자연어)와 달라서 회의를 통해 언어에 새로운 기능들을 추가합니다. 그렇게 추가된 기능이 block scope입니다. block scope을 이해하기 위해서는, scope을 먼저 이해해야 합니다.

사실, variable은 scope에 종속적입니다. 새로운 scope이 만들어지면, variable 또한 같이 생성됩니다. scope이 사라지면, 속하는 variable 또한 같이 사라지게 됩니다.

#### Function Scope

#### Block Scope


#### let


#### const

이전에 프로그래밍을 경험하셨다면, const가 낯설지 않을 것 입니다. 자바나 C++에도 있는 개념이며 Constant(상수)라고 불려집니다. const의 특징은 assign 이후에는 그 값을 변경할 수 없다는 점입니다. 사실, var와 let의 경우에는 그 값을 계속 변경할 수 있습니다. 무슨 애기냐 하면, 처음에는 'pi'를 3.14로 선언하였다가, 3.1415로 다시 바꿀 수 있다는 뜻입니다. 예를 들어

```
let pi = 3.14;

// let pi = 3.1415; 당연히 안되겠죠. let은 한번만 선언될 수 있어요

pi = 3.1415; // 이제부터, pi는 3.1415의 값을 가집니다
```

이렇게 값을 변경 할 수 있다는 뜻입니다. 아마 '그게 뭐가 문제지?' 혹은 '그게 뭐 어때서?'와 같은 생각을 하실지도 모르겠습니다. 간단한 프로그램에서는 특별히 문제가 되지 않지만, 프로그램이 커질 수록 variable을 관리하기가 어렵습니다. 각각의 변수가 어떤 상태인지 추적하여 프로그램을 작성하기 보다는, 상태가 변경될 때 새로운 변수에 대입하는 것이 훨씬 쉽고 간결한 프로그램을 작성하기 좋습니다. 예를 들어,

```
const pi_about = 3.14;
const pi = 3.1415;
```
pi가 프로그램 수행 중에 변한다고 할 때, 각각을 구분지어서 사용하는 것이 조금 더 방어적인 프로그래밍을 할 수 있습니다.
