<script>
export default {
  name: "tooltip-filter",
  functional: true,
  render(h, context) {
    const { filteredComments , nameCol } = context.props;
    const handlerInput = (event) => {
      filteredComments(event.target.value , nameCol)
    };
    return h("div", [
      h("div", { class: { tooltip: true} }, [
        h("p", "Что наити?"),
        h("input", {
          class: { "tooltip-input": true },
          on: { input: handlerInput },
        }),
      ]),
    ]);
  },
};
</script>

<style scoped>
.tooltip {
  position: absolute;
  top: 20px;
  left: -105px;
  border: 2px solid silver;
  border-radius: 5px;
  background-color: #fff;
}
.display-none {
    display: none;
}
.tooltip-input {
  margin: 0 2px 2px 2px;
}
</style>
