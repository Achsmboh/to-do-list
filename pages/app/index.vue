<!-- eslint-disable vue/attribute-hyphenation -->
<!-- eslint-disable vue/no-template-shadow -->
<template>
  <div class="py-4">
    <div class="container">
      <div
        class="title border-bottom d-flex align-items-center justify-content-between w-100 py-2"
      >
        <h5>Task</h5>
        <div class="d-flex align-items-center ms-auto">
          <!-- Dropdown Categories -->
          <span class="me-2">Categories:</span>
          <select
            v-model="searchCategories"
            class="form-select me-3"
            aria-label="Default select example"
          >
            <option value="all" selected>All Categories</option>
            <option
              v-for="category in uniqueCategories"
              :key="category"
              :value="category"
            >
              {{ category }}
            </option>
          </select>
          <!-- Form input pencarian -->
          <input
            v-model="searchQuery"
            type="text"
            class="form-control"
            placeholder="Search"
          />
          <div class="d-flex align-items-center justify-content-end w-100">
            <span class="me-2"> View As </span>
            <button
              class="btn btn-outline-secondary py-1 px-3"
              @click="isGrid = !isGrid"
            >
              {{ isGrid ? 'Grid' : 'List' }}
            </button>
          </div>
        </div>
      </div>
      <div class="list-task row">
        <CardItem
          v-for="(task, i) in resultQuery"
          :key="i"
          :task="task"
          :isGrid="isGrid"
        />
      </div>
      <div class="action py-2">
        <a
          v-if="!isCreating"
          href="#"
          class="add-button"
          @click="isCreating = !isCreating"
          >Add Task</a
        >
        <div v-else class="add-card">
          <form @submit.prevent="appendTask">
            <div class="card mb-2">
              <div class="card-body d-flex flex-column p-0">
                <input
                  v-model="newTask.title"
                  type="text"
                  placeholder="Title"
                  class="form-control border-0 mb-2"
                />
                <textarea
                  v-model="newTask.description"
                  class="form-control border-0 small"
                  placeholder="Description"
                  rows="3"
                ></textarea>
              </div>
            </div>
            <select
              id="categories"
              v-model="newTask.categories"
              name=""
              class="my-3 w-50"
            >
              <option disabled selected value="">Nothing selected</option>
              <option value="Ringan">Ringan</option>
              <option value="Sedang">Sedang</option>
              <option value="Berat">Berat</option>
            </select>
            <div class="button-wrapper d-flex">
              <button type="submit" class="btn btn-primary me-2">Save</button>
              <button
                class="btn btn-outline-secondary"
                @click="isCreating = !isCreating"
              >
                Cancel
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import CardItem from '@/components/Card/CardItem.vue'

export default {
  components: {
    CardItem,
  },
  data() {
    return {
      // Variabel penampung categories
      searchCategories: '',
      // Variabel penampung teks pencarian
      searchQuery: '',
      // Daftar task
      isCreating: false,
      // Tipe layout daftar task
      isGrid: false,
      task: [
        {
          id: 1,
          title: 'Task 1',
          description: 'ini deskripsi',
          isDone: false,
          categories: 'Sedang',
        },
        {
          id: 2,
          title: 'Task 2',
          description: 'ini deskripsi 2',
          isDone: false,
          categories: 'Berat',
        },
        {
          id: 3,
          title: 'Task 3',
          description: 'ini deskripsi 3',
          isDone: false,
          categories: 'Ringan',
        },
      ],
      newTask: {
        title: '',
        description: '',
        categories: '',
      },
    }
  },
  computed: {
    // mengambil kategori unik dari seluruh daftar task
    uniqueCategories() {
      return [...new Set(this.task.map((task) => task.categories))]
    },
    resultQuery() {
      if (this.searchQuery && this.searchCategories === 'all') {
        return this.task.filter((item) => {
          return this.searchQuery
            .toLowerCase()
            .split(' ')
            .every((v) => item.title.toLowerCase().includes(v))
        })
      } else if (this.searchQuery && this.searchCategories) {
        return this.task.filter((item) => {
          return (
            this.searchQuery
              .toLocaleLowerCase()
              .split(' ')
              .every((v) => item.title.toLocaleLowerCase().includes(v)) &&
            this.searchCategories
              .toLocaleLowerCase()
              .split(' ')
              .every((v) => item.categories.toLocaleLowerCase().includes(v))
          )
        })
      } else if (this.searchQuery) {
        return this.task.filter((item) => {
          return this.searchQuery
            .toLowerCase()
            .split(' ')
            .every((v) => item.title.toLowerCase().includes(v))
        })
      } else if (this.searchCategories === 'all') {
        return this.task
      } else if (this.searchCategories) {
        return this.task.filter((item) => {
          return this.searchCategories
            .toLowerCase()
            .split(' ')
            .every((v) => item.categories.toLocaleLowerCase().includes(v))
        })
      } else {
        return this.task
      }
    },
  },
  methods: {
    appendTask() {
      // Validasi input task sebelum ditambahkan
      if (
        this.newTask.title.trim() === '' ||
        this.newTask.description.trim() === '' ||
        this.newTask.categories.trim() === ''
      ) {
        return alert('Harap Isi Semua Field')
      }

      // Tambahkan task baru ke dalam array
      this.task.push({
        title: this.newTask.title,
        description: this.newTask.description,
        isDone: false,
        categories: this.newTask.categories,
      })

      // Reset form input
      this.newTask.title = ''
      this.newTask.description = ''
      this.newTask.categories = ''
      // tutup form tambah task
      this.isCreating = false
    },
  },
}
</script>
