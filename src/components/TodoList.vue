<template>
    <div class="mb-4">
        <TodoItem
            v-for="resultats of data_storage" :key="resultats"
            v-bind:result="resultats"
        />
    </div>
</template>

<script>
import TodoItem from '@/components/TodoItem.vue'
export default{
    props: ['data_storage'],
    components: {
        TodoItem
    }
}
</script>