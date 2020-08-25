<template>
  <div ref="container" class="masonry-layout">
    <MasonryCell :gutter="gutter">
      <slot/>
    </MasonryCell>
  </div>
</template>

<script>
import MasonryCell from "./masonry-cell.vue";

export default {
  components: {
    MasonryCell
  },
  props: {
    minColWidth: { type: Number, default: 600 },
    minNumCols: { type: Number, default: 1 },
    gutter: String
  },
  data() {
    return {
      noOfColumns: 1,
      masonryCells: []
    };
  },
  mounted() {
    this.setCells();
    this.onResize();
    window.addEventListener("resize", this.onResize);
  },
  beforeDestroy() {
    window.removeEventListener("resize", this.onResize);
  },
  methods: {
    refreshLayout() {
      this.setCells();
      this.shuffleColumns();
    },
    setCells() {
      console.log("masonry-layout > setupLayout");
      this.masonryCells = Array.prototype.map.call(
        this.$refs.container.children,
        cellElement => {
          var style = getComputedStyle(cellElement);
          return {
            element: cellElement,
            outerHeight:
              parseInt(style.marginTop, 10) +
              cellElement.offsetHeight +
              parseInt(style.marginBottom, 10)
          };
        }
      );
    },
    onResize() {
      const root = this.$refs.container;
      // only layout when the number of columns has changed
      var newNoOfColumns = Math.floor(root.offsetWidth / this.minColWidth);
      if (
        newNoOfColumns !== this.noOfColumns &&
        newNoOfColumns >= this.minNumCols
      ) {
        // initialize
        this.noOfColumns = newNoOfColumns;
        this.shuffleColumns();
      }
    },
    shuffleColumns() {
      const root = this.$refs.container;
      var columns = Array.from(new Array(this.noOfColumns)).map(function(
        column
      ) {
        return {
          cells: [],
          outerHeight: 0
        };
      });

      // divide...
      for (let cell of this.masonryCells) {
        var minOuterHeight = Math.min(
          ...columns.map(function(column) {
            return column.outerHeight;
          })
        );
        var column = columns.find(function(column) {
          return column.outerHeight === minOuterHeight;
        });
        column.cells.push(cell);
        column.outerHeight += cell.outerHeight;
      }

      // calculate masonry height
      var masonryHeight = Math.max(
        ...columns.map(function(column) {
          return column.outerHeight;
        })
      );

      // ...and conquer
      var order = 0;
      for (let column of columns) {
        for (let cell of column.cells) {
          cell.element.style.order = order++;
          // set the cell's flex-basis to 0
          cell.element.style.flexBasis = 0;
        }
        // set flex-basis of the last cell to fill the
        // leftover space at the bottom of the column
        // to prevent the first cell of the next column
        // to be rendered at the bottom of this column
        column.cells[column.cells.length - 1].element.style.flexBasis =
          column.cells[column.cells.length - 1].element.offsetHeight +
          masonryHeight -
          column.outerHeight -
          1 +
          "px";
      }

      // set the masonry height to trigger
      // re-rendering of all cells over columns
      // one pixel more than the tallest column
      root.style.maxHeight = masonryHeight + 1 + "px";
    }
  }
};
</script>

<style lang="scss">
.masonry-layout {
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  align-items: stretch;
}
</style>
