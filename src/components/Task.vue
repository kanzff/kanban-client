<template>
    <div class="card mb-3">
        <div class="card-body">
            <h5 class="card-title">{{task.title}}</h5>
            <p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
            <a v-if="category !== 'backlog'" class="btn btn-primary">&larr;</a>
            <a href="#" class="btn btn-primary">Edit</a>
            <a href="#" class="btn btn-primary">Delete</a>
            <a v-if="category !== 'done'" class="btn btn-primary">&rarr;</a>
        </div>
    </div>
</template>

<script>
export default {
    name: "Task",
    props: ['task', 'category']
}
</script>

<style>

</style>