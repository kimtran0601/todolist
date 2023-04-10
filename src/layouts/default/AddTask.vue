<template>
    <v-dialog
          v-model="dialog"
          activator="parent"
          width="auto"
        >
          <v-card>
            <v-card-title class="primary">
                <v-icon icon="mdi-plus"/>
                Add Task
            </v-card-title>

            <v-card-body>
                <v-sheet width="300" class="mx-auto">
                <v-form @submit.prevent>
                    <v-text-field
                        v-model="title"
                        :rules="[v => !!v || 'Title is required']"
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
                        :rules="[v => !!v || 'Date is required']"
                        label="Deadline"
                        required
                    ></v-text-field>

                    <v-radio-group
                    v-model="inline"
                    label="Priority"
                    inline
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

                <v-btn type="submit" block class="mt-2">Add</v-btn>
                </v-form>
            </v-sheet>
            </v-card-body>

            <v-card-actions>
              <v-btn color="error" block @click="dialog = false">Cancel</v-btn>
            </v-card-actions>
          </v-card>
    </v-dialog>
</template>

<script>
  export default {
    data() {
      return Object.assign({
        dialog: false,
        column: null,
        inline: null,
        title: '',
        description: '',
        date: ''
    }, 
      )
    }
  }
</script>

