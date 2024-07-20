<template>
    
  <div align="center" class="tasks w-50 p-4">
    
    <form @submit.prevent="addtask" style="text-align: start;">
      
      <div class="input-group mb-3">
        <span class="input-group-text material-symbols-outlined fs-4" >add</span>
        <input type="text" class="form-control" name="task" placeholder="New Task Title or description" v-model="task" required>
        <button class="btn btn-primary" type="submit " >Submit</button>
      </div>

      <div class="form-check form-switch">
      <input class="form-check-input" type="checkbox" role="switch" v-model="done" id="done">
      <label class="form-check-label" for="done" style="color: white">Mark as done</label>
    </div>
      
    </form>

    <ul>
  <li v-for="(task, index) in tasks" :key="index">
    <div class="todo px-3 py-3 d-flex justify-content-between">
      <div>
        <p class="fs-5 mb-2"><strong>{{ task.title }} </strong></p>
        <small style="color: #2B1887; font-weight: 600" class="m-0">{{ task.created_at }}</small>
      </div>
      <div>
        <span @click="Status(index)" :title="title(task.done)" class="material-symbols-outlined icon fs-2" :class="{'done': task.done, 'notdone': !task.done}">
          {{ iconeStatus(task.done) }}
        </span>

        <!-- Trigger Modal -->
        <span class="material-symbols-outlined fs-2 icon" :data-bs-target="'#modal-' + index" data-bs-toggle="modal" style="color: #FF0000;">delete</span>

        <span class="material-symbols-outlined fs-2 icon" @click="toupdate(index)" style="color: #F8E931;">update</span>
      </div>
    </div>

    <!-- Modal -->
    <div class="modal fade" :id="'modal-' + index" tabindex="-1" :aria-labelledby="'exampleModalLabel-' + index" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" :id="'exampleModalLabel-' + index">Delete Task</h1>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            Are you sure??
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary" @click="deleteTask(index)" data-bs-dismiss="modal">Yes</button>
          </div>
        </div>
      </div>
    </div>
  </li>
</ul>


    
    <div v-show="update" align="center" class="input-group mb-3 " >
      <input type="text" class="form-control" name="updated"  v-model="tasktoupdate.title">
      <span class="input-group-text material-symbols-outlined fs-4 icon" @click="update=false" >close</span>
    </div>





  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      task:'',
      index:null,
      update:false,
      tasktoupdate:{},
      done:false,
      tasks: JSON.parse(localStorage.getItem('tasks'))

    }
  },

  methods: {
    
    Status(index) {
      this.tasks[index].done = !this.tasks[index].done;
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },

    addtask() {
      let currentdate = new Date();
      let created_at = currentdate.getFullYear() + " / " + (currentdate.getMonth() + 1) + " / " + currentdate.getDate();
      this.tasks.push({ title: this.task, created_at: created_at, done: this.done, done_at: "" });
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
      this.task = '';
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },

    toupdate(index){
      this.update=true
      this.tasktoupdate=this.tasks[index]
    }
  },

  computed: {
    iconeStatus() {
      return (done) => {
        return done ? "done_all" : "watch_check";
      }
    },
    
    title() {
      return (done) => {
        return done ? "mark as not done" : "mark as done";
      }
    }
  }
}
</script>

<style scoped>
  ul li {
    list-style: none;
    margin-bottom: 4%;
  }

  .tasks {
    background-color: #2B1887;
    border-radius: 10px;
  }

  .todo {
    background-color: #F4F2FF;
    max-width: 500px;
    text-align: start;
    border-radius: 10px;
  }

  .icon {
    cursor: pointer;
  }

  .done {
    color: green;
  }

  .notdone {
    color: red;
  }
</style>
