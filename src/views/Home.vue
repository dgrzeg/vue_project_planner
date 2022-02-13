<template>
  <div class="home">
    <FilterNav @filteChange="setCurrent" :current="current" />
    <div v-if="filteredProjects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject
          :project="project"
          @delete="handleDelete"
          @complete="handleComplete"
        />
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue';
import FilterNav from '../components/FilterNav.vue';

export default {
  name: 'Home',
  components: {
    SingleProject,
    FilterNav,
  },
  data() {
    return {
      projects: [],
      current: 'all',
    };
  },
  methods: {
    setCurrent(current) {
      this.current = current;
    },
    handleDelete(id) {
      this.projects = this.projects.filter((project) => project.id !== id);
    },
    handleComplete(id) {
      const p = this.projects.find((project) => project.id === id);
      p.complete = !p.complete;
    },
  },
  computed: {
    filteredProjects() {
      if (this.current === 'ongoing')
        return this.projects.filter((el) => !el.complete);
      if (this.current === 'completed')
        return this.projects.filter((el) => el.complete);
      return this.projects;
    },
  },
  mounted() {
    fetch('http://localhost:3000/projects')
      .then((res) => res.json())
      .then((data) => (this.projects = data))
      .catch((err) => console.log(err.message));
  },
};
</script>
