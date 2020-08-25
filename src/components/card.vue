<template>
  <div class="item">
    <div class="item-title">#{{index}}: {{title}}</div>
    <div class="item-description">{{description}}</div>
    <button>View More!</button>
  </div>
</template>

<script>
export default {
  props: {
    title: String,
    description: String,
    index: Number
  }
};
</script>

<style lang="scss" scoped>
.item {
  background-color: lightblue;
  border-radius: 10px;
  border: 1px black solid;
  font-size: 25px;
  font-weight: 400;
  width: 518px;
  padding: 20px;
  box-sizing: border-box;

  @media screen and (max-width: 1134px) {
    width: 100%;
  }

  .item-title {
    text-transform: capitalize;
    border-bottom: 1px grey solid;
    margin-bottom: 10px;
  }

  .item-description {
    font-size: 15px;
    text-align: left;
  }
}
</style>
