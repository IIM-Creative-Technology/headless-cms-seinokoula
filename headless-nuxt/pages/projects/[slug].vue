<template>
  <div class="container" v-if="project">
    <div class="card">
      <p> url single : {{ $route.params.slug }}</p>
      <br>
      <!-- <img :src="project.image.url" :alt="project.image.alternativeText" /> -->
      <img :src="project?.image?.url" :alt="project?.image?.alternativeText" />
      <p>nom du projet : {{ project.name }}</p>
      <p>description du projet : {{ project.description }}</p>
      <p v-for="technology in project.technologies" :key="technology.id">techno utilisée : {{ technology.techno }}</p>
      <p v-for="type_of_projects in project.type_of_projects" :key="type_of_projects.id">type de projet : {{
        type_of_projects.sections }}</p>
      <p><a :href="project.link" target="_blank">Link to project</a></p>
      <div>
        <button @click="prevProject">Previous Project</button>
        <button @click="nextProject">Next Project</button>
      </div>
    </div>
  </div>
</template>

<script setup>
const { find, findOne } = useStrapi()
const route = useRoute()
const project = ref()
const allProjects = ref([])

onMounted(async () => {
  const data = await find('projects', '_sort=createdAt:desc&_limit=-1')
  allProjects.value = data.data
  project.value = await findOne(`projects?filters[slug]=${route.params.slug}&populate=deep`)
  project.value = project.value.data[0]
})

const prevProject = () => {
  const currentIndex = allProjects.value.findIndex(p => p.slug === project.value.slug)
  if (currentIndex > 0) {
    project.value = allProjects.value[currentIndex - 1]
  }
}

const nextProject = () => {
  const currentIndex = allProjects.value.findIndex(p => p.slug === project.value.slug)
  if (currentIndex < allProjects.value.length - 1) {
    project.value = allProjects.value[currentIndex + 1]
  }
}

</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 2rem;
}

.card {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #fff;
  box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.2);
  border-radius: 5px;
  padding: 2rem;
  margin-bottom: 2rem;
  width: 100%;
}

img {
  max-width: 500PX;
  height: auto;
  margin-bottom: 2rem;
  border-radius: 5px;
}

p {
  margin-bottom: 1rem;
  padding: 0.5rem;
  border-radius: 5px;
  /* align the text to the left */
  text-align: left;
}

a {
  color: #3f51b5;
  text-decoration: none;
  font-weight: bold;
  transition: all 0.2s ease-in-out;
}

a:hover {
  color: #283593;
  transform: scale(1.05);
}

.buttons {
  display: flex;
  justify-content: space-between;
  width: 100%;
  margin-top: 2rem;
}

button {
  border: none;
  background-color: #3f51b5;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 30px;
}
</style>
