<template>
    <div id="app">
        <TodoApp/>
        <footer class="info">
            <p>双击可以修改一个todo</p>
            <p>由MiracleChen编写</p>
        </footer>
    </div>
</template>

<script>
    import TodoApp from './components/TodoApp.vue'

    export default {
        name: 'app',
        components: {
            TodoApp
        }
    }
</script>

<style src="./css/custom.css"></style>
