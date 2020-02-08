<template>
  <div class="hello">
    
    <input type="text" ref="todo" v-model="newTodo" class="newTodo" placeholder="New ..." @keyup.enter="Todo"/>

    <ul>
      <h3>Todos:</h3>
      <p v-if="!todos.length" :style="{'color':'#888'}">empty! no todos exists ...</p>
      <li v-for="(todo, index) of todos" :key="index">
        {{(index+1) + '. ' + todo.name}} 
        <button class="remove" @click="remove(index)">Remove</button> 
        <button class="edit" @click="edit(index)">Edit</button> 
        <button class="done" @click="done(index, todo.name)">Done</button> 
      </li>
    </ul>

    <br><br>

    <ul class="doneLis">
      <h3>
        Dones:
        <button v-if="dones.length" class="removeDones" @click="removeDones">Remove All</button>
      </h3>
      <li v-for="(done, index) in dones" :key="index">
        {{'✓ ' + done.name}}
        <button class="remove" @click="removeDone(index)">Remove</button> 
        <button class="edit" @click="returnDone(index, done.name)">Return</button> 
      </li>
    </ul>

  </div>
</template>

<script>
export default {
  name: "todo",
  data: function() {
    return{
      newTodo: null,
      todos: [],
      new: true,
      index: null,
      dones: [],
    }
  },
  methods: {
    Todo() {

      if( this.new ){
        
        this.todos.push({name: this.newTodo})
        this.setItem( 'todos', this.todos )
        this.newTodo = ''
      }else{
        this.todos[this.index].name = this.newTodo
        this.new = true
        this.newTodo = ''
        this.setItem( 'todos', this.todos )
      }

    },

    edit(index){
      this.new = false
      this.newTodo = this.todos[index].name
      this.index = index
      this.$refs.todo.focus()
    },

    focus(){
      this.$refs.todo.focus()
    },

    getTodosLocal(){
      return JSON.parse(localStorage.getItem("todos"))
    },

    getDonesLocal(){
      return JSON.parse(localStorage.getItem("dones"))
    },

    remove(index){
      this.todos.splice(index,1)
      this.setItem( 'todos', this.todos )
    },

    removeDone(index){
      this.dones.splice(index,1)
      this.setItem( 'dones', this.dones )
    },

    returnDone(index, name){

      this.dones.splice(index,1)
      this.setItem( 'dones', this.dones )

      this.todos.push({name: name})
      this.setItem( 'todos', this.todos )

    },

    done(index, namea){
      this.todos.splice(index,1);
      this.dones.push({name: namea})
      this.setItem( 'todos', this.todos )
      this.setItem( 'dones', this.dones )
    },

    removeDones() {
      this.dones = []
      this.setItem( 'dones', [] )
    },

    setItem( name, value ){
      localStorage.setItem(name, JSON.stringify(value) )
    }
  },
  created(){

    //get todos form localstorage id exists
    if ( this.getTodosLocal() !== null ) {
      this.todos = this.getTodosLocal()
    }else{
      this.setItem( 'todos', [] )
    }

    //dones localstorage when load
    if (localStorage.getItem("dones") !== null) {
      this.dones = JSON.parse(localStorage.getItem("dones"))
    }else{
      localStorage.setItem('dones', JSON.stringify([]));
      this.setItem( 'dones', [] )
    }
    
  },
  mounted(){
    this.focus()
  }
};
</script>

<style scoped lang="scss">
* {
  box-sizing: border-box;
}
.newTodo {
    width: 100%;
    height: 55px;
    border: 2px solid #ddd;
    border-radius: 3px;
    font-size: 31px;
    color: #555;
    padding-left: 10px;
    box-shadow: 0 3px 27px #eee inset;
}
.newTodo::placeholder {
  color: #ddd;
}
ul {
  padding-left: 0;
  list-style: none;
  width: 100%;
}
li {
    font-size: 20px;
    padding: 10px;
    background: #f5f5f5;
    border-radius: 3px;
    margin-bottom: 6px;
    border: 1px solid #e5e5e5;
    color: #555;
}
.doneLis{
  li{
    background: #DCEDC8;
    border: none;
  }
}
.remove {
    background: #F8BBD0;
    color: #000;
    border: none;
    padding: 5px 10px;
    float: right;
    border-radius: 3px;
    cursor: pointer;
    font-weight: bold;
    transition: .2s;
    &:hover{
      background: #F48FB1;
      transition: .2s;
    }
}
.edit {
    background: #B2EBF2;
    color: #000;
    border: none;
    padding: 5px 10px;
    float: right;
    border-radius: 3px;
    cursor: pointer;
    font-weight: bold;
    margin-right: 5px;
    transition: .2s;
    &:hover{
      background: #80DEEA;
      transition: .2s;
    }
}

.done{
  background: #DCEDC8;
  color: #000;
  border: none;
  padding: 5px 10px;
  float: right;
  border-radius: 3px;
  cursor: pointer;
  font-weight: bold;
  margin-right: 5px;
  transition: .2s;
  &:hover{
    background: #AED581;
    transition: .2s;
  }
}
.removeDones{
    float: right;
    background: #F8BBD0;
    font-weight: bold;
    border: none;
    padding: 8px;
    border-radius: 3px;
    cursor: pointer;
}
</style>
