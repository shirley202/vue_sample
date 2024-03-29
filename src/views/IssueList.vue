<template>
  <div>
    <h1>Lista de Problemas</h1>
    <el-button type="success" @click="getIssues()">Obtener Problemas</el-button>
    <div v-if="issues.length > 0">
      <el-card v-for="issue in issues" :key="issue.id" class="issue-card" shadow="hover">
        <div class="issue-content">{{ issue.title }}</div>
        <el-button @click="removeIssue(issue)" type="success" icon="el-icon-check" circle></el-button>
      </el-card>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      issues: []
    };
  },
  methods: {
    getIssues() {
      axios.get('https://api.github.com/repos/diveintocode-corp/vue_seriese_api/issues', {
        headers: {
          'Accept': 'application/vnd.github.v3+json',
          'Content-Type': 'application/json',
        },
      })
      .then((res) => {
        this.issues = res.data;
      })
      .catch((error) => {
        console.error('Error fetching issues:', error);
      });
    },
    removeIssue(issue) {
      const index = this.issues.indexOf(issue);
      if (index !== -1) {
        this.issues.splice(index, 1);
      }
    }
  }
};
</script>

<style scoped>
.issue-card {
  margin-bottom: 10px;
}

.issue-content {
  font-weight: bold;
}
</style>
