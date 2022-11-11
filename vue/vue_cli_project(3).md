# Vue cli 프로젝트(3) - 게시판 기능 라우팅 처리로 바꿔보기

# 어떻게 바꿀 수 있을까?

- boardview 에 보일 것은 router-view 이고 이 태그가 내가 갖고있는 라우터 들의 구조에 따라 알맞은 라우터를 화면에 띄워준다.

# 어려웠던 점

- 글을 쓰는 기능까지는 금방 구현을 하였으나 라우팅 처리를 하니 어려운 점은 이제 push 를 해서 호출할 때마다 vue 객체가 새로 생성되어버린다는 점이다.
- 그렇게 되면 이제 게시판 안에 있던 내용물들이 다 초기화가 되어 버려서 새로 추가한 기존의 글들은 날라가게 된다.

# 생각했던 해결 방향

1. 시스템의 성능에는 치명적이지만 서버 사용 없이 뷰에서만 하는 것이니 기존 게시글을 담았던 배열 전체를 라우팅할 때 주고 받으면서 그 안에 값을 추가하고 빼는 방법으로 구현을 시도하였다.

   - 이러면 모든 게시글을 가지고 계속 이동하니까 성능에는 매우 치명적이고 보안상으로도 그 많은 값이 자꾸 이동하니 안좋을 것으로 생각된다.

   → 배열로 넘겨주었는데 이상하게 받은 쪽 vue 에서 그냥 object로 받아들이는 상황 발생 ! 그래서 이 Object 를 배열로 변환하고 싶었으나 방법이 도저히 떠오르질 않아서 실패하였다….(이 때의 코드를 기록을 해두지 않아 소실되었습니다..ㅠ)

2. 이 방법은 아예 생각도 못하고 있었는데 다른 친구들꺼를 참고하니 vue 객체 생성 전에 const 형 배열을 선언하여 그 안에 게시글 객체들을 넣어주는 방법이다!

   → const 형 배열의 특성을 이용한 것인데

   ex) const arr [1,2,3] → 안의 값은 변경할 수 없음.

   const arr[{1,2,3},{4,5},{6}] 의 형태라면 안의 객체 자체는 못지워도 [{1,3},{5},{6}] 이런 형태로 수정이 가능한 성질을 이용한 것이다.

# BoardView.vue

```
<template>
  <div class="board">
    <h1>게시판</h1>
    <div>
      <router-view></router-view>
    </div>
  </div>
</template>
<script>
export default {
  name: "BoardView",
};
</script>

<style scoped>
.board {
  background-color: aqua;
  text-decoration-color: brown;
}
</style>
```

- <router-view></router-view> 를 선언하여 저 자리에 알맞은 router 들이 화면에 데이터를 띄워줄 것이다.

# index.js (routing)

```
import Vue from "vue";
import VueRouter from "vue-router";
import HomeView from "../views/HomeView.vue";
import BoardView from "../views/BoardView.vue";

Vue.use(VueRouter);

const routes = [
  {
    path: "/",
    name: "home",
    component: HomeView,
  },
  {
    path: "/about",
    name: "about",
    component: () => import("../views/AboutView.vue"),
  },
  {
    path: "/board",
    name: "board",
    component: BoardView,
    redirect: "/board/list",
    children: [
      //board 안에서 바뀌는 라우터들을 관리
      {
        path: "list",
        name: "boardlist", // 글 리스트
        component: () => import("../components/board/BoardList.vue"),
        props: true,
      },
      {
        path: "write",
        name: "boardwrite", // 글 작성 폼
        component: () => import("../components/board/BoardWrite"),
      },
    ],
  },
  {
    path: "/todo",
    name: "todo",
    component: () => import("../views/TodoView.vue"),
  },
  {
    path: "/watch",
    name: "watch",
    component: () => import("../views/WatchView.vue"),
  },
  {
    path: "/ssabab",
    name: "ssabab",
    component: () => import("../views/SsababView.vue"),
  },
];

const router = new VueRouter({
  mode: "history",
  base: process.env.BASE_URL,
  routes,
});

export default router;
```

```
{
    path: "/board",
    name: "board",
    component: BoardView,
    redirect: "/board/list",
    children: [
      {
        path: "list",
        name: "boardlist",
        component: () => import("../components/board/BoardList.vue"),
        props: true,
      },
      {
        path: "write",
        name: "boardwrite",
        component: () => import("../components/board/BoardWrite"),
      },
    ],
  },

```

- 이 보드 list 를 계속 보여주게 하고 다른 라우팅 처리를 해주었다.

# BoardWrite.vue

```
<template>
  <div class="writestyle">
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
</template>

<script>
export default {
  name: "BoardWrite",
  data() {
    return {
      temp: [],
      article: { no: "", title: "", author: "", content: "" },
    };
  },
  methods: {
    doWrite() {
      this.$router.push({
        name: "boardlist",
        params: this.article,
      });
    },
  },
};
</script>

<style>
.writestyle {
  background-color: bisque;
}
</style>
```

- 버튼이 눌리면 라우터 push를 해주는 간단한 기능만이 있다.
- 기존의 해결 방법을 생각했을 땐 여기서 $route.params 로 배열을 받아와서 그 배열에 article을 push 해주고 다시 list로 보내주려 하였다.

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
    <button @click="moveWrite">글쓰러 가기</button><br />
  </div>
</template>

<script>
const arr = [
  { no: 1, title: "안녕하세요", author: "안싸피", content: "안녕안녕" },
  { no: 2, title: "안녕히계세요", author: "장싸피", content: "안녕" },
];
export default {
  name: "BoardList",
  data() {
    return {
      writes: arr,
      article: { no: "", title: "", author: "", content: "" },
    };
  },
  methods: {
    moveWrite() {
      this.$router.push({ name: "boardwrite" });
    },
  },
  created() {
    let art = this.$route.params;
    if (art != undefined && art.title && art.author && art.content) {
      art.no = this.writes.length + 1;
      this.writes.push(art);
      art = "";
    }
  },
};
</script>

<style scoped>
.donestyle {
  text-decoration: line-through;
  color: darkgray;
}
table {
  background-color: blueviolet;
  width: 100%;
}
</style>
```

## 동작 구조 파악하기

```
const arr = [
  { no: 1, title: "안녕하세요", author: "안싸피", content: "안녕안녕" },
  { no: 2, title: "안녕히계세요", author: "장싸피", content: "안녕" },
];
```

- 이게 아까 언급했던 미리 선언해둔 const 이다.

```
 moveWrite() {
      this.$router.push({ name: "boardwrite" });
    },
```

- moveWrite() 이다. boardwrite 뷰를 부르는 기능만 있지만 처음 해결 방법으로 풀이하려 할 때는 “params:writes” 를 붙여서 배열을 보내주려 하였었는데 그런 방법은 구현 못했으니 삭제하였다.

```
created() {
    let art = this.$route.params;
    if (art != undefined && art.title && art.author && art.content) {
      art.no = this.writes.length + 1;
      this.writes.push(art);
      art = "";
    }
  },

```

- vue 객체가 만들어 졌을 때 이 입력받아 만들어온 article 안에 비어있는 값이 있나 체크를 해준다.
- 사실 이런 null 값 체크는 입력받을 때 하여 사용자에게 값을 입력 받도록 유도를 하는 것이 좋지만
- 기능 구현에 너무 많은 시간을 써버려서 어쩔 수 없이 아까 여기에 써둔 것만 남아있게 되었다.
