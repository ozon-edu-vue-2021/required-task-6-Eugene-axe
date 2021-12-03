<script>
export default {
  name: "observer-component",
  props : {
      options : {
          type : Object,
          defualt : () => ({})
      }
  },
  data() {
    return {
      observer: null,
    };
  },
  mounted() {
    this.observer = new IntersectionObserver(([entry]) => {
      if (entry && entry.isIntersecting) {
        this.$emit("intersect");
      }
    }, this.options);
    this.observer.observe(this.$el); 
  },
  destroyed(){
      this.observer.disconnect();
  },
  render(h) {
    return h("div", { class: { observer: true } } ,[h('p', '...download')]);
  },
};
</script>
