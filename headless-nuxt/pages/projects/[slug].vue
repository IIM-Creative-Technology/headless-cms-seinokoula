<template>
    <div class="container" v-if="project">
        <div class="card">
        <p> url single : {{ $route.params.slug }}</p>
        <br>
        <img :src="project.image.url" :alt="project.image.alternativeText" />
        <p>nom du projet : {{ project.name }}</p>
        <p>description du projet : {{ project.description }}</p>
        <p v-for="technology in project.technologies" :key="technology.id">techno utilisée : {{ technology.techno }}</p>
        <p v-for="type_of_projects in project.type_of_projects" :key="type_of_projects.id">type de projet : {{ type_of_projects.sections }}</p>
        <p><a :href="project.link" target="_blank">Link to project</a></p>
        </div>
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
.container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    border: 1px solid black;
}

.card{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    border: 1px solid black;
    margin-bottom: 2rem;
    width: 50%;
}

img {
    max-width: 50%;
    align-items: center;
    justify-content: center;
    display: flex;
    height: auto;
    margin-bottom: 2rem;
}

p {
    margin-bottom: 10px;
    border: 1px solid black;
    /* align the text to the left */
    text-align: left;
}

a {
    color: blue;
}
</style>
