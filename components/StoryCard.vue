<template>
  <article class="story-card">
    <div class="content">
      <p>
        {{ visibleText }}
      </p>
      <button
        v-show="!isExpanded"
        class="btn-transparent btn-see-more"
        @click="toggleText"
      >
        See more
      </button>
    </div>

    <div v-if="isAuthor" class="story-card-actions">
      <button class="btn btn-dark-green btn-delete" @click="toggleDeleteModal">
        Delete
      </button>
      <NuxtLink class="btn btn-dark-green btn-edit" :to="editLink">
        Edit
      </NuxtLink>
    </div>

    <DeleteModal
      v-show="deleteModal"
      :story-id="id"
      @delete="onDelete"
      @close="toggleDeleteModal"
    />
  </article>
</template>

<script>
import DeleteModal from '@/components/DeleteModal'

export default {
  components: {
    DeleteModal,
  },
  props: {
    id: {
      type: String,
      required: true,
    },
    text: {
      type: String,
      required: true,
    },
    author: {
      type: String,
      required: true,
    },
    visibleLength: {
      type: Number,
      default: 140,
    },
  },
  data() {
    return {
      isExpanded: false,
      deleteModal: false,
    }
  },
  computed: {
    visibleText() {
      return this.isExpanded
        ? this.text
        : `${this.text.substring(0, this.visibleLength)}[...]`
    },
    isAuthor() {
      const auth = this.$store.state.auth
      return auth && auth.id === this.author
    },
    editLink() {
      return `/my-stories/${this.id}/edit`
    },
  },
  mounted() {
    if (this.text.length <= this.visibleLength) {
      this.isExpanded = true
    }
  },
  methods: {
    toggleText() {
      this.isExpanded = !this.isExpanded
    },
    toggleDeleteModal() {
      this.deleteModal = !this.deleteModal
    },
    onDelete(data) {
      this.$emit('delete', data)
    },
  },
}
</script>

<style lang="scss">
@use '@/assets/css/vars' as vars;

.story-card {
  background-color: vars.$white;
  border-radius: vars.$border-radius;
  margin-bottom: 20px;

  .content {
    padding: 20px;
  }
  p {
    margin: 0px;
    color: vars.$black;
    margin-bottom: 20px;
  }
  .btn-see-more {
    width: 100%;
  }
  .story-card-actions {
    display: flex;
    flex-direction: row;

    .btn {
      flex-grow: 1;
      text-align: center;
      width: 50%;
    }
    .btn-delete {
      border-radius: 0 0 0 vars.$border-radius;
      margin-right: 1px;
      color: vars.$white;
      background-color: vars.$black;
    }
    .btn-edit {
      border-radius: 0 0 vars.$border-radius 0;
    }
  }
}
</style>
