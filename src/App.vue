<template>
  <div id="app">
    <div class="container my-5">
      <div class="row justify-content-md-center">
        <div class="col-md-8 bg-white p-2 dabba">
          <div class="row"></div>
          <div class="row justify-content-md-center py-2">
            <div class="col-md-3">
              <button class="btn btn-outline-primary" @click="addNewToDo">
                Add New
              </button>
            </div>
            <div class="col-md-9">
              <div class="form-group">
                <input v-model="newItemText" class="form-control" />
              </div>
            </div>
          </div>
          <div class="row">
            <div class="col-md-6">{{ name }}'s ToDo List</div>
            <div class="col-md-6">
              <input
                type="checkbox"
                v-model="hide"
                class="mr-2"
                id="showTodo"
              />
              <label for="showTodo">Hide Completed</label>
            </div>
          </div>
          <hr />
          <div class="row px-3 py-2" v-for="l in hiddenList" :key="l.id">
            <div class="col-md-9">
              <div class="form-group">
                {{ l.task }}
              </div>
            </div>
            <div class="col-md-3">
              <div class="row">
                <div class="form-group float-right">
                  <label class="form-check-label">
                    <input type="checkbox" v-model="l.comp" class="mr-2" />
                  </label>
                  <button
                    class="btn btn-outline-danger btn-sm"
                    v-on:click="deleteToDo"
                  >
                    Delete
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      name: "Aniket",
      list: [],
      hide: true,
      newItemText: "",
    };
  },
  computed: {
    hiddenList() {
      return this.hide ? this.list.filter((l) => !l.comp) : this.list;
    },
  },
  methods: {
    addNewToDo() {
      this.list.push({
        id: this.list.length + 1,
        task: this.newItemText,
        comp: false,
      });
      sessionStorage.setItem("listItems", JSON.stringify(this.list));
      this.newItemText = "";
    },
    deleteToDo() {
      this.list = this.list.filter((l) => !l.comp);
      sessionStorage.setItem("listItems", JSON.stringify(this.list));
    },
  },
  created() {
    let data = sessionStorage.getItem("listItems");
    if (data != null) {
      this.list = JSON.parse(data);
    }
  },
  destroyed() {
    sessionStorage.clear();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Cabin:ital@0;1&display=swap");
body {
  background-color: #f8f8f8 !important;
}
.dabba {
  outline: 1px solid pink;
  padding: 4px;
}
#app {
  font-family: "Cabin", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  font-size: 1.5em;
}
</style>
