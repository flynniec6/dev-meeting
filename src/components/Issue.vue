<template>
  <div class="issue">
    <span class="issue-number">{{ issue.iid }}</span>
    <span class="issue-title">{{ issue.title }}</span>
    <span v-if="issue.time_stats.human_time_estimate" class="issue-estimate">({{issue.time_stats.human_time_estimate}})</span>
  </div>
</template>

<script>
export default {
  name: "Issue",
  props: {
    issue: {type: Object, required: true}
  }
}
</script>

<style scoped>
.issue {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
}
.issue-number {
  display: inline-block;
  flex-basis: 2.5em;
  margin-right: 1em;
  font-weight: bold;
  text-align: right;
}
.issue-estimate {
  margin-left: 1em;
  color: #888;
}

</style>