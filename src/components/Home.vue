<template>
    <div class="row">
        <category v-for="(category, idx) in categories" :key="idx" :category="category" :tasks="tasks"></category>
    </div>
</template>

<script>
import Category from './Category'
export default {
    name: "Home",
    components: {
        Category
    },
    props: ['categories', 'tasks']
}
</script>

<style>

</style>