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

  <add-task v-model="showAddTask" @todo-list="captureTodoList" />
</template>

<script>
  import AddTask from '@/components/AddTask.vue';

  export default {
    components: {
      AddTask,
    },
    data() {
      return {
        showAddTask: false,
        todoList: [],
      }
    },
    methods: {
      captureTodoList(todoList){
        this.todoList = todoList;
        console.log(this.todoList);
      },
      deleteRow(todo){
        
        const index = this.todoList.indexOf(todo);
        this.todoList.splice(index, 1);
      }
    },
  }
</script>
