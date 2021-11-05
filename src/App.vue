<template>
  <div id="app">
    <div class="office">
      <Map :legend.sync="legend" @click="currentPerson = $event" />
      <SideMenu
        :isUserOpenned="!!currentPerson"
        :person.sync="currentPerson"
        :legend="legend"
      >
        <Draggable v-model="legend">
          <LegendItem
            v-for="item in legend"
            :key="item.group_id"
            :color="item.color"
            :text="item.text"
            :counter="item.counter"
            class="legend__item"
          />
        </Draggable>
      </SideMenu>
    </div>
  </div>
</template>

<script>
import legend from "@/assets/data/legend.json";

import Draggable from "vuedraggable";

import Map from "./components/Map.vue";
import SideMenu from "./components/SideMenu.vue";
import LegendItem from "./components/SideMenu/LegendItem.vue";

export default {
  name: "App",
  components: {
    Map,
    SideMenu,
    LegendItem,
    Draggable,
  },
  data: () => ({
    legend: [],
    currentPerson: null,
  }),
  created() {
    this.legend = legend;
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  background-color: #fafafa;
  padding: 24px;
  box-sizing: border-box;
}

html,
body,
#app {
  height: 100%;
}

* {
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

.office {
  display: grid;
  grid-template-columns: 1fr 320px;
  border-radius: 6px;
  border: 1px solid #ccd8e4;
  height: 100%;
  background: white;
  max-width: 1500px;
  margin: 0 auto;
}

/* From SideMenu */
.content .legend .legend__items {
  flex: 1;
  width: 100%;
}

.content .legend .legend__items .legend__item:not(:first-child) {
  margin-top: 16px;
}

.content .legend .legend__items .legend__item {
  cursor: pointer;
}

.content .legend .legend__items .legend__item.sortable-chosen {
  opacity: 25%;
}
</style>
