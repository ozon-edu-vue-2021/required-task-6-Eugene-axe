<template>
  <div class="pagination-table">
    <slot name="tableFrame" :data="rowPage"></slot>
    <div class="pagination-bar">
      <ul class="pagination-list">
        <li v-show="currentPage !== 1">
          <a class="pagination-list__link" href="#" @click="updateCurrentPage(currentPage -1)"
            >предыдущая
          </a>
        </li>
        <li
          v-for="n in numericInPagBar"
          :key="n + getRandKey()"
          :class="{ 'active-page': currentPage === n }"
        >
          <a
            class="pagination-list__link"
            href="#"
            v-text="n"
            @click="handleNumberPage(n)"
          ></a>
        </li>
        <li v-show="isShowNext">
          <a class="pagination-list__link" href="#" @click="updateCurrentPage(currentPage -1)"
            >следующая
          </a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "pagination-table",
  data() {
    return {
      countPagLink: 5,
      isShowNext: true,
    };
  },
  props: {
    inputData: {
      type: Array,
      default: () => [],
    },
    currentPage : {
      type : Number,
      required: true,
    },
    countRowPerPage : {
      type : Number,
      reauired : true
    }
  },
  computed: {
    rowPage() {
      const from = this.countRowPerPage * (this.currentPage - 1);
      const to = from + this.countRowPerPage;
      return this.inputData.slice(from, to);
    },
    amountPage() {
      return Math.ceil(this.inputData.length / this.countRowPerPage);
    },
    visiblePageCount() {
      return Math.min(this.amountPage, this.countPagLink);
    },
    numericInPagBar() {
      let numeric = new Array(this.visiblePageCount).fill(0);
      const middle = Math.floor(this.visiblePageCount / 2) + 1;
      if (this.currentPage <= middle) {
        numeric = numeric.reduce((acc, item, i) => {
          acc.push(this.currentPage + i);
          return acc;
        }, []);
      } else {
        numeric = numeric.reduce((acc, item, i) => {
          this.isShowNext = this.currentPage < this.amountPage;
          acc.push(
            this.currentPage + 1 + i - middle <= this.amountPage
              ? this.currentPage + 1 + i - middle
              : ""
          );
          return acc;
        }, []);
      }
      return numeric;
    },
  },
  methods: {
    handleNumberPage(n) {
      this.updateCurrentPage(n);
    },
    getRandKey() {
      return new Date().getTime() * Math.random();
    },
    updateCurrentPage(n) {
      this.$emit('updateCurrentPage' , n)
    }
  },
};
</script>

<style scoped>
.pagination-bar {
  width: 600px;
  margin: 0 auto;

  --activelink-bg-color: rgb(130, 107, 206);
}

.pagination-list {
  width: 100%;
  margin: 10px auto 0;
  list-style-type: none;
  display: flex;
  flex-flow: row nowrap;
  justify-content: space-around;
  align-items: center;
}
.pagination-list__link {
  text-decoration: none;
  cursor: pointer;
  color: var(--activelink-bg-color);
  user-select: none;
}
.active-page {
  padding: 0.5em;
  border-radius: 3px;
  background-color: var(--activelink-bg-color);
}
.active-page a {
  color: #fff;
}
</style>
