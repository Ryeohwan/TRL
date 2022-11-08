## ES6가 무엇인가
---
- 자바 스크립트의 개정판이다. 
  
<br>  

## ES6에서의 자료형
---
|키워드|구분|선언위치|재선언|
|:----:|:----:|:----:|:----:|
|var|변수 |전역 스코프(함수레벨) |가능|
|let|변수|해당 스코프(블록레벨) |불가능|
|const|상수|해당 스코프(블록레벨) |불가능|

<br>  

## ES6에서의 객체
---
- 객체를 정의할 때 객체의 key값과 value 에 할당한 변수명이 같을 경우 value를 생략할 수 있다.

```
<script>
      const id = "ssafy",
        name = "안려환",
        age = 20;

      const user = {
        id,
        name,
        age,
      };
</script>
```
> 훨씬 간략해 졌다.

<br>

## ES6에서의 메서드
---
- 메서드도 위 객체와 같이 key 값과 value 에 할당한 변수명이 같을 경우 생략 가능하다. 구조는 동일하니 sample 코드는 생략.

## ES6 구조 분해 할당 (DestructingAssignment)
---
- 배열이나 객체에 입력된 값을 개별적인 변수에 할당하는 간편한 방식 제공

```
<script>
      const user = {
        id: "ssafy",
        name: "안려환",
        age: 20,
      };
			// 객체의 property와 변수명이 같을 경우.
      let { id, name, age } = user;
      console.log(id, name, age);

			// 변수명을 객체의 property명과 다르게 만들 경우.
			let { id: user_id, name: user_name, age: user_age } = user;
			console.log(user_id, user_name, user_age);
</script>
```

<br>

```
<script>
	function getUser() {
		return {  // 객체를 리턴하는 함수이다.
		id: "ssafy",
		name: "안려환",
		age: 20,
	};


let { id, name, age } = getUser();
console.log(id, name, age);


</script>
```

<br>

## Spread Syntax (…)
---
- 이건 반복 가능한(iterable) 객체에 적용할 수 있는 문법
- 배열이나 문자열 등을 풀어서 하나 하나로 전개 시킬 수 있다.

```
<script>
	const user1 = { id: "ssafy1" };
	const user2 = { id: "ssafy2" };
	const arr = [user1, user2];
	// array copy
	const copyArr = [...arr];
	console.log(arr, copyArr);
	console.log(arr === copyArr);

	// 주의 : spread operator의 경우 값 복사가
	// 아닌 주소를 가져오기 때문에 값을 바꿀 경우
	// 모두 변경.
	user1.id = "ssafy8";
	console.log(arr, copyArr);

	// 배열 복사 + 새로운 객체 추가.
	const addArr = [...arr, { id: "ssafy3" }];
	console.log(arr, addArr)
</script>
```
<br>

