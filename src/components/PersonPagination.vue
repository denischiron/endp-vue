<template>
  <div class="pagination-box box">
    <nav class="pagination is-rounded">
      <a class="pagination-previous" :class="{ 'disabled': currentPage === 1 }"
         @click="handleChangePage(currentPage - 1)">
        <span class="icon is-primary">
          <i class="fas fa-chevron-left"></i>
        </span>
      </a>
      <ul class="pagination-list">
        <!-- Bouton pour aller à la première page -->
        <li v-if="currentPage > 2">
          <a class="pagination-link" aria-label="Aller à la page 1" @click="handleChangePage(1)">1</a>
        </li>
        <!-- Ellipse de début -->
        <li v-if="currentPage > 3">
          <span class="pagination-ellipsis">&hellip;</span>
        </li>
        <!-- Page précédente -->
        <li v-if="currentPage > 1">
          <a class="pagination-link" aria-label="Page précédente" @click="handleChangePage(currentPage - 1)">
            {{ currentPage - 1 }}
          </a>
        </li>
        <!-- Page actuelle -->
        <li>
          <a class="pagination-link is-current" aria-label="Page actuelle" aria-current="page">
            {{ currentPage }}
          </a>
        </li>
        <!-- Page suivante -->
        <li v-if="currentPage < totalPages">
          <a class="pagination-link" aria-label="Page suivante" @click="handleChangePage(currentPage + 1)">
            {{ currentPage + 1 }}
          </a>
        </li>
        <!-- Ellipse de fin -->
        <li v-if="currentPage < totalPages - 2">
          <span class="pagination-ellipsis">&hellip;</span>
        </li>
        <!-- Bouton pour aller à la dernière page -->
        <li v-if="currentPage < totalPages - 1">
          <a class="pagination-link" aria-label="Aller à la dernière page" @click="handleChangePage(totalPages)">
            {{ totalPages }}
          </a>
        </li>
      </ul>
      <a class="pagination-next" :class="{ 'disabled': currentPage === totalPages }"
         @click="handleChangePage(currentPage + 1)">
        <span class="icon is-primary">
          <i class="fas fa-chevron-right"></i>
        </span>
      </a>
    </nav>
    <div class="pagination-panel">
      <label class="label" for="limit-results">Résultats par page</label>
      <input id="limit-results" class="input items-by-page-input" type="number" placeholder="50"
             :min="itemsByPageMinDisplay" :max="itemsByPageMaxDisplay" step="10"
             v-model="itemsByPageDisplay" @change="handleItemsPerPageChange">
    </div>
  </div>
</template>


<script>
export default {
  name: 'PaginationComponent',
  props: {
    currentPage: Number,
    totalPages: Number,
    itemsByPageDefault: Number,
    itemsByPageMax: Number,
    itemsByPageMin: Number,
  },
  data() {
    return {
      itemsByPageMinDisplay: this.$props.itemsByPageMin,
      itemsByPageMaxDisplay: this.$props.itemsByPageMax,
      itemsByPageDisplay: this.$props.itemsByPageDefault,
    };
  },
  computed: {
    pages() {
      let pages = [];
      for (let i = 1; i <= this.totalPages; i++) {
        pages.push(i);
      }
      return pages;
    },
  },
  watch: {
    itemsByPageDisplay(newVal) {
      if (newVal < this.itemsByPageMinDisplay) {
        this.itemsByPageDisplay = this.itemsByPageMinDisplay;
      } else if (newVal > this.itemsByPageMaxDisplay) {
        this.itemsByPageDisplay = this.itemsByPageMaxDisplay;
      }
    },
  },
  methods: {
    /**
     * Triggered when the user clicks on a page number
     *
     * @param<Number> page
     */
    handleChangePage(page) {
      if (page >= 1 && page <= this.totalPages) {
        this.$emit('update:change-page', page);
      }
    },
    /**
     * Triggered when the user changes the number of items display per page
     */
    handleItemsPerPageChange() {
      this.$emit('change:items-by-page-display', this.itemsByPageDisplay);
    },
  },
};
</script>


<style scoped>

nav .pagination {
   background-color: #f5f5f5 !important; ;
  padding: 1rem 1.5rem 1.5rem 1.5rem;
  border-radius: 5px;
  top: 0;
  position: sticky;
}

.pagination-panel {
  align-items: center;
  justify-content: space-between;
}

.pagination-panel .label {
  margin-right: 10px;
}

.items-by-page-input {
  width: 80px;
  padding: 0.5em;
  margin-left: 10px;
}

.pagination-box .pagination-input .label {
  margin-left: 10px;
  padding: 5px;
}


.disabled {
  pointer-events: none;
  opacity: 0.5;
}

.pagination-panel {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 20px;
}


.pagination-previous, .pagination-next {
  margin-right: 10px;
}
</style>
