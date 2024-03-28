<template>
  <div>
    <h1>issueリスト</h1>
    <el-button type="success" @click="getIssues()">issue取得</el-button>
    <div v-if="issues.length > 0">
      <ul>
        <li v-for="issue in issues" :key="issue.id" @click="showIssueDetails(issue)">
          {{ issue.title }}
        </li>
      </ul>
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
    showIssueDetails(issue) {
      console.log('Clicked issue:', issue);
      // Aquí puedes realizar acciones adicionales cuando se hace clic en un problema
    }
  }
};
</script>
