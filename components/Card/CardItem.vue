<template>
  <div
    :class="[
      'item-task d-flex align-items-start border-bottom pt-3 pb-4 card my-1',
      isGrid ? 'col-12 col-md-6 col-lg-4' : 'col-12',
    ]"
  >
    <input
      id="tasks"
      v-model="localTask.isDone"
      type="checkbox"
      name="status"
      class="me-2 mt-2"
      :checked="task.isDone"
    />
    <div
      :class="[
        'd-flex flex-column',
        task.isDone ? 'text-decoration-line-through fst-italic' : '',
      ]"
    >
      <nuxt-link class="title-task mb-1" :to="'app/detail/' + task.id">
        {{ task.title }}
      </nuxt-link>
      <div>
        <p class="small">
          Categories : <span>{{ task.categories }}</span>
        </p>
      </div>
      <div class="description-task small text-muted">
        {{ task.description }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    task: {
      type: Object,
      default: () => {},
    },
    isGrid: {
      type: Boolean,
      required: true,
      default: false,
    },
  },

  computed: {
    localTask: {
      get() {
        return this.task
      },
      set(newValue) {
        this.$emit('update:task', newValue)
      },
    },
  },
}
</script>
