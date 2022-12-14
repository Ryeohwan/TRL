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

## 동작 과정 보기

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

- article 이라는객체를 하나 선언해두고
- writes 라는 배열을 만들어 안에 게시글을 담아 두었다.

```
methods: {
    doWrite(article) {
      this.writes.push(article);
      alert("글 작성이 완료되었읍니다...");
    },
  },
```

- doWrite 라는 메소드로 아까 writes 배열에 article 객체를 push 해 준다.
- alert 를 띄운 이유는 현재의 내 게시판이 아무런 보안 조치가 없기 때문에 게시판에 엄청나게 많은 양의 글이 적히기 어렵게 하기 위함이다.

# BoardList.vue

```
<template>
  <div class="BoardList">
    <table border="1">
      <tr>
        <th>no</th>
        <th>title</th>
        <th>writer</th>
        <th>content</th>
      </tr>
      <tr v-for="(item, index) in writes" :key="index">
        <td>{{ item.no }}</td>
        <td>{{ item.title }}</td>
        <td>{{ item.author }}</td>
        <td>{{ item.content }}</td>
      </tr>
    </table>
    <button @click="checkWrite" v-show="!isWrite">글쓰러 가기</button><br />
    <div v-if="isWrite">
      제목 <input type="text" v-model="article.title" /> <br />
      글쓴이<input type="text" v-model="article.author" /> <br />
      내용
      <textarea
        cols="22"
        rows="4"
        placeholder="크기를 조정하실 수 있습니다."
        v-model="article.content"
      /><br />
      <button @click="doWrite">글쓰기</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "BoardList",
  props: [
    // 부모로 부터 받는 것이 여러 개 있다.
    "writes",
  ],
  data() {
    return {
      isWrite: false,
      article: { no: "", title: "", author: "", content: "" },
    };
  },
  methods: {
    checkWrite() {
      this.isWrite = true;
    },
    doWrite() {
      this.article.no = this.writes.length + 1;
      let art = this.article;
      this.article = {};
      this.$emit("doWrite", art);
      this.isWrite = false;
    },
  },
};
</script>

<style scoped>
.donestyle {
  text-decoration: line-through;
  color: darkgray;
}
table {
  width: 100%;
}
</style>

```

## 동작 과정 보기

- template 부분

```
<template>
  <div class="BoardList">
    <table border="1">
      <tr>
        <th>no</th>
        <th>title</th>
        <th>writer</th>
        <th>content</th>
      </tr>
      <tr v-for="(item, index) in writes" :key="index">
        <td>{{ item.no }}</td>
        <td>{{ item.title }}</td>
        <td>{{ item.author }}</td>
        <td>{{ item.content }}</td>
      </tr>
    </table>
    <button @click="checkWrite" v-show="!isWrite">글쓰러 가기</button><br />
    <div v-if="isWrite">
      제목 <input type="text" v-model="article.title" /> <br />
      글쓴이<input type="text" v-model="article.author" /> <br />
      내용
      <textarea
        cols="22"
        rows="4"
        placeholder="크기를 조정하실 수 있습니다."
        v-model="article.content"
      /><br />
      <button @click="doWrite">글쓰기</button>
    </div>
  </div>
</template>
```

- v-for 를 이용하여 부모로 부터 받아온 writes 배열을 테이블에 뿌려준다.

- button

```
 <button @click="checkWrite" v-show="!isWrite">글쓰러 가기</button><br />
```

- 버튼을 클릭하면 checkWrite 라는 메소드를 호출하고 v-show 를 사용하여 isWrite 가 false 일 때만 사용자에게 보여진다.

- 글쓰기 div 부분

```
  <div v-if="isWrite">
      제목 <input type="text" v-model="article.title" /> <br />
      글쓴이<input type="text" v-model="article.author" /> <br />
      내용
      <textarea
        cols="22"
        rows="4"
        placeholder="크기를 조정하실 수 있습니다."
        v-model="article.content"
      /><br />
      <button @click="doWrite">글쓰기</button>
    </div>

```

- isWrite 가 true 이면 이 div 가 나타난다.
- 이 입력부분을 article 이라는 객체의 각 값들과 바인딩해줘서 값을 읽어오게 하였다.
- 맨 아래 버튼을 클릭하면 doWrite 메소드를 호출한다.

- method 부분

```
methods: {
    checkWrite() {
      this.isWrite = true;
    },
    doWrite() {
      this.article.no = this.writes.length + 1;
      let art = this.article;
      this.article = {};
      this.$emit("doWrite", art);
      this.isWrite = false;
    },
  },
```

- checkWrite 는 글쓰기 기능의 보이기 기능을 구현한다. 글쓰러 가기를 누르면 true 로 바꿔 아래의 글쓰기 관련 div 의 내용이 보이게 한다.
- doWrite 는 article 객체에 사용자로 부터 입력받은 내용을 담아준다.
- $emit() 을 사용하여 부모에게 doWrite 와 art라는 객체를 전달한다.

# 실행 결과

![result](https://user-images.githubusercontent.com/73810834/200928951-638f2f4b-db08-4810-95ac-a7da9aa38d84.gif)
