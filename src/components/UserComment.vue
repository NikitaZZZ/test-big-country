<template>
  <div class="card comment">
    <div class="comment-header">
      <img class="avatar" alt="avatar" :src="comment.author.avatar" />

      <p class="text-muted comment-date">{{ comment.created_at }}</p>
      <p class="card-title author-name">{{ comment.author.name }}</p>

      <p class="text-muted author-company">{{ comment.author.company }}</p>
    </div>
    <div class="comment-body">
      <p>{{ comment.body }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UserComment',
  props: {
    comment: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style>
.comment {
  max-width: 90vw;
  border-radius: 10px;
  margin: 1rem auto;
  padding-top: 0.5rem;
  background-color: #fff;
}

.comment-header {
  width: 100%;
  height: 40%;
}

.avatar {
  width: 45px;
  height: 45px;
  border-radius: 50%;
  float: left;
  margin: 0.5rem 1rem;
}

.author-name {
  text-align: left;
  font-size: 1.1rem;
  font-weight: bold;
  margin-top: 0.3rem;
  margin-bottom: 0;
}

.author-company {
  text-align: left;
  font-size: 0.9rem;
  font-weight: bold;
  margin-bottom: 0;
}

.comment-date {
  float: right;
  font-size: 0.9rem;
  margin-top: 0.5rem;
  margin-right: 1.5rem;
}

.comment-body {
  float: left;
  text-align: left;
  margin-left: 1rem;
  margin-right: 1rem;
}
</style>
