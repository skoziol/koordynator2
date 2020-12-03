<template>
    <div class="tasks-section">
      <div class="headline">
        <h3>Lista podzadań</h3>
        <button v-if="taskList.length> 0 && showCompleteElements" @click="showCompleteElements = false">ukryj zakończone zadania</button>
        <button v-if="taskList.length> 0 && !showCompleteElements" @click="showCompleteElements = true">pokaż zakończone zadania</button>
      </div>
      <div class="progress">
        <span v-if="taskList.length > 0">{{(completedTasks/taskList.length * 100).toFixed(0)}}%</span>
        <progress :max="taskList.length" :value="completedTasks" v-if="taskList.length > 0"></progress>
      </div>
      <div class="scroll">
        <div class="task" v-for="task in taskList" :key="task" :class="{hide: !showCompleteElements && task.status}">
          <input ref="taskCheckbox" type="checkbox" v-model="task.status" @change="taskCompeted(task.status)">
          <span :class="{'line-through': task.status}" v-if="!task.editMode" @click="editTask(task.id)">{{task.name}}</span>
          <input type="text" v-model="mordor" class="mordor" @blur="cancelTimeoutEdit"  v-else>
          <button v-if="!task.editMode" @click="attachExpert">Odeślij do experta</button>
          <button v-if="task.editMode" @click="saveEdit(mordor !== task.name)">zapisz</button>
          <button v-if="task.editMode" @click="cancelEdit()">anuluj</button>
          <button v-if="task.editMode" @click="deleteThis()">delete</button>
        </div>
      </div>
      <div class="addTask" :class="{isFocused: newTask, notFocused: !newTask}">
        <input type="text" placeholder="Dodaj zadanie" v-model="newTaskName" @focus="newTask = true" v-show="!newTask">
        <textarea v-show="newTask" @show="focus" rows="4" v-model="newTaskName"></textarea>
        <div class="row">
          <button @click="addTask()" :class="{active: newTaskName.length > 0}" v-show="newTask">Dodaj</button>
          <button @click="newTask = false; newTaskName='';" v-show="newTask">Anuluj</button>
        </div>
      </div>
  </div>
</template>

<script>
export default {
  name: 'tasks',
   data () {
    return {
      taskId: 4,
      completedTasks: 0,
      newTaskName:'',
      taskList: [
          {name: 'Akceptacja dokumentacji HLD',
          status: false,
          editMode: false,
          id: 0},
          {name: 'Akceptacja architektury w HLD',
          status: false,
          editMode: false,
          id: 1},
          {name: 'Opiniowanie HLD',
          status: false,
          editMode: false,
          id: 2},
          {name: 'Opiniowanie AW',
          status: false,
          editMode: false,
          id: 3},
      ],
      newTask: false,
      mordor: '',
      showCompleteElements: true
    }
  },
  methods: {
    addTask() {
      if(this.newTaskName.length === 0) return
      this.taskList.push({
          name: this.newTaskName,
          status: false,
          editMode: false,
          id: this.taskId
        })
      this.taskId++
      this.newTaskName = ''
      this.newTask = false;
    },
    taskCompeted(){
      this.completedTasks = 0
      this.taskList.map(task => {
        if(task.status) this.completedTasks ++
      })
    },
    attachExpert(){
      console.log('Attach Expert')
    },
    editTask(id){
      const edditingTask = this.taskList.find(t => t.id === id)
      edditingTask.editMode = true
      this.mordor = edditingTask.name
    },
    cancelEdit(){
      const edditingTask = this.taskList.find(t => t.editMode === true)
      edditingTask.editMode = false
      this.mordor = ''
    },
    cancelTimeoutEdit(){
      setTimeout(()=>this.cancelEdit(), 50)
    },
    saveEdit(isChange){
      if(isChange){
        const edditingTask = this.taskList.find(t => t.editMode === true)
        edditingTask.name = this.mordor
        edditingTask.editMode = false
      }
    },
    deleteThis(){
      const deleteTask = this.taskList.find(t => t.editMode === true)
      this.taskList.splice(this.taskList.indexOf(deleteTask), 1)
    }
  }
}
</script>

<style lang="scss" scoped>
.tasks-section{
  flex-grow:1;
  margin: 0 20px;

  .headline{
    display:flex;
    align-items: center;
    justify-content: center;

    h3{
      flex-grow:1;
    }

    button{
      height: 30px;
      display:flex;
      font-size:16px;
      align-items: center;
      justify-content: center;
    }
  }

  .progress{
    display:flex;
    
    span{
      flex-basis: 50px;
    }
    
    progress{
      height: 20px;
      flex-grow:1;
      transition: all 0.3s ease;
    }
  }

  .scroll{
    max-height: 250px;
    overflow: auto;

  .task{
    display:flex;
    align-items: center;
    justify-content: start;
    font-size: 15px;
    margin: 10px 0;

    &.hide{
      display: none;
    }

    span{
      flex-grow:1;
    }

    .line-through{
      text-decoration: line-through;
    }
    button {
      height: 20px;
      background:  none;
      text-decoration: underline;
    }
  }
  }

  .addTask{
    display:flex;
    flex-direction: column;
    position: relative;
    margin: 10px 0 ;

    .row{
      display:flex;

      button{
        &+button{
          margin-left:10px;
        }
      }
    }

    &.isFocused{
      button{
        align-items: center;
        justify-content: center;
        font-size: 17px;
        font-weight: 500;
        height: 30px;
        width: 60px;
        transition: 0.25s ease;
        margin-top: 10px;
        &.active{
          background: #2196F3;
          color: white;
        }
      }
    } 

    &.notFocused{
      input{
        border: none;
        background: lightgray;
        font-size: 16px;
        color: black;
        height: 30px;
        width: 120px;

        &::placeholder{
          color: black;
          text-align: center;
        }
      }
    }
  } 
}
</style>