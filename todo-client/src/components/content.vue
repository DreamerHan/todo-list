<template>
    <section class="main">
        <input class="toggle-all" type="checkbox" v-model="isCheckedAll"/>
        <ul class="todo-list">
            <li v-for="(item,index) of taskList" :key="item.id"
                :class="{completed : item.checked, editing : editIndex === index}"
            >
                <div class="view">
                    <input class="toggle" type="checkbox" v-model="item.checked"/>
                    <label @dblclick="handleEdit(index)">{{item.title}}</label>
                    <button class="destroy" @click="handleDelete(item)"></button>
                </div>
                <input class="edit" 
                    v-focus="editIndex === index" 
                    v-model="item.title"
                    @blur="DoneEdit($event, item)"
                    @keyup.enter="DoneEdit($event, item)"
                    @keyup.esc="outEdit(item)"
                />
            </li>
        </ul>
    </section>
</template>
<script>
export default { 
    props :{
        taskList : {
            type : Array,
            default(){
                return [];
            }
        }
    },
    data(){
        return {
            editIndex : -1,
            beforeEdit : ''
        }
    },
    methods :{
        handleEdit(index){ //开始编辑
            this.beforeEdit = this.taskList[index].title;
            this.editIndex = index;
        },
        DoneEdit(e, target){ //编辑完成
            if(e.type === 'keyup'){
                e.target.blur();
                return;
            }

            if( target.title === '' ){
                this.handleDelete(target);
            }

            this.$emit('doneFn', target);
            this.editIndex = -1;
        },
        handleDelete(target){ //删除这一条
            this.$emit('deleteFn', target);
        },
        outEdit(target){ //退出编辑
            target.title = this.beforeEdit;
            this.editIndex = -1;
        }
    },
    computed : {
        isCheckedAll : {
            get(){
                return this.taskList.every( item => item.checked);
            },
            set(value){
                this.taskList.forEach( item => {
                    item.checked = value;
                } )
            }
        }
    }
}
</script>
