<template>
    <div class="title">
        <h1>Portfolio</h1>
        <p>Bonjour à tous, je m’appelle Alexandre Charles, Web Développeur en devenir.</p>
        <p>Vous pouvez voir tous mes talents de CSS à l'œuvre sur mon portfolio même</p>
    </div>
    <div class="container">
        <div class="card-container" v-if="projects">
            <button @click="filterProjects('all')">reset</button>
            <button v-for="type in types" @click="filterProjects(type)">{{ type }}</button>
            <div class="cards">
                <div v-for="project in filteredProjects" :key="project.slug" class="card">
                    <NuxtLink :to="`/projects/${project.slug}`">
                        <h3>{{ project.name }}</h3>
                        <p v-for="technology in project.technologies" :key="technology.id">techno utilisée : {{
                            technology.techno }}</p>
                        <p v-for="type_of_projects in project.type_of_projects" :key="type_of_projects.id">type de projet :
                            {{ type_of_projects.sections }}</p>
                        <img :src="project.image.url" :alt="project.image.alternativeText" />
                        <button :to="`/projects/${project.slug}`">more info</button>
                    </NuxtLink>
                </div>
            </div>
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
button {
    background-color: #ddd;
    border: none;
    border-radius: 20px;
    color: #333;
    cursor: pointer;
    font-size: 16px;
    margin: 0 10px 10px 0;
    padding: 10px 20px;
    transition: all 0.3s ease;
    border: 0.5px solid #333;
    box-shadow: 0 0 0 0.5px #333;
}

button:hover {
    background-color: #aaa;
    color: #fff;
}

button.active {
    background-color: #333;
    color: #fff;
}

.title {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-bottom: 2rem;
}

.container {
    display: flex;
    justify-content: center;
}

.card-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
}

.cards {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

.card {
    width: 100%;
    max-width: 400px;
    margin: 0 10px 20px 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
    border-radius: 10px;
    overflow: hidden;
    text-align: center;
    display: flex;
    /* add this */
    justify-content: center;
    /* add this */
}

.card button {
    align-self: center;
}

.card:hover {
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
    transition: ease-in-out 0.3s;
}

.card img {
    width: 100%;
    height: 200px;
    object-fit: cover;
}

.card h3,
.card p {
    margin: 10px;
}
</style>
