<template>
  <div id="app">
    <section class="todoapp">
        <todoHeader @addFn="handleAddFn"></todoHeader>
        <todoContent
          v-show="taskList.length"
          :taskList="filterTask"
          @doneFn="handleDoneFn"
          @deleteFn="handleDeleteFn"
        ></todoContent>
        <todoFoot
          v-show="taskList.length"
          :unCheckedLen="unCheckedLen"
          :nowHash = "nowHash"
          @hashFn="handleHashFn"
        ></todoFoot>
    </section>
  </div>
</template>

<script>
import todoHeader from '@/components/header';
import todoContent from '@/components/content';
import todoFoot from '@/components/footer';

export default {
  name: 'App',
  data(){
    return{
      nowHash : 'all',
      taskList : []
    }
  },
  methods :{
    handleAddFn(value){ //添加
      this.taskList.push({
        id : Date.now() + Math.random(),
        title : value,
        checked : false
      });
    },
    handleDoneFn(value){ //编辑
      this.taskList.filter( (item) => {
        if( item.id === value.id ){
          item = value;
        }
      } );
    },
    handleDeleteFn(value){ //删除
      let posIndex = this.taskList.findIndex( option => option === value );
      this.taskList.splice(posIndex, 1);
    },
    handleHashFn(value){ //hash值筛选数据
      window.location.hash = value;
      this.nowHash = value;
    }
  },
  watch : {
      taskList : {
          handler(){
              localStorage.setItem('todo-component', JSON.stringify(this.taskList) );
          },
          deep : true
      }
  },
  computed :{
    unCheckedLen(){
      return this.taskList.filter( (item) =>{
        return !item.checked
      } ).length;
    },
    filterTask(){
      switch(this.nowHash){
        case 'all' :
          return this.taskList;
        break;
        case 'active' :
          return this.taskList.filter( item => !item.checked );
        break;
        case 'completed' :
          return this.taskList.filter( item => item.checked );
        break;
      }
    }
  },
  components :{
    todoHeader,
    todoContent,
    todoFoot
  },
  created(){
    this.taskList = JSON.parse( localStorage.getItem('todo-component') ) || [];
    let hash = window.location.hash;
    if ( hash ) {
      this.nowHash = hash.replace(/#/, '');
    }
  },
  watch: {
    nowHash: function(newHash) {
      if (!'allactivecompleted'.includes(newHash)) {
        this.nowHash = 'all';
        window.location.hash = this.nowHash;
      }
    }
  }
}
</script>

<style>
#app {
  height:100%;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
