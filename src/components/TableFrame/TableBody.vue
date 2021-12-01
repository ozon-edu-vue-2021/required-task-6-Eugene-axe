<script>
import { COLUMN_NAMES } from "../../constants/constants";

export default {
  name: "table-body",
  functional: true,
  render(h, context) {
    const { commentRows } = context.props;
    return h(
      "tbody",
      commentRows.map((commentObj) =>
        h(
          "tr",
          { class: { "table-row": true } },
          COLUMN_NAMES.map((col) => h("td", commentObj[col]))
        )
      )
    );
  },
};
</script>
