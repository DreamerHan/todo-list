<template>
    <footer class="footer">
        <span class="todo-count">
            <strong>{{unCheckedLen}}</strong>
            <span>条未选中</span>
        </span>
        <ul class="filters">
            <li><a href="javascript:void (0)" :class="{selected : nowHash === 'all' }" @click.prevent="handleHash('all')">All</a></li>
            <li><a href="javascript:void (0)" :class="{selected : nowHash === 'active' }" @click.prevent="handleHash('active')">Active</a></li>
            <li><a href="javascript:void (0)" :class="{selected : nowHash === 'completed' }" @click.prevent="handleHash('completed')">Completed</a></li>
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

