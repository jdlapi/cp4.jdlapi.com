<template>
  <div class="admin">
    <h1>Justin's control center:</h1>
      <div class="forms">
        <div class="add">
          <div class="heading">
            <h2>Add Film Review:</h2>
          </div>
          <div class="form">
            <input v-model="title" placeholder="Title">
            <p></p>
            <textarea v-model="description" placeholder="Review" rows="5" cols="50" ></textarea>
            <p></p>
            <input type="file" name="photo" @change="fileChanged">
            <button @click="upload">Upload</button>
          </div>
          <div class="itemAdded" v-if="addItem">
            <h2>Added review for {{addItem.title}}</h2>
          </div>
    </div>
      <div class="edit">
      <div class="heading">
        <h2>Edit Film Review:</h2>
      </div>
        <div class="form">
            <input v-model="findTitle" placeholder="Find Film">
          <div class="suggestions" v-if="suggestions.length > 0">
            <div class="suggestion" v-for="s in suggestions" :key="s.id" @click="selectItem(s)">{{s.title}}
            </div>
          </div>
          <p></p>
        </div>
        <div class="upload" v-if="findItem">
          <div class = "action-container">
            <input v-model="findItem.title">
            <div class="actions" v-if="findItem">
              <button @click="deleteItem(findItem)">Delete</button>
              <button @click="editItem(findItem)">Edit</button>
            </div>
          </div>
          <p></p>
          <textarea v-model="findItem.description" rows="5" cols="50" ></textarea>
          <p></p>
        </div>
      </div>
    </div>
</div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Admin',
  data() {
    return {
      title: "",
      description: "",
      file: null,
      addItem: null,
      items: [],
      findTitle: "",
      findItem: null,
    }
  },
  computed: {
    suggestions() {
      let items = this.items.filter(item => item.title.toLowerCase().startsWith(this.findTitle.toLowerCase()));
      return items.sort((a, b) => a.title > b.title);
    }
  },
  created() {
   this.getItems();
   },
  methods: {
    fileChanged(event) {
      this.file = event.target.files[0]
    },
    async upload() {
      try {
        const formData = new FormData();
        formData.append('photo', this.file, this.file.name)
        let r1 = await axios.post('/api/photos', formData);
        let r2 = await axios.post('/api/items', {
          title: this.title,
          description: this.description,
          path: r1.data.path
        });
        this.addItem = r2.data;
      } catch (error) {
        console.log(error);
      }
    },
    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;
        return true;
      } catch (error) {
        console.log(error);
      }
    },
    async deleteItem(item) {
      try {
        await axios.delete("/api/items/" + item._id);
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },
    selectItem(item) {
      this.findTitle = "";
      this.findItem = item;
    },
    async editItem(item) {
      try {
        await axios.put("/api/items/" + item._id, {
          title: this.findItem.title,
          description: this.findItem.description,
        });
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },
  }
}
</script>

<style scoped>
.image h2 {
  font-style: italic;
  font-size: 1em;
}

.forms {
  display: flex;
  flex-wrap: wrap;
}

.heading {
  display: flex;
  margin-bottom: 20px;
  margin-top: 20px;
}

.heading h2 {
  margin-top: 8px;
  margin-left: 10px;
}

.circle {
  border-radius: 50%;
  width: 18px;
  height: 18px;
  padding: 8px;
  background: #333;
  color: #fff;
  text-align: center
}

/* Form */
input,
textarea,
select,
button {
  font-family: 'Montserrat', sans-serif;
  font-size: 1em;
}

.form {
  margin-right: 50px;
}

/* Uploaded images */
.upload h2 {
  margin: 5px;
}

.upload h3 {
  font-size: .8em;
  margin: 0px;
  margin-left: 5px;
  width: 400px;
  font-style: italic;
  border-left: solid;
  padding-left: 3px;
}

.upload img {
  max-width: 300px;
}

.action-container {
  display: flex;
}

/* Suggestions */
.suggestions {
  width: 200px;
  border: 1px solid #ccc;
}

.suggestion {
  min-height: 20px;
}

.suggestion:hover {
  background-color: #5BDEFF;
  color: #fff;
}
</style>