![1](https://user-images.githubusercontent.com/73810834/200440945-d8ab393e-94d1-490d-8a27-60ad2be7a839.png)

<br>

- 주소를 가져오기 때문에 값을 변경하면 호이스팅으로 인해 전부 변경이 된다.

<br>

## …는 배열이나 문자열 등을 풀어서 하나 하나로 전개 시킬 수 있다.
---
```
<script>
	// 객체 복사
	const copyUser = { ...user1 };

	// const copyUser = user1;
	console.log(copyUser);
	copyUser.id = "ssafy100";
	console.log(user1, copyUser);
	console.log(user1 === copyUser);

	// 객체 복사 + 새로운 property 추가.
	const copyUser2 = {
	...user1, name: "안려환"
	};
	console.log(copyUser2);

	// 주의 : spread operator의 경우 값 복사가 
  // object merge (병합)
	const u1 = { id1: "ssafy1" };
	const u2 = { id2: "ssafy2" };
	const u = { ...u1, ...u2 };
	console.log(u);

	// 주의점 : key가 같은 object를 병합할 경우
	마지막 obj의 값이 설정.
	const user = { ...user1, ...user2 };
	console.log(user);
</script>
```
<br>

![2](https://user-images.githubusercontent.com/73810834/200440991-ce1644fb-8415-49bf-bde2-df62ecff518c.png)

```
<script>
	// 함수 호출 시 인자로 전달.
	const num = [1, 3, 5, 7];
	function plus(x, y, z) {
	return x + y + z;
	}
	const result = plus(...num);
	console.log(result);
</script>
```

⇒ 콘솔에 찍히는 값은 1 + 3 + 5 해서 9가 나온다.

<br>

## Default Parameter
---
- 함수에 전달된 파라미터가 undefined이거나 전달되지 않았을 경우, 설정한 값으로 초기화 됨.

```
<script>
      function print2(msg = "안녕하세요 싸피") {
        console.log(msg);
      }
      print2("hello ssafy");
      print2();
</script>
```

<br>

```
<script>
      function getUserId(userId = "ssafy9") {
        return userId;
      }
      console.log(getUserId());
      console.log(getUserId(undefined));
      console.log(getUserId(null));
      console.log(getUserId("troment"));
      // default parameter각 reference type일 경우 호출될 때마다 새로운 객체를 생성함.
      function appendArr(val, array = []) {
        array.push(val);
        return array;
      }
      console.log(appendArr(10)); // [10]
      console.log(appendArr(20)); // [20] or [10, 20] ??
</script>
```

- 맨 마지막 값을 보면 객체를 새로 생성했기 때문에 array.push() 가 되지 않고 20만 들어있는 값이 리턴되었다.


## Template 란 무엇인가
---
- 문자열에 변수를 포함시킬 때 더 직관적이고 편하게 사용하기 위한 기능
- 변수를 넣고자 하는 부분에 ${ var } 키워드를 사용해 변수를 넣어 줌.
- 문자열 사용시 큰따옴표( “ ) 대신 백틱( ` )을 사용.
- 백틱 내부의 줄 바꿈은 그대로 적용.

```
<script>
      // Template String(`, 백틱 사용)
			const id = "ssafy",
			name = "안려환",
			age = 20;
			
			// ES6 이후
			console.log(`${name}(${id})님의
			나이는 ${age}입니다.`);
</script>
```
- 이렇게 “\n” 라는 줄바꿈 문자를 넣지 않고도 가능하다.

<br>

## Arrow Function(화살표 함수)
---
- 기존 함수의 function name(param){} 의 형식을 축약하여 사용.
- 함수의 이름이 없는 익명 함수이므로 변수에 담아서 사용
  - const name = (param) => {}; 
  - 매개변수에 따른 설정
  ```
	<script>
	// 매개변수가 없을 경우.
	() => {};
	// 매개변수가 한 개 있을 경우. (param)의 소괄호 생략 가능.
	(param) => {};
	// 매개변수가 여러 개 있을 경우. (param1, param2)의 소괄호 생략 불가능.
	(param1, param2) => {}
	</script>
	```
	- function body에 따른 설정
  
  ```
	<script>
	(x) => {
		return x + 10;
	};
	
	// body의 내용이 한 줄일 경우 {} 생략 가능하고 자동으로 결과값이 return된다. 
	// 위와 동일.
	(x) => x + 10;
	
	// object return시 () 사용하면 return 생략 가능.
	() => {
		return { id: "ssafy" };
	};
	() => ({ id: "ssafy" });
	
	// body가 여러 줄일 경우 {}, return 생략 불가.
	(x) => {
	const y = x + 100;
	return y;
	};
	</script>
	```

<br>

```	
	<script>
		const func6 = (x, y) => x + y;
		result = func6(50, 80);
		console.log(result)
		
		const func8 = () => ({ id: "ssafy" });
		result = func8();
		console.log(result.id)
		
		const func10 = (x) => {
		const y = x * 3;
		return y;
		};
		result = func10(10);
		console.log(result)
		
		// Arrow Function에서는
		// this가 바인딩 되지 않음.
		const user = {
		id: "ssafy8",
		age: 20,

		// info() {
		// console.log(this.id, this);
		// },

		// this 사용 불가.
		info: () => console.log(this.id, this),
		};
		user.info();
</script>
```

<br>

