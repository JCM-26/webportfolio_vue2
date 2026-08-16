<template>
    <section id="projects" class="py-5">
        <div class="container py-4">

            <div class="text-center mb-5">
                <h2 class="section-title center d-inline-block">
                    Featured Projects
                </h2>

                <p class="text-muted">
                    A curated collection of my recent engineering and development tasks
                </p>
            </div>

            <div class="row g-4 justify-content-center">
                
                <ProjectCard
                    v-for="project in projects"
                    :key="project.id"
                    :project="project"
                />

            </div>

        </div>
    </section>
</template>

<script setup>
import ProjectCard from './ProjectCard.vue'
import projects from '../data/projects.json'
</script>