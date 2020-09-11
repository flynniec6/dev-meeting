<template>
  <div class="sprint">
    <h1>Current Sprint</h1>
    <div v-if="tasks.length === 0">{{ message }}</div>
    <ul>
      <li v-for="task in tasks" :key="task.iid"><issue :issue="task"></issue></li>
    </ul>
  </div>

</template>

<script>
import {onMounted, ref} from "vue";
import axios from "axios";
import Issue from "@/components/Issue";

export default {
  components: {
    Issue
  },
  setup() {
    const message = ref("No tasks in current sprint");
    let tasks = ref([]);
    onMounted(() => {
      const token = process.env.VUE_APP_GITLAB_TOKEN;
      const ajax = axios.create({
        headers: {
          'X-Requested-With': 'XmlHttpRequest',
          'Accepts': 'application/json',
          'Authorization': `Bearer ${token}`
        }
      });
      ajax.get("https://gitlab.gyron.net/api/v4/projects/1/issues", {
        params: {
          state: "opened",
          per_page: 100
        }
      })
          .then(response => {
            let tmpTasks;
            tmpTasks = response.data.filter(task => {
              return task.labels.includes("In Development") || task.labels.includes("Sprint") || task.labels.includes("Complete / Review");
            });
            tmpTasks.sort((a, b) => {
              if (a.iid > b.iid) {
                return 1;
              }
              if (a.iid < b.iid) {
                return -1;
              }
              return 0;
            });
            tmpTasks.forEach(i => {
              tasks.value.push(i);
            });
          })
          .catch((error) => {
            console.error(error)
          });


    });
    return {message, tasks}
  },
  name: "Sprint"
}
</script>

<style scoped>
ul {
  text-align: left;
  margin: 0 5%;
}

</style>