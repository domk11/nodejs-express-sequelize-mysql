<template>
  <div class="submit-form">
    <div v-if="!submitted">
      <div class="form-group">
        <label for="title">Title</label>
        <input
          type="text"
          class="form-control"
          id="title"
          required
          v-model="article.title"
          name="title"
        />
      </div>

      <div class="form-group">
        <label for="author">author</label>
        <input
          class="form-control"
          id="author"
          required
          v-model="article.author"
          name="author"
        />
      </div>

      <div class="form-group">
        <label for="content">Content</label>
        <input
                class="form-control"
                id="content"
                required
                v-model="article.content"
                name="content"
        />
      </div>

      <button @click="saveArticle" class="btn btn-success">Submit</button>
    </div>

    <div v-else>
      <h4>You submitted successfully!</h4>
      <button class="btn btn-success" @click="newArticle">Add</button>
    </div>
  </div>
</template>

<script>
import ArticleDataService from "../services/ArticleDataService";

export default {
  name: "add-article",
  data() {
    return {
      article: {
        id: null,
        title: "",
        author:"",
        content: "",
        published: false
      },
      submitted: false
    };
  },
  methods: {
    saveArticle() {
      var data = {
        title: this.article.title,
        author: this.article.author,
        content: this.article.content
      };

      ArticleDataService.create(data)
        .then(response => {
          this.article.id = response.data.id;
          console.log(response.data);
          this.submitted = true;
        })
        .catch(e => {
          console.log(e);
        });
    },
    
    newArticle() {
      this.submitted = false;
      this.article = {};
    }
  }
};
</script>

<style>
.submit-form {
  max-width: 300px;
  margin: auto;
}
</style>
