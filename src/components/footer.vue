<template>
    <footer class="footer">
        <span class="todo-count">
            <strong>{{unCheckedLen}}</strong>
            <span>条未选中</span>
        </span>
        <ul class="filters">
            <li><a href="#/all" :class="{selected : nowHash === 'all' }" @click="handleHash('all')">All</a></li>
            <li><a href="#/active" :class="{selected : nowHash === 'active' }" @click="handleHash('active')">Active</a></li>
            <li><a href="#/completed" :class="{selected : nowHash === 'completed' }" @click="handleHash('completed')">Completed</a></li>
        </ul>
    </footer>
</template>
<script>
export default {
    props :{
        unCheckedLen : {
            type : Number,
            default : 0
        },
        nowHash : {
            type : String,
            default : 'all'
        }
    },
    methods : {
        handleHash(value){
            this.$emit('hashFn', value);
        }
    }
}
</script>

