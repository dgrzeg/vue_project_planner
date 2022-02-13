<template>
  <form @submit.prevent="updateProject">
    <label>Title</label>
    <input type="text" required v-model="title" />
    <label>Details</label>
    <textarea required v-model="details"></textarea>
    <button>Update Project</button>
  </form>
</template>

<script>
export default {
  props: ['id'],
  data() {
    return {
      title: '',
      details: '',
      url: `http://127.0.0.1:3000/projects/${this.id}`,
    };
  },
  methods: {
    updateProject() {
      fetch(this.url, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          title: this.title,
          details: this.details,
        }),
      })
        .then(() => this.$router.push({ name: 'Home' }))
        .catch((err) => console.log(err.message));
    },
  },
  mounted() {
    fetch(this.url)
      .then((res) => res.json())
      .then((data) => {
        this.title = data.title;
        this.details = data.details;
      })
      .catch((err) => console.log(err.message));
  },
};
</script>

<style></style>
