<template>
  <div class="form-create-post">
    <div class="mb-3">
      <label for="post-body" class="form-label">Добавить комментарий</label>
      <input
        type="email"
        class="form-control mb-3"
        id="post-body"
        placeholder="Введите сюда текст комментария"
        @input="$emit('input', $event.target.value)"
      />

      <button class="btn btn-success" @click="eventClick">Добавить</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CreateComment',
  props: {
    eventClick: Function,
  },

  methods: {
    handleInput() {
      console.log(this.commentText);
      this.$emit('input', this.commentText);
    },
  },
};
</script>

<style>
.form-create-post {
  width: 300px;
  margin: auto;
}
</style>
