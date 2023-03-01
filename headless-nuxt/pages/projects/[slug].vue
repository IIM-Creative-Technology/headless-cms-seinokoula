<template>
    <div v-if="project">
        <p> url single : {{ $route.params.slug }}</p>
        <br>
        <img :src="project.image.url" :alt="project.image.alternativeText" />
        <p>nom du projet : {{ project.name }}</p>
        <p>description du projet : {{ project.description }}</p>
        <p v-for="technology in project.technologies" :key="technology.id">techno utilisée : {{ technology.techno }}</p>
        <p v-for="type_of_projects in project.type_of_projects" :key="type_of_projects.id">type de projet : {{ type_of_projects.sections }}</p>
        <p><a :href="project.link" target="_blank">Link to project</a></p>
        <!-- display the map of technologies -->
    </div>
</template>

<script setup>
const { findOne } = useStrapi()
const route = useRoute()
const project = ref()

// const technologies = computed(() => {
//     return project.value.technologies.map(technology => technology.techno)
// })

// console.log('lola',technologies)

onMounted(async () => {
    project.value = await findOne(`projects?filters[slug]=${route.params.slug}&populate=deep`)
    project.value = project.value.data[0]
    console.log(project.value)
})


</script>

<style scoped>
div {
    align-items: center;
    justify-content: center;
    margin: 0 auto;
    width: 100%;
    height: 100%;
    background-color: #f5f5f5;
}

p {
    font-size: 1.5rem;
    font-weight: 500;
    color: #333;
}

img {
    width: 250px;
    height: 250px;
}

a {
    color: #c75ac4;
    text-decoration: none;
    font-size: 1.5rem;
    font-weight: 500;
    border: 2px solid #c75ac4;
    border-radius: 30px;
    padding: 0.5rem;
}
</style>