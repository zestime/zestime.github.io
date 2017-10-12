---
layout: post
title:  "Javascript Getting Start"
date:   2017-09-12 14:03:45 +0900
categories: javascript play-with-javascript 
---



## Getting Start

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

Javascript에서는 모든 type이 object라고 해도 틀린 말이 아닙니다. 기본적인 몇 가지 type외에는 모두 object의 상속을 받습니다. 가장 기본이 되는 type이 되겠습니다.

```js
const emptyObject = {};
const complexObject= {
  name: "",
  

};
```


## This

`this`는 OOP에서는 instance 자신을 뜻하는 용어로 사용되나, javascript는 좀 애매하게 작동합니다. 간단히 말하면, `.`연산자의 앞의 object를 의미합니다.

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

math.dir();		// {pi: 3.14, dir: function(){...}}
```

참 이상한 결과가 아닐 수 없습니다. 변수에 참조를 걸었더니, 
잘 작동하는 함수에 대해서, dir로 참조하도록 하고 실행헀더니 전혀 다른 결과가 나옵니다. 그 말인듯, this가 이전에는 math를, 다음 번에는 window객체를 참조하고 있다는 뜻 입니다.

사실, 이건 이해하기 어렵게 만들면 프로그램을 복잡하게 만드는 요소가 있습니다. Javascript는 Java/C#과 같은 강력한 OOP를 제공하는 언어가 아닙니다. 



`{}`을 이용해서, 빈 object를 만들었습니다. 주로, 여러 변수를 묶는 용도로 사용되기도 하고, 함수등을 value로 할당하여 namespace와 같이 사용하기도 합니다.

```js
const option = {
  lang: 'kr',
  timezone: 'seoul'
};

const Logger = {
  error: msg => console.log('ERROR: ' + msg)
  debug: msg => console.log('DEBUG: ' + msg)
};

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
