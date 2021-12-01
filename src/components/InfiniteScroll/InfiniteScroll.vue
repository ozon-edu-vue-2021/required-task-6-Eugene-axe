<template>
  <div class="infinite-scroll" ref="root">
    <slot name="tableFrame" :data="inputData"></slot>
    <div v-if="scrollEnd">это конец</div>
    <observer-component
      v-else
      @intersect="loadDataRow"
      :options="options"
    ></observer-component>
  </div>
</template>

<script>
import ObserverComponent from "../ObserverComponent/ObserverComponent.vue";

export default {
  name: "infinite-scroll",
  components: {
    "observer-component": ObserverComponent,
  },
  props: {
    inputData: {
      type: Array,
      default: () => [],
    },
    // currentPage: {
    //   type: Number,
    //   required: true,
    // },
    scrollEnd: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      options: {},
      page: 1,
    };
  },
  methods: {
    loadDataRow() {
      this.$emit("pushData", this.page);
      this.page += 1;
    },
  },
  mounted() {
    this.options = {
      root: document.querySelector(".infinite-scroll"),
      rootMargin: "400px",
      treshold: 0.1,
    };
  },
};
</script>

<style scoped>
.infinite-scroll {
  height: 500px;
  overflow-x: hidden;
  overflow-y: scroll;
}
</style>
