# Vue cli 프로젝트(2) - 남은 할일 체크와 게시판 기능

# 간단한 게시판을 만들어 보자.

## 어떻게 해야 할까

- 먼저 자식에서 부모의 객체를 변경할 수 없다... 에러가 발생한다.
- 그러니 자식 컴포넌트에서는 함수 호출만 해주고 입력받은 값을 객체에 담아서 부모 vue 한테 보내서 거기서 작업을 하게 하자.

<br>

# 결과 화면

![1](https://user-images.githubusercontent.com/73810834/200838402-981096ff-f889-4788-84c9-9ff9d67d77df.PNG)

# app.vue

- 길이가 너무 길어지니 관련된 코드만 작성하겠다.

```
<template>
  <div id="app">
    <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>|
      <router-link to="/board">게시판</router-link>|
      <router-link to="/todo">TODO</router-link>|
      <router-link to="/ssabab">ssabab</router-link>
    </nav>
    <router-view/>
  </div>
</template>

```

- 여기서 봐야할건

```
<router-link to="/board">게시판</router-link>|
      <router-link to="/todo">TODO</router-link>
```

- board 라는 이름으로 라우팅을 하였다.

# BoardView.vue

```
<template>
  <div class="board">
    <h1>게시판</h1>
    <div>
      <board-list :writes="writes" @doWrite="doWrite" @article="article"></board-list>
    </div>
  </div>
</template>
<script>
import BoardList from "../components/board/BoardList.vue";

export default {
  name: "BoardView",
  components: {
    BoardList,
  },
  data() {
    return {
      article: "",
      writes: [
        { no: 1, title: "안녕", author: "안싸피", content: "안녕하세요" },
        { no: 2, title: "잘가", author: "장싸피", content: "안녕히가세요" },
      ],
    };
  },
  methods: {
    doWrite(article) {
      this.writes.push(article);
      alert("글 작성이 완료되었읍니다...");
    },
  },
};
</script>

<style scoped>
.board {
  background-color: aqua;
  text-decoration-color: brown;
}
</style>
```

<br>

## 동작 원리 보기

<br>

- 템플릿에 들어있는 내용을 보자.

```
<board-list :writes="writes" @doWrite="doWrite" @article="article"></board-list>
```

- writes 라는 배열 객체를 자식 뷰한테 보내준다.
- doWrite 라는 메소드 요청을 받아온다.
- article 이라는 객체를 받아온다.

<br>

- 이 녀석이 자식 뷰이다.

```
import BoardList from "../components/board/BoardList.vue";
```

- data 부분

```
 data() {
    return {
      article: "",
      writes: [
        { no: 1, title: "안녕", author: "안싸피", content: "안녕하세요" },
        { no: 2, title: "잘가", author: "장싸피", content: "안녕히가세요" },
      ],
    };
  }
```

- article 이라는
