<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-accent">
      <q-input
        v-model="newTask"
        @keyup.enter="addTask"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Add task"
        dense
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        clickable
        :class="{ 'done bg-blue-1': task.done }"
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="accent"
          />
        </q-item-section>

        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>

        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>

    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="accent" />
      <div class="text-h5 text-accent text-center">
        No tasks
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [
        // Test Data
        // {
        //   title: 'Dev App',
        //   done: false
        // },
        // {
        //   title: 'Debug App',
        //   done: false
        // },
        // {
        //   title: 'Deploy App',
        //   done: false
        // }
      ]
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Really delete?",
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          const deletedTask = this.tasks[index].title;
          this.tasks.splice(index, 1);
          this.$q.notify("Task: " + "'" + deletedTask + "'" + " Deleted");
        });
    },
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false
      });
      this.newTask = "";
    }
  }
};
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #aaa;
  }
}
.no-tasks {
  opacity: 0.5;
}
</style>
