<script>
import TableHeader from "./TableHeader.vue";
import TableBody from "./TableBody.vue";
import { ASC, COLUMN_NAMES } from "../../constants/constants";

export default {
  name: "TableFrame",
  components: {
    "table-header": TableHeader,
    "table-body": TableBody,
  },
  props: {
    dataComments: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      sortState: {},
      isSort: false,
      isFilter: false,
    };
  },
  methods: {
    initSortState() {
      return COLUMN_NAMES.reduce((acc, item) => {
        acc[item] = { status: false };
        acc[item] = { order: "" };
        return acc;
      }, {});
    },
    updateSortState(name, order) {
      this.isSort = true;
      const sortObj = { ...this.initSortState() };
      sortObj[name].status = true;
      sortObj[name].order = order;
      this.sortState = { ...sortObj };
    },
    sortComments(array, name, order) {
      let inv = order === ASC ? 1 : -1;
      return array.sort((a, b) => (a[name] < b[name] ? 1 * inv : -1 * inv));
    },
    filteredComments(filterObj) {
      this.$emit('filter' , filterObj);
    },
  },
  computed: {
    comments() {
      let name = "";
      let order = "";

      for (let nameCol in this.sortState) {
        if (this.sortState[nameCol].status) {
          name = nameCol;
          order = this.sortState[nameCol].order;
        }
      }
      return this.isSort
        ? this.sortComments(this.dataComments, name, order)
        : this.dataComments;
    },
  },
  created() {
    this.sortState = this.initSortState();
  },
  render() {
    return (
      <table class="table">
        <caption>Комментарии</caption>
        <table-header
          updateSortState={this.updateSortState}
          sortState={this.sortState}
          filteredComments={this.filteredComments}
        />
        <table-body commentRows={this.comments} />
      </table>
    );
  },
};
</script>

<style scoped>
.table {
  width: 1000px;
  margin: 0 auto;
  border-left: 2px solid silver;
  border-right: 2px solid silver;
}
.table caption {
  height: 35px;
}
table >>> .table-row {
  display: grid;
  grid-template-columns: 1fr 1fr 3fr 5fr;
  height: 35px;
}

table >>> .table-row > * {
  border-right: 1px solid silver;
  /* overflow:hidden; */
}
table >>> .table-row > *:last-child {
  border-right: none;
}
</style>
