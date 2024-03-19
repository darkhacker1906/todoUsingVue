<template>
  <div class="table_div">
    <table id="todo-table">
      <thead>
        <tr>
          <th class="header-cell detail">Details</th>
          <th class="header-cell">Actions</th> 
        </tr>
      </thead>
      <tbody>
        <tr v-for="(todo, index) in data" :key="index">
          <td class="text_show">
            <div v-if="todo.editing" class="map_text">
              <input type="text" v-model="todo.editedText" @keyup.enter="saveEdit(todo)" class="edit_text" />
            </div>
            <div v-else class="map_text">
              <span><input type="checkbox" class="check" v-model="todo.completed" @change="updateLocalStorage" /></span>
              <span>{{ todo.text }}</span> 
            </div>
          </td>
          <td class="checkedd">
            <div>
              <button v-if="todo.editing" @click="saveEdit(todo)" class="open">Save</button>
              <!-- <button v-else @click="toggleEdit(todo)" class="open"> -->
                <i class="fas fa-edit icon" v-else @click="toggleEdit(todo)"></i>
                <!-- </button> -->
              <!-- <button @click="deleteRow(index)" class="open"><i class="fas fa-trash-alt icon"></i></button> -->
              <button v-if="todo.editing" @click="cancel(todo)">Cancel</button>
              <i @click="deleteRow(index)" v-else class="fas fa-trash-alt icon"></i>
            </div>
            <div>
              <span v-if="todo.completed" class="complete">Completed</span>
              <span class="hid" v-else>Incomplete</span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "TodoTable",
  props: {
    data: {
      type: Array,
      required: true
    }
  },
  methods: {
    toggleEdit(todo) {
      todo.editing = !todo.editing;
      if (todo.editing) {
        todo.originalText = todo.text;
        todo.editedText = todo.text; 
      }
      this.updateLocalStorage();
    },

    saveEdit(todo) {
      if (todo.editedText.trim() !== "") {
        todo.editing = false;
        todo.text = todo.editedText;
      } else {
        todo.editing = false;
        todo.editedText = todo.originalText;
      }
      this.updateLocalStorage();
    },

    cancel(todo){
      todo.editing=false;
    },

    deleteRow(index) {
      this.$emit("delete", index);
    },

    updateLocalStorage() {
      localStorage.setItem('todos', JSON.stringify(this.data));
    }
  }
};
</script>

<style>
.table_div{
  height: 300px;
  overflow-y: auto;

  width: 90%;
  margin: auto;
  background: white;
  padding: 20px;
  border: transparent;
      border-radius: 13px;
}
.table_div::-webkit-scrollbar {
  width: 10px;
}
#todo-table {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
  margin: auto;
}

#todo-table td, #todo-table th {
  border: 1px solid #ddd;
  padding: 8px;
}

#todo-table tr:nth-child(even) {
     /* background:#e7dcf4; */
     background: #eae4d9;
}

#todo-table tr:hover {
  /* background-color: #ddd; */
 /* background:#dcc5f8;*/
}

.header-cell {
  text-align: center;
}
.detail{
  text-align: left;
  padding-left: 40px;
}

#todo-table th {
  padding-top: 12px;
  padding-bottom: 12px;
  width: 60%;

  background: #ab8a4e;
  color: white;
}
button{
  margin-right: 5px;
  font-size: 16px;
  font-weight: bold;
}
.map_text{
  width: 100%;
  display: flex;
  justify-content: flex-start;
  gap: 10px;
}
.check{
  accent-color:#ddd;
}
.text_show{
  word-break: break-all;
}
.open{
  /* background: #AF7EEB; */
  border: none;
  padding: 7px;
  /* color: #ffffff; */
}

.fa-trash-alt{
  color: red;
  background: #e2cff9;
}
.checkedd{
  display: flex;
  justify-content: center;
  gap: 20px;
}
.hid{
  visibility: hidden;
}
i{
  margin-right: 10px;
}
.edit_text{
  height: 24px;
    width: 70%;
    padding:0px 10px;
}
.complete{
  font-size: 13px;
}
</style>