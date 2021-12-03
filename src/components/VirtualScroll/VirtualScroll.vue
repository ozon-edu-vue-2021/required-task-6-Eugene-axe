<template>
  <div class="virtual-scroll" ref="root" :style="rootStyle">
    <div class="viewport" ref="viewport" :style="viewportStyle">
      <div class="spacer" ref="spacer" :style="spacerStyle">
        <slot name="tableFrame" :data="visibleItems"></slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "virtual-scroll",
  props: {
    inputData: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      scrollTop: 0,
      nodePadding: 3,
      captionAndTheadHeight : 70,
      rowHeight: 35,
      rootHeight: 500,
    };
  },
  computed: {
    amountRowInputData() {
      return this.inputData.length;
    },
    viewportHeight() {
      return this.amountRowInputData * this.rowHeight + this.captionAndTheadHeight;
    },
    indexFrom() {
      const from =
        Math.floor(this.scrollTop / this.rowHeight) - this.nodePadding;
      return Math.max(0, from);
    },
    amountVisibleRow() {
      const count =
        Math.ceil(this.rootHeight / this.rowHeight) + 2 * this.nodePadding;
      return Math.min(this.amountRowInputData - this.indexFrom, count);
    },
    visibleItems() {
      return this.inputData.slice(
        this.indexFrom,
        this.indexFrom +  this.amountVisibleRow
      );
    },
    offsetY() {
      return this.indexFrom * this.rowHeight;
    },
    spacerStyle() {
      return {
        transform: `translateY(${this.offsetY}px)`,
      };
    },
    rootStyle() {
      return {
        overflow: "auto",
        height: this.rootHeight+'px',
      };
    },
    viewportStyle() {
      return {
        overflow: "hidden",
        position: "relative",
        height: this.viewportHeight + "px",
      };
    },
  }, //end computed
  methods: {
    handleScroll() {
      this.scrollTop = this.$refs.root.scrollTop;
    },
  },
  mounted() {
    this.$refs.root.addEventListener("scroll", this.handleScroll);
  },
  beforeDestroy() {
    this.$refs.root.removeEventListener("scroll", this.handleScroll);
  },
};
</script>

<style scoped>
.virtual-scroll {
  overflow: auto;
}
</style>