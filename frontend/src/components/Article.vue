<template>
  <div v-if="currentArticle" class="edit-form">
    <h4>Article</h4>
    <form>
      <div class="form-group">
        <label for="title">Title</label>
        <input type="text" class="form-control" id="title"
          v-model="currentArticle.title"
        />
      </div>
      <div class="form-group">
        <label for="content">content</label>
        <input type="text" class="form-control" id="content"
          v-model="currentArticle.content"
        />
      </div>

      <div class="form-group">
        <label for="author">author</label>
        <input type="text" class="form-control" id="author"
               v-model="currentArticle.author"
        />
      </div>

      <div class="form-group">
        <label><strong>Status:</strong></label>
        {{ currentArticle.published ? "Published" : "Pending" }}
      </div>
    </form>

    <button class="badge badge-primary mr-2"
      v-if="currentArticle.published"
      @click="updatePublished(false)"
    >
      UnPublish
    </button>
    <button v-else class="badge badge-primary mr-2"
      @click="updatePublished(true)"
    >
      Publish
    </button>

    <button class="badge badge-danger mr-2"
      @click="deleteArticle"
    >
      Delete
    </button>

    <button type="submit" class="badge badge-success"
      @click="updateArticle"
    >
      Update
    </button>
    <p>{{ message }}</p>
  </div>

  <div v-else>
    <br />
    <p>Please click on a Article...</p>
  </div>
</template>

<script>
import ArticleDataService from "../services/ArticleDataService";

export default {
  name: "Article",
  data() {
    return {
      currentArticle: null,
      message: ''
    };
  },
  methods: {
    getArticle(id) {
      ArticleDataService.get(id)
        .then(response => {
          this.currentArticle = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },

    updatePublished(status) {
      var data = {
        id: this.currentArticle.id,
        title: this.currentArticle.title,
        author: this.currentArticle.author,
        content: this.currentArticle.content,
        published: status
      };

      ArticleDataService.update(this.currentArticle.id, data)
        .then(response => {
          this.currentArticle.published = status;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },

    updateArticle() {
      ArticleDataService.update(this.currentArticle.id, this.currentArticle)
        .then(response => {
          console.log(response.data);
          this.message = 'The Article was updated successfully!';
        })
        .catch(e => {
          console.log(e);
        });
    },

    deleteArticle() {
      ArticleDataService.delete(this.currentArticle.id)
        .then(response => {
          console.log(response.data);
          this.$router.push({ name: "Articles" });
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  mounted() {
    this.message = '';
    this.getArticle(this.$route.params.id);
  }
};
</script>

<style>
.edit-form {
  max-width: 300px;
  margin: auto;
}
</style>
