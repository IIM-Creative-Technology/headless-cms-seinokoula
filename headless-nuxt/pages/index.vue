<template>
    <div v-if="projects">
        <NuxtLink :to="`/projects/${project.slug}`" v-for="project in projects.data">
            {{ project.name }}
            <img :src="project.image.url" :alt="project.image.alternativeText" />
            <br><br><br>
        </NuxtLink>
    </div>
</template>

<script setup>
const { find } = useStrapi()
const projects = ref()

onMounted(async () => {
    projects.value = await find('projects', { populate: 'deep' })
    // console.log('PROJECTS', projects.value)
})
</script>

<style scoped>
div {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin: 0 auto;
    width: 100%;
    height: 100%;
    background-color: #f5f5f5;
}

img {
    width: 100px;
    height: 100px;
}
</style>
