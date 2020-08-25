<script>
export default {
  functional: true,
  props: {
    gutter: { type: String, default: "4px" }
  },
  render: function(h, { children, props }) {
    return children.map(child =>
      h(`div`, { class: "masonry-cell", style: `margin: ${props.gutter}` }, [
        child
      ])
    );
  }
};
</script>

<style scoped lang="scss">
.masonry-cell {
  flex: 1;
}
</style>
