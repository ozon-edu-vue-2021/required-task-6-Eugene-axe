<template>
  <div id="app">
    <header>
      <nav class="header-nav">
        <ul class="header-nav__list">
          <li
            class="header-nav__list__item"
            v-for="item in navMenu"
            :key="item"
          >
            <input
              type="radio"
              :id="item"
              :value="item"
              v-model="navigationChecked"
            />
            <label :for="item" v-text="item.toLowerCase()"></label>
          </li>
        </ul>
      </nav>
    </header>

    <pagination-table
      v-if="isShowPagination"
      :inputData="filteredDataComments"
      :currentPage="currentPage"
      :countRowPerPage="countRowPerPage / 2"
      @updateCurrentPage="updateCurrentPage"
    >
      <template v-slot:tableFrame="data">
        <table-frame :dataComments="data.data" @filter="handlerFilter" />
      </template>
    </pagination-table>

    <infinite-scroll
      v-if="isShowInfinite"
      :inputData="partData"
      :scrollEnd="scrollEnd"
      :currentPage="currentPage"
      @pushData="pushData"
    >
      <template v-slot:tableFrame="data">
        <table-frame :dataComments="data.data" @filter="handlerFilter">
          <template v-slot:observer>
            <observer-component />
          </template>
        </table-frame>
      </template>
    </infinite-scroll>

    <virtual-scroll v-if="isShowVirtual" :inputData="filteredDataComments">
      <template v-slot:tableFrame="data">
        <table-frame :dataComments="data.data" @filter="handlerFilter" />
      </template>
    </virtual-scroll>
  </div>
</template>

<script>
import TableFrame from "./components/TableFrame";
import PaginationTable from "./components/Pagination/Pagination.vue";
import InfiniteScroll from "./components/InfiniteScroll/InfiniteScroll.vue";
import VirtualScroll from "./components/VirtualScroll/VirtualScroll.vue";
import {
  INFINITE_SCROLL,
  PAGINATION,
  VIRTUAL_SCROLL,
} from "./constants/constants";

export default {
  name: "App",
  components: {
    "table-frame": TableFrame,
    "pagination-table": PaginationTable,
    "infinite-scroll": InfiniteScroll,
    "virtual-scroll": VirtualScroll,
  },
  data() {
    return {
      navigationChecked: INFINITE_SCROLL,
      dataComments: [],
      partData: [],
      scrollEnd: false,
      filter: {},
      currentPage: 1,
      countRowPerPage: 20,
    };
  },
  created() {
    fetch("https://jsonplaceholder.typicode.com/comments")
      .then((response) => response.json())
      .then((data) => (this.dataComments = data))
      .then(() => {
        this.partData = this.filteredDataComments.slice(
          0,
          this.countRowPerPage
        );
      });
  },
  mounted() {},
  computed: {
    filteredDataComments() {
      return this.isFilter
        ? this.dataComments.filter((comment) => {
            return this.isIncludesProperty(this.filter, comment);
          })
        : this.dataComments;
    },
    isFilter() {
      return !!Object.keys(this.filter).length;
    },
    navMenu() {
      return [PAGINATION, INFINITE_SCROLL, VIRTUAL_SCROLL];
    },
    isShowPagination() {
      return this.navigationChecked === PAGINATION;
    },
    isShowInfinite() {
      return this.navigationChecked === INFINITE_SCROLL;
    },
    isShowVirtual() {
      return this.navigationChecked === VIRTUAL_SCROLL;
    },
  },
  methods: {
    pushData(page) {
      setTimeout(() => {
        const newData = this.filteredDataComments.slice(
          (page - 1) * this.countRowPerPage,
          page * this.countRowPerPage
        );
        const oldLength = this.partData.length;
        this.partData = [...this.partData, ...newData];

        const newLength = this.partData.length;
        this.scrollEnd = oldLength === newLength && page !== 1;
      }, 100);
    },
    handlerFilter(filterObj) {
      this.updateCurrentPage(1);
      this.filter = { ...this.filter, ...filterObj };
      this.partData = [];
      this.pushData(1, 50);
    },
    updateCurrentPage(n) {
      this.currentPage = n;
    },
    isIncludesProperty(objWhat, objWhere) {
      let isContains = true;
      for (const key in objWhat) {
        if (!(objWhere[key]+"").includes(objWhat[key])) return false;
      }
      return isContains;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.header-nav {
  padding-bottom: 5px;
  border-bottom: 3px solid black;
  margin: 0px 50px 10px;
}
.header-nav__list {
  max-width: 60%;
  margin: 10px auto;
  list-style-type: none;
  display: flex;
  justify-content: space-around;
}
</style>
