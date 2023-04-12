<template>
    <v-dialog
          v-model="show"
          width="auto"
      >
        <v-card
        >
          <div class="d-block pa-2 bg-primary">
            <v-card-title>
              <v-icon icon="mdi-file-edit-outline"/>
              Edit Task
            </v-card-title>
          </div>

          <v-card-text>
              <v-sheet width="300" class="mx-auto">
              <v-form @submit.prevent="submit">
                  <v-text-field
                      v-model="description"
                      :rules="[v => !!v || 'Description is required']"
                      label="Description"
                      required
                  ></v-text-field>

                  <v-text-field
                    v-model="date"
                    type="date"
                    :rules="[date => {
                        if (!date) {
                            return 'Date is required';
                        }
                        if (isNaN(Date.parse(date))) {
                            return 'Please enter a valid date';
                        }
                        return true;
                    }]"
                    label="Deadline"
                    required
                ></v-text-field>
                  <v-radio-group
                  v-model="priority"
                  :rules="[v => !!v || 'Priority is required']"
                  label="Priority"
                  inline
                  required
                  >
                  <v-radio
                      label="Low"
                      value="low"
                  ></v-radio>
                  <v-radio
                      label="Med"
                      value="med"
                  ></v-radio>
                  <v-radio
                      label="High"
                      value="high"
                  ></v-radio>
                  </v-radio-group>

                  <div class="d-flex flex-row-reverse mb-6">
                    <v-btn
                      class="ma-1 pa-2" 
                      prepend-icon="mdi-cancel"
                      color="red"
                      @click="closeDialog"
                    >
                      <template v-slot:prepend>
                        <v-icon></v-icon>
                      </template>
                      Cancel
                    </v-btn>

                    <v-btn
                      class="ma-1 pa-2" 
                      prepend-icon="mdi-file-edit-outline"
                      color="blue"
                      type="submit"
                    >
                      <template v-slot:prepend>
                        <v-icon></v-icon>
                      </template>
                      Edit
                    </v-btn>
                  </div>
            </v-form>
          </v-sheet>
          </v-card-text>
      </v-card>
    </v-dialog>
</template>

<script>

  export default {
    props :{
      value : Boolean,
      todo: {
            type: Object,
            default: () => ({})
        }
    },
    computed: {
      show: {
        get() {
          return this.value
        },
        set(value){
          this.$emit('input', value);
        }
      },
    },
    data() {
      return {
        showEditTask: false,
        column: null,
        inline: null,
        description: this.todo.description,
        date: this.todo.date,
        priority: this.todo.priority,
        updatedTodo: {
            description: '',
            date: '', 
            priority: ''
        },
        snackbar: {
          text: '',
          color: '',
        },
      }
    },
    methods: {
      submit() {
        if (this.description && this.date && this.priority){
          const editedTodo = {
            description: this.description,
            date: this.date,
            priority: this.priority
          }
          this.updatedTodo = editedTodo;

          this.description = '';
          this.date = '';
          this.priority = '';

          this.closeDialog();

          this.$emit("update-todo", this.updatedTodo)

          this.snackbar.text = "Task was updated successfully";
          this.snackbar.color = "success"
          this.$emit("snackbar", this.snackbar);
        }
        
      },
      closeDialog(){
        this.$emit("show-edit-task", this.showEditTask);
        
      },
      sendToDoList(){
        this.$emit("update-todo-list", this.todoList);
      },
    },
    mounted(){
      this.sendToDoList();
    }
  }

</script>