<template>
  <li class="list-group-item d-flex justify-content-between">
    <a :href="link.url">{{ link.linktext }}</a>
    <div>
      <span class="badge badge-primary">{{ link.votes }}</span>
      <button
        class="btn btn-primary btn-sm ml-1"
        @click="$emit('upvote')"
        aria-label="Hochvoten"
      >
        <i class="lni lni-thumbs-up"></i>
      </button>
      <button
        class="btn btn-danger btn-sm ml-1"
        @click="$emit('delete-link')"
        aria-label="Löschen"
      >
        <i class="lni lni-trash"></i>
      </button>
    </div>
  </li>
</template>

<script>
export default {
  props: {
    link: {
      required: true,
      type: Object,
    },
  },
};
</script>

<style scoped>
</style>