<script>
import { ASC, DESC } from "../../constants/constants";
import TooltipFilter from "./TooltipFilter.vue";

export default {
  name: "sort-filter-block",
  functional: true,

  render(h, context) {
    const { nameCol, updateSortState, sortState, filteredComments } = context.props;
    const isSort = sortState[nameCol].status;
    const order = sortState[nameCol].order;

    let isDisplay = true;

    const handlerSort = () => {
      let updOrder = order === ASC ? DESC : ASC;
      updateSortState(nameCol, updOrder);
    };
    return (
      <div>
        <div class="sort-filter-block">
          <button class="th-btn btn-sort" onClick={handlerSort}>
            {!isSort ? "↕" : order === ASC ? "↓" : "↑"}
          </button>
          <button class="th-btn btn-filter">
            &#9734;
          </button>
          <div class="tooltip">
            <TooltipFilter
              filteredComments={filteredComments}
              nameCol={nameCol}
              display={isDisplay}
            />
          </div>
        </div>
      </div>
    );
  },
};
</script>

<style scoped>
.sort-filter-block .th-btn {
  border: none;
  box-sizing: border-box;
  border-right: 1px solid silver;
  background-color: transparent;
  font-size: 1em;
  cursor: pointer;
}
.th-btn:last-of-type {
  border: none;
}

.sort-filter-block {
  position: relative;
}

.tooltip { 
  display : none;
}

.sort-filter-block:hover .tooltip, 
.sort-filter-block:focus-within .tooltip { 
  display: block;  
}


 .btn-sort:hover ~ .tooltip{
  display : none;
}
</style>
