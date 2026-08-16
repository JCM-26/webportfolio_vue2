<template>
    <div class="col-12 col-md-6 col-lg-4">
        <div class="project-card">

            <div class="project-img-placeholder">
                <img
                    class="project-imgs"
                    :src="project.image"
                    :alt="project.title"
                >
            </div>
            
                <div class="card-body">
                    <h5 class="card-title fw-bold">
                        {{ project.title }}
                    </h5>

                    <p class="card-text text-muted">
                        {{ project.description }}
                    </p>

                    <a
                        href="#"
                        class="btn btn-outline-emerald btn-sm w-100"
                    >
                        View Project
                    </a>
                </div>
            

        </div>
    </div>
</template>

<script setup>
defineProps({
    project: Object
})
</script>