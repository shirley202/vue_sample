<template>
  <div>
    <h1>todoリスト</h1>
    <!-- todo入力フォーム -->
    <form @submit.prevent="addTodo()">
      <el-input placeholder="todo" v-model="todo"></el-input>
    </form>
    <el-row :gutter="12">
      <!-- todo表示エリア -->
      <TodoItem v-for="(todo, index) in todos" :key="index" :todo="todo" @remove="removeTodo" :index="index" />
      <!-- issue表示エリア -->
      <el-col :span="12"  v-for="(issue, index) in issues" :key="issue.id">
        <el-card class="box-card" shadow="hover" style="margin: 5px 0;">
          <el-row :gutter="12">
            <el-col :span="21">{{ issue.title }}</el-col>
            <el-col :span="3">
              <el-button @click="closeIssue(index)" type="success" icon="el-icon-check" circle></el-button>
            </el-col>
          </el-row>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>


<script>
import axios from 'axios';
import TodoItem from '@/components/TodoItem';

const client = axios.create({
  baseURL: `${process.env.VUE_APP_GITHUB_ENDPOINT}`,
  headers: {
    'Authorization': `token ${process.env.VUE_APP_GITHUB_TOKEN}`,
    'Accept': 'application/vnd.github.v3+json',
    'Content-Type':'application/json',
  },
})

export default {
  name: 'TodosIssues',
  components: {
    TodoItem
  },
  data () {
    return {
      todo: '',
      todos: [],
      issues: []
    }
  },
  methods: {
    addTodo(){
      this.todos.push(this.todo);
      this.todo= '';
    },
    removeTodo(index){
      this.todos.splice(index, 1);
    },
    closeIssue(index){
      const target = this.issues[index];
      client.patch(`/issues/${target.number}`,
          {
            state: 'closed'
          },
        )
        .then(() => {
         this.issues.splice(index, 1);
      })
    },
    getIssues() {
  const endpoint = 'issues';
  console.log('Requesting issues from:', endpoint);

  client.get(endpoint)
    .then((res) => {
      console.log('Response from GitHub:', res.data); // Verifica la respuesta de GitHub
      this.issues = res.data;
    })
    .catch((error) => {
      console.error('Error fetching issues:', error);
    });
}


  },
  created() {
    this.getIssues();
  }
}
</script>