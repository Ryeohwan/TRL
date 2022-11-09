# Vue 프로젝트(1) - router, vuex 등

# vue cli 의 기본 구조 이해하기

# 설치하기

```
vue create myapp1
   - Manually select features
   - *Babel *Router * Vuex *Linter
   - 2.x
   - history mode (enter)
   - ESLint + Prettier
   - * Lint on save
   - package.json
   - preset이름 설정
```

# NPM Server 시작하기

`npm run serve`

<br>

# index. js확인

```
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false
// 이거는 기본 설정 신경 ㄴㄴ

new Vue({  // 이게 중요한 내용
  render: h => h(App),  // 렌더링한대... 화살표 함수니까 앞 h 가 인자값 그 인자를 호출인데 h는 function(이것도 객체) app 은 객체
}).$mount('#app')  // 이 뷰 객체에 있는 마운트라는 함수를 호출한다. mount 라는 함수는 mounted 되는 라이프 사이클에서 훅 있잖아 그거 말하는겨
// 그니까 el:"#app" 이랑 같은 거다. 근데 App 는 App.vue 인 것이다.

// 이해 순서 main.js -> app.vue
```

# app.vue

```
<template> <!--html 해석해야 할 것들 안에 커스텀 태그들은 해석이 되어야 한다. 실제로는 엘레멘트 될 수 없다....-->
   <div id="app"> <!--뷰 스타일의 자바 스크립트인 것인데 자바 스크립트 엔진이 알 수 있도록 해석을 해줘야 하는 것이다. 그래서 전통적인 js 해석되어 들어가는 것 -->
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/> <!-- helloworld 는 아래애서 해석-->
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue' // 여기서도 export 되어 있구나 } 없는 거 보고 판단

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

- html 해석해야 할 것들 안에 커스텀 태그들은 해석이 되어야 한다. 실제로는 엘레멘트 될 수 없다....
- 뷰 스타일의 자바 스크립트인 것인데 자바 스크립트 엔진이 알 수 있도록 해석을 해줘야 하는 것이다. 그래서 전통적인 js 해석되어 들어가는 것
- import HelloWorld from './components/HelloWorld.vue' // 여기서도 export 되어 있구나 }가 없는 거 보고 판단

<br>

# HelloWorld.vue

```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>  // export default 되어 있네 보니까 이 객체가 방출되는 것인데 얘 이름이 hello world 인데 이름만 지었는데 안에를 다 js로 맹글어 방출
export default {
  name: 'HelloWorld', // helloworld 태그를 가진 애가 부모인데 app.vue에 부모 있다.
  props: {  // 부모가 자식한테 데이터 보낼 때 props
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
```

- router-link 는 router 안에 있는 index.js 를 실행시켜라.
- app.vue 에서 router.vue 가동시켜라
- to는 index.js 에서 path 랑 연결이 된다.

  <br>

---

---

# 여기부터 ToDoList 만들기

# index.js

```
import Vue from 'vue'
import VueRouter from 'vue-router'
import HomeView from '../views/HomeView.vue'
import BoardView from '../views/BoardView.vue'

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'home',
    component: HomeView
  },
  {
    path: '/about',
    name: 'about',
    component: () => import('../views/AboutView.vue')
  },
  {
    path: '/board',
    name: 'board',
    component: BoardView
  },
  {
    path: '/todo',
    name: 'todo',
    component: () => import('../views/TodoView.vue')
  },
]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

export default router
```

- index.js 는 라우팅을 해주는 것이다. (실제로도 route 폴더 안에 들어있다.)

```
import BoardView from '../views/BoardView.vue'
{
    path: '/board',
    name: 'board',
    component: BoardView
  }
```

## 라우팅에는 두 가지의 방법이 있다.

```
import BoardView from '../views/BoardView.vue'
{
    path: '/board',
    name: 'board',
    component: BoardView
  },

//////////////////////////////////////////////////////////
{
    path: '/todo',
    name: 'todo',
    component: () => import('../views/TodoView.vue')
  },
```

<br>

# TodoView.vue

```
<template>
  <div class = "todo">
    <h1>to do</h1>
    <div>
      <todo-header></todo-header>
      <todo-input @addTodo="addTodo"></todo-input>
      <todo-list :todos="todos"></todo-list> <!--이렇게 넘겨주는 것이다.-->
      <todo-footer :todos="todos"></todo-footer>
    </div>
  </div>
</template>

<script>
import TodoHeader from '../components/todo/TodoHeader.vue'
import TodoInput from '../components/todo/TodoInput.vue'
import TodoList from '../components/todo/TodoList.vue'
import TodoFooter from '../components/todo/TodoFooter.vue'

