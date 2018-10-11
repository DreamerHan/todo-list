<template>
    <header class="header">
        <h1>todos</h1>
        <input class="new-todo" placeholder="请输入内容"
            v-model.trim="todoVal"
            @keyup.enter="handleAdd"
        />
    </header>
</template>
<script>
export default {
    data(){
        return {
            todoVal : ''
        }
    },
    methods :{
        handleAdd(){
           if ( !this.todoVal ) return;
            this.$emit('addFn', this.todoVal);
            this.todoVal = '';
        }
    }
}
</script>

