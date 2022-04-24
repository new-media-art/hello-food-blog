<template>
  <div>
    <h3 class="display-5" v-if="$store.state.user"></h3>

    <h1 class="display-4">Hello, {{ $store.state.user.firstName }}!</h1>

    <h3>Here are your recipes!</h3>
    <h6>Your taskListId is: {{ $store.state.user.taskLists[0].taskListId }}</h6>

    <br />

    <!--start-->
    <div v-if="popUp">
      <div class="container">
        <div class="row">
          <div class="col">
            <div class="card">
              <div class="card-header text-center bg-vue"></div>
              <div class="card-body">
                <input
                  type="text"
                  class="form-control"
                  placeholder="New recipe titel"
                  v-model="title"
                />
                <br />
                <input
                  type="text"
                  class="form-control"
                  placeholder="New recipe"
                  v-model="description"
                />

                <hr />
                <button type="button" class="btn btn-dark" @click="editTask(taskToEdit.taskId)">
                  Eintragen
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <br />

    <!--end End -->

    <div class="row row-cols-1 row-cols-md-4 g-4">
      <div v-for="task in this.$store.state.tasks" :key="task.taskId">
        <div class="col">
          <div id="border" class="card">
            <img src="https://picsum.photos/400" class="card-img" />

            <div class="card-body">
              <h4 class="card-title">
                <strong>{{ task.title }}</strong>
              </h4>
              <p class="card-text h6 text-secondary">
                Task id:{{ task.taskId }}
              </p>
              <p class="card-text h5">{{ task.description }}</p>
            </div>

            <div class="card-footer">
              <div>
                <i
                  class="fas fa-edit"
                  style="cursor: pointer"
                  @click="popup(task)"
                ></i>
                <i
                  class="fas fa-heart"
                  style="cursor: pointer"
                  @click="likeTask(task.taskId)"
                  >{{ task.points }}</i
                >

                <i
                  class="fa fa-trash"
                  style="cursor: pointer"
                  @click="deleteTask(task.taskId)"
                ></i>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "TaskListOverview",
  //new
  data() {
    return {
      points: 1,
      title: "",
      description: "",
      popUp: false,
      taskToEdit: null,
    };
  },
  methods: {
    popup(task){
      this.popUp = true
      this.taskToEdit=task
      this.title=task.title
      this.description = task.description
    },
    async deleteTask(x) {
      await axios
        .delete(`https://codersbay.a-scho-wurscht.at/api/task/${x}`)
        .then((response) => {
          console.log(response);
        }),
        this.$store.dispatch("loadAllTasks");
    },
    async likeTask(x) {
      await axios
        .put(`https://codersbay.a-scho-wurscht.at/api/task/${x}`, {
          spentTime: 0,
          points: this.points++,
        })
        .then((response) => {
          console.log(response);
        }),
        this.$store.dispatch("loadAllTasks");
    },
    async editTask(x) {

      await axios
        .put(`https://codersbay.a-scho-wurscht.at/api/task/${x}`, {
          
          spentTime: 0,
          
          title: this.title,
          description: this.description,
          
        })
        
        .then((response) => {
          console.log(response);
        }),
        this.popUp = false
        this.$store.dispatch("loadAllTasks");
    },
  },
};
</script>

<style>
i {
  margin-right: 15px;
  margin-left: 15px;
}
</style>
