<template>
    <div class="container">
        <div class="card" v-if="projects">
            <button @click="filterProjects('all')">
                reset
            </button>
            <button v-for="type in types" @click="filterProjects(type)">
                {{ type }}
            </button>
            <NuxtLink :to="`/projects/${project.slug}`" v-for="project in filteredProjects">
                <h3>{{ project.name }}</h3>
                <p v-for="technology in project.technologies" :key="technology.id">techno utilisée : {{ technology.techno }}
                </p>
                <p v-for="type_of_projects in project.type_of_projects" :key="type_of_projects.id">type de projet : {{
                    type_of_projects.sections }}</p>
                <img :src="project.image.url" :alt="project.image.alternativeText" />
            </NuxtLink>
        </div>
    </div>
</template>

<script setup>
const { find } = useStrapi()

const projects = ref()
const types = ref([])
const activeFilter = ref('all')

const filterProjects = (type) => {
    activeFilter.value = type
}

const filteredProjects = computed(() => {
    if (activeFilter.value === 'all') {
        return projects.value.data
    }
    return projects.value.data.filter(project => {
        return project.type_of_projects.map(type => type.sections).includes(activeFilter.value)
    })
})

onMounted(async () => {
    projects.value = await find('projects', { populate: 'deep' })
    types.value = projects.value.data.map(project => {
        return project.type_of_projects.map(type => type.sections)
    }).flat()
    types.value = [...new Set(types.value)]

})
</script>

<style scoped>

.container{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin: 0 auto;
    width: 100%;
    height: 100%;
    background-color: #f5f5f5;
    margin-top: 1rem;
}

.card {
    border: 2px solid #c75ac4;
    text-align: center;
    padding: 1rem;

}

img {
    width: 100px;
    height: 100px;
}
</style>
