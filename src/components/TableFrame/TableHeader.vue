<script>
import { COLUMN_NAMES } from '../../constants/constants';
import SortFilterBlock from "./SortFilterBlock.vue";


export default {
  name: "table-header",
  functional: true,
  render(h, context) {
    const { updateSortState , sortState , filteredComments } = context.props;

    return h("thead", [
      h(
        "tr",
        {
          class: {
            "table-row": true,
          },
        },
        COLUMN_NAMES.map((nameCol) =>
          h("th", [
            nameCol,
            h(SortFilterBlock, {
              props: {
                nameCol,
                updateSortState,
                sortState,
                filteredComments
              }
            }),
          ])
        )
      ),
    ]);
  },
};
</script>

<style scoped>
th {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
</style>
