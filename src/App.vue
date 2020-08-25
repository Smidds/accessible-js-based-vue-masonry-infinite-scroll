<template>
  <div id="app">
    <div class="header">
      <h1 class="title">Accessible JavaScript based Masonry Layout in Vue.js</h1>
      <p class="new">Now with infinite scrolling!</p>
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
      <infinite-loading @infinite="addMoreItems"></infinite-loading>
    </div>
  </div>
</template>

<script>
import faker from "faker";

import InfiniteLoading from "vue-infinite-loading";
import MasonryLayout from "./components/masonry-layout";
import Card from "./components/card";

export default {
  name: "App",
  components: {
    InfiniteLoading,
    MasonryLayout,
    Card
  },
  data() {
    return {
      items: [...new Array(10)].map(this.createItem)
    };
  },
  methods: {
    createItem() {
      return {
        title: faker.lorem.words(4),
        description: faker.lorem.paragraphs(Math.floor(Math.random() * 4) + 1)
      };
    },
    addMoreItems($state) {
      this.items.push(...[...new Array(20)].map(this.createItem));
      this.$nextTick(() => {
        $state.loaded();
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

.new {
  font-style: italic;
}
</style>