export default {
  name:"TodoView",
  components:{
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter
  },
  data(){
    return{
      todos:[],
    }
  },
  methods:{
    addTodo(item){
      console.log(item);
      this.todos.push(item);
    }
  }
}
</script>

<style scoped>
.todo{
  color: azure;
  background-color: rebeccapurple;
}

</style>
```

- import 해서 다른 자식 vue 들로부터 값을 주고 받을 수 있다.

```
<template>
  <div class = "todo">
    <h1>to do</h1>
    <div>
      <todo-header></todo-header>
      <todo-input @addTodo="addTodo"></todo-input>
      <todo-list :todos="todos"></todo-list> <!--이렇게 넘겨주는 것이다.-->
      <todo-footer :todos="todos"></todo-footer>
    </div>
  </div>
</template>
```

- template 는 html 에게 이해시킬 값들이라고 보면 된다. 지금은 vue 자바스크립트로 되어있지만 template 태그를 통해서 바닐라 자바 스크립트로 변환을 해준다고 한다.

- :로 값을 자식들에게 넘겨주고 @로 받아온다.

```
export default {
  name:"TodoView",
  components:{
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter
  },
  data(){
    return{
      todos:[],
    }
  },
  methods:{
    addTodo(item){ // 이러면 헷갈린다. 여러군데 생겨야 하는데 똑같은게 또 와서....
      console.log(item);
      this.todos.push(item);
    }
  }
}
```

- name은 이 view 가 무엇인지 우리가 쉽게 확인하기 위해 쓴다.
- components 들은 등록을 해두는 것이다. 이 vue의 자식 vue들을 연결해 두는 것이고 자식과 부모는 데이터를 주고 받을 수 있다.
- data 는 component 에서는 함수의 형태를 갖게 해야 한다.
- methods는 함수 지정인데 addTodo 를 써서 자식으로부터 전달 받은 todos 안의 item을 todos 라는 배열에 집어넣는 기능을 하고 있다.

# TodoHeader.vue

```
<template>
  <div class="todoHeader">
   작업 관리

  </div>
</template>

<script>
export default {
  name: 'TodoHeader',

}
</script>


<style scoped>

</style>
```

- 별다른 기능을 구현해두지 않은 component의 기본적인 형태이다.

# TodoInput.vue

```
vue<template>
  <div class="TodoInput">
   <input v-model="job" placeholder="할 일" @keyup.enter="addTodo">
  </div>
</template>

<script>
export default {
  name: 'TodoInput',
  //data:function(){ // component 들은 변수를 function 으로 줘야 한다.
  data(){ // 축약 가능
    return {
      job:"",
    }
  },
  methods:{
    addTodo(){
      console.log(this.job);
      let item = {done:false,text:this.job};
      this.job = "";
      this.$emit("addTodo",item);  // 자식이 부모에게 보내는 것
      // 부모는 todoView
    }
  }
}
</script>


<style scoped>

</style>
```

- 동작 원리를 살펴보자!

## 동작 원리

- 먼저 v-model 로 연결되어 input 받은 값이 job 으로 들어 갑니다.
- 그리고 @keyup.enter 로 이벤트 리스너가 addTodo 를 호출합니다.

- 그렇게 되면 여기서 export 를 해주는데

```
export default {
  name: 'TodoInput',
  //data:function(){ // component 들은 변수를 function 으로 줘야 한다.
  data(){ // 축약 가능
    return {
      job:"",
    }
  },
  methods:{
    addTodo(){
      console.log(this.job);
      let item = {done:false,text:this.job};
      this.job = "";
      this.$emit("addTodo",item);  // 자식이 부모에게 보내는 것
      // 부모는 todoView
    }
  }
}
```

<br>

- data 에 job을 초기화를 해두고

```
data(){ // 축약 가능
    return {
      job:"",
    }
  },
```

- emit 사용하여 자식에서 부모로 데이터를 보냅니다.

```
let item = {done:false,text:this.job};
      this.job = "";
      this.$emit("addTodo",item);
```

> 그러니까 TodoInput 은 사용자에게 text 를 입력받아서 그 값을 $emit 으로 부모인 TodoView로 보내주는 역할을 합니다.

# TodoList.vue

```
<template>
  <div class="TodoList">
   <div v-for="(item,index) in todos" :key="index">
      <input type="checkbox" v-model="item.done">
      <span :class="{donestyle:item.done}">{{item.text}}</span>
   </div>
  </div>
</template>

<script>
export default {
  name: 'TodoList',
  props:[ // 부모로 부터 받는 것이 여러 개 있다.
    "todos"
  ],


}
</script>


<style scoped>
.donestyle{
  text-decoration: line-through;
  color: darkgray;
}

