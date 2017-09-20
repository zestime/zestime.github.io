
---
layout: post
title:  "Javascript Getting Start"
date:   2017-09-12 14:03:45 +0900
categories: javascript play-with-javascript 
---

## Getting Start

너무 지엽적인 부분에 집착하면, 숲을 보는 지혜를 잊기 쉽습니다. 가야할 길이 멀다면, 정상을 바라보는 것만으로도 강한 동기가 되어줄 수 있습니다. 디테일에 치중하기 보다는, 우리의 여정을 먼저 간략히 살펴보고 갈까 합니다.

## Contents

1. Cheatsheet
1. Variables & Scope
1. Scope
1. Object
1. This
1. Array
1. Generator
1. Proxy
1. Async

## Types

프로그래밍 언어에서 그 기저가 되는 것이 바로 타입입니다. 다음은 자바스크립트에서

```
// 'typeof' 연산자는 type을 확인할 때 사용합니다.
var undefine;
console.log(undefine); // "undefined", 아직 값을 지정하지 않은 상태
console.log(typeof null); // "object", null은 값이 없는 상태
console.log(typeof true); // "boolean"
console.log(typeof 100); // "number"
console.log(typeof "hello"); // "string"
console.log(typeof symbol.iterator); // "symbol"
```
그 외의 타입은 모두 object에서 파생된 타입입니다. 가장 많이 사용하는 타입은 object, array 그리 function입니다. Date, Map, Set 등 많지만, 이들은 앞으로 예제를 통해서 살펴보겠습니다.


## Variables

```
const pi = 3.14;      
pi = 3.1415;          // error - const는 값을 변경할 수 없습니다. 

let e = 2.71;
e = 2.718;            // 2.718, no error
let e = 2.71828;      // error - 선언은 한번만 

var variable = "old-style";       
var variable = "terrible override, nobody use" // no error
```
const, let 그리고 var를 이용해서 변수를 선언합니다. 조금씩 그 용법이 다르지만, var의 문제점을 대체하기 위해서, const와 let이 고안되었습니다. const와 let은 block-scope을 지원합니다.
```
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
기존에는 global, function scope만 제공되었으나, 새로이 block scope이 제공되고 있습니다. 모든 변수는 scope에 종속되어 있으며, scope이 없어지면 변수들도 같이 사라지게 됩니다.


## Object

```
const emptyObject = {};
const complexObject= {
  name: "",
  

};
```








variable은 변수라고 번역되나, 변수라는 단어는 자주 사용하는 말이 아니니 통 무슨 뜻인지 감이 오지 않습니다. 차라리, 이름을 정해주는 일이라고 보셔도 괜찮을 것 같습니다. '3.14를 pi라고 부르겠다'라고 선언하는 일입니다. 제가 선언이라는 표현을 사용했는데, 사실 우리가 쓰는 선언이라는 단어는 혁명이나 크나큰 행사에서 새로운 의미를 정하고 공표하는 일입니다. 여기서는, 작은 의미의 선언이라고 생각하시면 좋겠습니다. 컴퓨터에게 나는 'pi'를 쓴다고 알려주는 일을 선언이라고 표현합니다. 정확히는, declaration이라고 합니다. declaration의 예를 들어보면,

```
var pi = 3.14;
```

위에 구문을 선언문이라고 하는데, 원래는 declaration statement라고 합니다. declaration이 담고 있는 하나의 문장 정도로 생각하시면 됩니다. 문장을 구분하기 위해서, semicolon(;)을 이용하여 문장이 끝났음을 알려줍니다.

```
var a=3, b=5;
```


#### var

변수를 선언할 때에는 var를 사용합니다. var라는 키워드 뒤에 변수 이름과 값을 표기하면 되겠습니다. 예를 들면,

```
var hello = "world";
```

hello라는 변수를 만들고, 거기에 "world"라는 문자열을 넣었습니다. 그렇습니다. 2가지 일을 한 셈입니다. 나눠보면,

```
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
