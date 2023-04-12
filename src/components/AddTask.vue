<template>
    <v-dialog
          v-model="show"
          width="auto"
      >
        <v-card
        >
          <div class="d-block pa-2 bg-primary">
            <v-card-title>
              <v-icon icon="mdi-plus-circle"/>
              Add Task
            </v-card-title>
          </div>

          <v-card-text>
              <v-sheet width="300" class="mx-auto">
              <v-form @submit.prevent="submit">
                  <v-text-field
                      v-model="title"
                      :rules="[title => {
                        if (!title) {
                            return 'Title is required';
                        }
                        if (titles.has(title)) {
                            return 'Duplicate title';
                        }
                        return true;
                    }]"
                      label="Title"
                      required
                  ></v-text-field>

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
                      prepend-icon="mdi-plus"
                      color="blue"
                      type="submit"
                    >
                      <template v-slot:prepend>
                        <v-icon></v-icon>
                      </template>
                      Add
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
      value : Boolean
    },
    computed: {
      show: {
        get() {
          return this.value
        },
        set(value){
          this.$emit('input', value);
        }
      }
    },
    data() {
      return {
        snackbar: {
          text: '',
          color: '',
        },
        showAddTask: false,
        column: null,
        inline: null,
        title: '',
        description: '',
        date: '',
        priority: '',
        // todoList: [],
        todoList: [{
          title: "abc",
          description: "abc",
          date: new Date("01/05/2023")
        }],
        titles: new Set()
      }
    },
    methods: {
      submit() {
        if (this.title && !this.titles.has(this.title) && this.description && this.date && this.priority){
          const newTodo = {
            title: this.title,
            description: this.description,
            date: this.date,
            priority: this.priority
          }
          this.todoList.push(newTodo)
          this.titles.add(this.title)

          this.title = '';
          this.description = '';
          this.date = '';
          this.priority = '';

          this.closeDialog();
          this.snackbar.text = "Task was added successfully";
          this.snackbar.color = "success"
          this.$emit("snackbar", this.snackbar);
        }
      },
      closeDialog(){
        console.log(this.show);
        this.$emit("show-add-task", this.show);
      },
      sendToDoList(){
        this.$emit("todo-list", this.todoList);
      }
    },
    mounted(){
      this.sendToDoList();
    }
  }

</script>

