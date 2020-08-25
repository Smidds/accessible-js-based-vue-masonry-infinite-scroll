<template>
  <div id="app">
    <div class="header">
      <h1 class="title">Accessible JavaScript based Masonry Layout in Vue.js</h1>
      <button class="add-more" @click="addMoreItems(100)">Add more items</button>
    </div>
    <div class="layout-container">
      <MasonryLayout gutter="20px" ref="layout" :minColWidth="560">
        <Card
          v-for="(item, i) in items"
          :key="i"
          :index="i"
          :title="item.title"
          :description="item.description"
        />
      </MasonryLayout>
    </div>
  </div>
</template>

<script>
import faker from "faker";

import MasonryLayout from "./components/masonry-layout";
import Card from "./components/card";

export default {
  name: "App",
  components: {
    MasonryLayout,
    Card
  },
  data() {
    return {
      items: [...new Array(500)].map(this.createItem)
    };
  },
  methods: {
    createItem() {
      return {
        title: faker.lorem.words(4),
        description: faker.lorem.paragraphs(Math.floor(Math.random() * 4) + 1)
      };
    },
    addMoreItems(amountToAdd) {
      this.items.push(...[...new Array(amountToAdd)].map(this.createItem));
      this.$nextTick(() => {
        this.$refs.layout.refreshLayout();
      });
    }
  }
};
</script>

<style lang="scss" scoped>
body,
html {
  margin: 0;
  padding: 0;
}

* {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  text-align: center;
  color: #2c3e50;
}

.header {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 15px;
  border-bottom: 1px black solid;
  position: -webkit-sticky;
  position: sticky;
  top: 0;
  background-color: white;

  .add-more {
    margin-top: 15px;
    width: 150px;
    padding: 10px;
    background-color: rgb(184, 82, 184);
    font-size: 15px;
    color: white;
    border: black;
    border-radius: 10px;
  }
}

.layout-container {
  max-width: 1125px;
  margin: 0 auto;
}

.title {
  font-size: 25px;
  margin: 0;
}
</style>
