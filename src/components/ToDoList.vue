<template>
  <v-app-bar color="primary">
    <template v-slot:prepend>
        <v-app-bar-nav-icon></v-app-bar-nav-icon>
    </template>

    <v-app-bar-title align="center">Framework</v-app-bar-title>

    <template v-slot:append>
      <v-btn
      @click.stop="showAddTask=true"
      >
        Add
        <v-icon
          icon="mdi-plus"
          size="large"
          start
        />
      </v-btn>
    </template>
  </v-app-bar>

  <v-container>
    <v-snackbar
      v-model="snackbar"
      :color="color"
      absolute
      right
      top
    >
      {{ text }}
      <template v-slot:actions>
        <v-btn
          variant="text"
          @click="snackbar = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
    <v-table>
      <thead>
        <tr>
          <th class="text-center">
            Title
          </th>
          <th class="text-center">
            Description
          </th>
          <th class="text-center">
            Deadline
          </th>
          <th class="text-center">
            Priority
          </th>
          <th class="text-center">
            Is Complete
          </th>
          <th class="text-center">
            Action
          </th>
        </tr>
      </thead>
      <tbody class="text-center">
        <tr
          v-for="(todo, index) in todoList"
          :key="index"
        >
          <td>{{ todo.title }}</td>
          <td>{{ todo.description }}</td>
          <td>{{ todo.date }}</td>
          <td>{{ todo.priority }}</td>
          <td style="align-self: center; justify-self: center;">
            <div class="text-center">
              <v-checkbox
                v-model="todo.isCompleted"
                :true-value="true"
                :false-value="false"
                @change="deleteRow(todo)"
                align-self="center"
                justify-self="center"
              ></v-checkbox>
            </div>
            
          </td>
          <td>
            <v-col>
              <v-row density="compact" align="start" justify="center">
                <div class="text-center">
                  <v-btn
                    prepend-icon="mdi-file-edit-outline"
                    color="blue"
                    size="small"
                    @click.stop="updateTodoList(todo)"
                  >
                    <template v-slot:prepend>
                      <v-icon></v-icon>
                    </template>
                    Update
                  </v-btn>
                </div>
              </v-row>
              <v-row density="compact" align="start" justify="center">
                <div class="text-center">
                  <v-btn
                    prepend-icon="mdi-alpha-x-box-outline"
                    color="red"
                    size="small"
                    @click.stop="deleteRow(todo)"
                  >
                    <template v-slot:prepend>
                      <v-icon></v-icon>
                    </template>
                    Delete
                  </v-btn>
                </div>
              </v-row>
            </v-col>
          </td>
        </tr>
      </tbody>
    </v-table>
  </v-container>

  <add-task v-model="showAddTask" @show-add-task="captureShowAddTask" @todo-list="captureTodoList" @snackbar="showSnackbar" />
  <edit-task v-model="showEditTask" @show-edit-task="captureShowEditTask" @update-todo="captureEditedTodo"/>
</template>

<script>
  import AddTask from '@/components/AddTask.vue';
  import EditTask from '@/components/EditTask.vue';

  export default {
    components: {
      AddTask,
      EditTask
    },
    data() {
      return {
        snackbar: false,
        text: '',
        color: '',
        showAddTask: false,
        showEditTask: false,
        todoList: [],
        editedTodo: {}
      }
    },
    methods: {
      captureTodoList(todoList){
        this.todoList = todoList;
      },
      captureEditedTodo(updatedTodo){
        this.editedTodo = updatedTodo;
      },
      captureShowAddTask(show){
        this.showAddTask = show;
      },
      captureShowEditTask(show){
        this.showEditTask = show;
      },
      showSnackbar(snackbar){
        this.snackbar = true;
        this.text = snackbar.text;
        this.color = snackbar.color;
        console.log(this.color);
      },
      deleteRow(todo){
        const index = this.todoList.indexOf(todo);
        this.todoList.splice(index, 1);

        this.snackbar = true;
        this.text = "Task Deleted Successful";
        this.color = "success"
      },
      updateTodoList(todo){
        this.showEditTask = true;
        if (this.editedTodo){
          const index = this.todoList.indexOf(todo);
          // this.todoList[index].description = this.editedTodo.description;
          this.todoList[index].description = this.editedTodo.description;
          this.todoList[index].date = this.editedTodo.date;
          this.todoList[index].priority = this.editedTodo.priority;

          // console.log({todolist: this.todoList});

        }
        
        // todo.description = updateTodoList.description
      },
    },
  }
</script>