</style>
```

## 동작 원리를 알아보자!

```
<div v-for="(item,index) in todos" :key="index">
      <input type="checkbox" v-model="item.done">
      <span :class="{donestyle:item.done}">{{item.text}}</span>
   </div>
```

- v-for 를 사용하여 todos 배열 안에 있는 값을 check 옆에다가 붙여주는데 item.done 의 부울값을 확인하여 style을 넣어주고 있다.

```
props:[ // 부모로 부터 받는 것이 여러 개 있다.
    "todos"
  ],
```

- props 는 부모로 부터 받아온 값을 지정해둔 곳이다.
- 여기서는 todos 라는 객체를 받아왔다.

# TodoFooter.vue

```
<template>
  <div class="TodoFooter">
   <p>{{doneCnt}}/{{todos.length}}원 쵸디</p>

  </div>
</template>

<script>
export default {
  name: 'TodoFooter',
  props:["todos"],
  computed:{
    doneCnt(){
      return this.todos.filter(function(item){
        return item.done
      }).length;
    }
  }
}
</script>


<style scoped>

</style>
```

## 동작 원리 보기

```
props:["todos"],
```

- 부모로부터 todos 값을 받아왔다.

```
<template>
  <div class="TodoFooter">
   <p>{{doneCnt}}/{{todos.length}}원 쵸디</p>

  </div>
</template>
```

- 여기에 받아온 todos 를 이용하여 무스타치를 넣어주었는데
- doneCnt 라는 함수의 리턴 값을 넣어주었다.

```
<script>
export default {
  name: 'TodoFooter',
  props:["todos"],
  computed:{
    doneCnt(){
      return this.todos.filter(function(item){
        return item.done
      }).length;
    }
  }
}
</script>
```

- 필터를 거쳐서 item.done 이 true 인 애들의 수를 파악해주는 doneCnt() 라는 함수이다.

# 결과 화면

![1](https://user-images.githubusercontent.com/73810834/200833869-5fd80959-ed1e-495f-91d6-962b663a9939.png)

# 완료한 할 일 목록 삭제하기

# TodoList.vue

```
<template>
  <div class="TodoList">
   <div v-for="(item,index) in todos" :key="index">
      <input type="checkbox" v-model="item.done">
      <span :class="{donestyle:item.done}">{{item.text}}</span>
   </div>
   <button @click="removeTodo">완료한 항목 삭제</button>
  </div>
</template>

<script>
export default {
  name: 'TodoList',
  props:[ // 부모로 부터 받는 것이 여러 개 있다.
    "todos"
  ],
  methods:{
    removeTodo(){
      this.$emit("removeTodo",this.todos)
    }
  }


}
</script>


<style scoped>
.donestyle{
  text-decoration: line-through;
  color: darkgray;
}

</style>
```

`<button @click="removeTodo">완료한 항목 삭제</button>` 을 통해서 removeTodo 메소드를 호출한다.

```
 removeTodo(){
      this.$emit("removeTodo",this.todos)
    }
```

- 여기선 부모의 removeTodo 를 호출하기만 하면서 마무리한다.

<br>

# TodoView.vue

```
<template>
  <div class = "todo">
    <h1>to do</h1>
    <div>
      <todo-header></todo-header>
      <todo-input @addTodo="addTodo"></todo-input>
      <todo-list @removeTodo="removeTodo" :todos="todos"></todo-list> <!--이렇게 넘겨주는 것이다.-->
      <todo-footer :todos="todos"></todo-footer>
    </div>
  </div>
</template>

<script>
import TodoHeader from '../components/todo/TodoHeader.vue'
import TodoInput from '../components/todo/TodoInput.vue'
import TodoList from '../components/todo/TodoList.vue'
import TodoFooter from '../components/todo/TodoFooter.vue'

export default {
  name:"TodoView",
  components:{
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter
  },
  data(){
    return{
      todos:[],
    }
  },
  methods:{
    addTodo(item){ // 이러면 헷갈린다. 여러군데 생겨야 하는데 똑같은게 또 와서....
      console.log(item);
      this.todos.push(item);
    },
    removeTodo(){
      this.todos = this.todos.filter(function(item){
        return item.done == false;
      })
    }
  }
}
</script>

<style scoped>
.todo{
  color: azure;
  background-color: rebeccapurple;
}

</style>
```

`<todo-list @removeTodo="removeTodo" :todos="todos"></todo-list> ` @removeTodo 로 자식으로부터 읽어온다.

이후

```
removeTodo(){
      this.todos = this.todos.filter(function(item){
        return item.done == false;
      })
    }
```

- 이 함수에서는 item.done 이 false 가 true 인 경우의 애들만 필터링해서 this.todos의 값으로 넣어주면서 삭제 기능은 마무리 된다.
