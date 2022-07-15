<template>
  <div id="app">
    <CreateComment v-model="commentText" :eventClick="createComment"></CreateComment>

    <h3>Страница: {{ pageNumber }}</h3>
    <UserComment v-for="comment in paginatedData" :key="comment.id" :comment="comment" />

    <div class="btns-navigation mb-3">
      <button
        class="btn btn-outline-primary btn-previous"
        :disabled="pageNumber == 0"
        @click="previousPage"
      >
        Previous
      </button>
      <button
        class="btn btn-outline-primary btn-next"
        :disabled="pageNumber >= pageCount - 1"
        @click="nextPage"
      >
        Next
      </button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import UserComment from './components/UserComment.vue';
import CreateComment from './components/CreateComment.vue';

export default {
  name: 'App',
  components: { UserComment, CreateComment },
  data() {
    return {
      pageNumber: 0,
      size: 20,
      comments: [],
      commentText: '',
    };
  },

  methods: {
    nextPage() {
      this.pageNumber++;
      this.updateParams();
    },

    previousPage() {
      this.pageNumber--;
      this.updateParams();
    },

    toMonthName(monthNumber) {
      const date = new Date();
      date.setMonth(monthNumber - 1);

      return date.toLocaleString('en-US', {
        month: 'long',
      });
    },

    createComment() {
      const date = new Date();

      const countWords = this.commentText.split(' ').length;
      const lengthString = this.commentText.replace(/\s/g, '').length;

      if (countWords >= 3 && lengthString < 1000) {
        this.comments.unshift({
          body: this.commentText,
          created_at: `${date.getDate()} ${this.toMonthName(
            date.getMonth() + 1,
          )} ${date.getFullYear()}`,
          author: {
            id: 16,
            name: 'John Doe',
            avatar: 'http://placeimg.com/640/480/business',
            company: 'BigCountry',
          },
        });
      }
    },

    dataFilter() {
      this.comments.forEach((comment) => {
        const date = new Date(comment.created_at);
        date.toDateString();

        comment.created_at = `${date.getDate()} ${this.toMonthName(
          date.getMonth() + 1,
        )} ${date.getFullYear()}`;
      });
    },

    getQueryVariable(variable) {
      const query = window.location.search.substring(1);
      const vars = query.split('&');

      for (var i = 0; i < vars.length; i++) {
        let pair = vars[i].split('=');
        if (pair[0] == variable) {
          return pair[1];
        }
      }

      return false;
    },

    updateParams() {
      const urlParams = new URLSearchParams(window.location.search);
      urlParams.set('page', this.pageNumber);

      window.location.search = urlParams;
    },
  },

  computed: {
    pageCount() {
      let lengthArrayComments = this.comments.length,
        sizeArrayComments = this.size;

      return Math.ceil(lengthArrayComments / sizeArrayComments);
    },

    paginatedData() {
      const start = this.pageNumber * this.size,
        end = start + this.size;

      return this.comments.slice(start, end);
    },
  },

  created: async function () {
    await axios.get('https://bigcountry-task.vercel.app/comments.json').then((comments) => {
      this.comments = comments.data;
      this.dataFilter();

      this.pageNumber = this.getQueryVariable('page');
    });
  },
};
</script>

<style>
body {
  background-color: #e5e5e5;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.btn-previous {
  margin-right: 1rem;
}
</style>
