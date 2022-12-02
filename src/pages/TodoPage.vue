<template>
<q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
        <q-input
          filled 
          class="col"
          @keyup.enter = "addTask"
          square
          bg-color="white"
          bottom-slots 
          v-model="state.newTask" 
          placeholder="Add task" 
          dense>
            <template v-slot:append>
                <q-btn 
                  round 
                  dense 
                  flat 
                  icon="add" 
                  @click="addTask"
                />
            </template>
        </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
        <q-item v-for="(task,index) in state.tasks" :key="task.title" clickable @click="changeState(index)" :class="{'done bg-blue-1':task.done}" v-ripple>
            <q-item-section avatar>
                <q-checkbox v-model="task.done" class="no-pointer-events" color="primary" />
            </q-item-section>
            <q-item-section>
                <q-item-label>{{task.title}}</q-item-label>
            </q-item-section>
            <q-item-section side v-if="task.done">
                <q-btn @click.stop="deleteTask(index)" flat round dense color="primary" icon="delete" />
            </q-item-section>
        </q-item>
    </q-list>
    <div 
      class="no-tasks absolute-center"
      v-if="!state.tasks.length"
      >
      <q-icon 
        name="check"
        size="100px"
        color="primary"
      />
      <div class="text-h5 text-primary text-center">
          No tasks
      </div>
    </div>
</q-page>
</template>

<script>
import {
    defineComponent,
    reactive
} from 'vue'
import {
    useQuasar
} from 'quasar'

export default defineComponent({
    name: 'IndexPage',
    setup(ctx) {
        const $q = useQuasar()
        const state = reactive({
            newTask:'',
            tasks: [{
                    title: 'Get Bananas',
                    done: false
                },
                {
                    title: 'Wash Bananas',
                    done: false
                },
                {
                    title: 'Eat Bananas',
                    done: true
                }
            ],
        })

        function changeState(id) {
            state.tasks[id].done = !state.tasks[id].done;
        }

        function deleteTask(id) {
            $q.dialog({
                title: 'Confirm',
                message: 'Would you like delete this?',
                cancel: true,
                persistent: true
            }).onOk(() => {
                state.tasks.splice(id, 1)
                $q.notify({
                    message: 'Task Deleted Successfully.'
                })
            })
        }

        function addTask(){
          state.tasks.push({title:state.newTask,done:false})
          state.newTask = ''
        }

        return {
            state,
            changeState,
            deleteTask,
            addTask
        }
    }
})
</script>

<style lang="scss">
.done {
    .q-item__label {
        text-decoration: line-through;
        color: #bbb;
    }
}
.no-tasks{
  opacity: 0.5;
}
</style>
