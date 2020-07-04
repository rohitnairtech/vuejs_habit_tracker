<template>
  <v-container class="mt-12" style="max-width: 500px">
    <v-text-field
      v-model="task"
      label="What are you working on?"
      solo
      @keydown.enter="create"
    >
    <!-- <template > -->
      <v-fade-transition slot="append">
        <v-icon
          v-if="task"
          @click="create"
        >
          mdi-plus-circle-outline
        </v-icon>
      </v-fade-transition>
    <!-- </template> -->
    </v-text-field>
<!--     <p class="">
      
      <v-fade-transition slot="append">
        <span class="text-caption accent-1 red--text" v-if="error">{{error}}</span>
      </v-fade-transition>


    </p> -->
      <v-fade-transition slot="append">
        <p class="text-caption accent-1 red--text" v-if="error">{{error}}</p>
      </v-fade-transition>

    <h2 class="display-1 success--text pl-4">
      Tasks:&nbsp;
      <v-fade-transition leave-absolute>
        <span :key="`tasks-${tasks.length}`">
          {{ tasks.length }}
        </span>
      </v-fade-transition>
    </h2>

    <v-divider class="mt-4"></v-divider>

    <v-row
      class="my-1"
      align="center"
      dense
    >
      <strong class="mx-4 info--text text--darken-2">
        Remaining: {{ remainingTasks }}
      </strong>

      <v-divider vertical></v-divider>

      <strong class="mx-4 success--text text--darken-2">
        Completed: {{ completedTasks }}
      </strong>

      <v-spacer></v-spacer>

      <v-progress-circular
        :value="progress"
        class="mr-2"
      ></v-progress-circular>
    </v-row>

    <v-divider class="mb-4"></v-divider>

    <v-card v-if="tasks.length > 0" >
      <v-slide-y-transition
        class="py-0"
        group
        tag="v-list"
      >
        <template v-for="(task, i) in tasks">
          <v-divider
            v-if="i !== 0"
            :key="`${i}-divider`"
          ></v-divider>

          <v-list-item :key="`${i}-${task.text}`">
            <v-list-item-action>
              <v-checkbox
                v-model="task.done"
                :color="task.done && 'grey' || 'primary'"
              >
                <template v-slot:label>
                  <div
                    :class="task.done && 'grey--text' || 'primary--text'"
                    class="ml-4"
                    v-text="task.text"
                  ></div>
                </template>
              </v-checkbox>
            </v-list-item-action>

            <v-spacer></v-spacer>

            <v-scroll-x-transition>
              <v-icon
                v-if="task.done"
                color="success"
              >
                mdi-check
              </v-icon>
            </v-scroll-x-transition>
          </v-list-item>
        </template>
      </v-slide-y-transition>
    </v-card>
  </v-container>
</template>

<script>
  export default {
    data: () => ({
      tasks: [
  
      ],
      task: null,
      error: null
    }),

    computed: {
      completedTasks () {
        return this.tasks.filter(task => task.done).length
      },
      progress () {
        if(this.tasks.length){
          return this.completedTasks / this.tasks.length * 100;
        }
        else{
          return 0;
        }
      },
      remainingTasks () {
        return this.tasks.length - this.completedTasks;
      },
    },

    methods: {
      create () {
        const currTask = {
          done: false,
          text: this.task,
        };

        let exists = false;

        for(let i = 0; i < this.tasks.length; i++){
          const {text, done} = this.tasks[i];
          if(text.toLowerCase() === currTask.text.toLowerCase() && done === currTask.done){
            exists = true;
            break;
          }
        }
        if(exists){
          this.error = 'Task already exists';
          setTimeout(()=>{
            this.error = null;
          },2000);
        }
        else{
          this.tasks.push(currTask);
          localStorage.tasks = JSON.stringify(this.tasks);
        }

        this.task = null
      },
    },
    mounted(){
      if(localStorage.tasks){
        this.tasks = JSON.parse(localStorage.tasks);    
      }
    }
  }
</script>











