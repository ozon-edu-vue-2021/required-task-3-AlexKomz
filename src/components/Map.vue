<template>
  <div class="map">
    <h3>Карта офиса</h3>

    <div v-if="!isLoading" class="map-root">
      <MapSvg ref="svg" />
      <TableSvg v-show="false" ref="table" />
    </div>
    <div v-else>Loading...</div>
  </div>
</template>

<script>
import * as d3 from "d3";

import tables from "@/assets/data/tables.json";

import MapSvg from "@/assets/images/map.svg";
import TableSvg from "@/assets/images/workPlace.svg";

export default {
  props: {
    legend: {
      type: Array,
      default: () => [],
    },
  },
  components: {
    MapSvg,
    TableSvg,
  },
  data() {
    return {
      isLoading: false,
      svg: null,
      g: null,
      tables: [],
      tableSVG: null,
    };
  },
  created() {
    this.tables = tables;

    this.updateLegendCounts();
  },
  mounted() {
    this.isLoading = true;

    this.svg = d3.select(this.$refs.svg);
    this.g = this.svg.select(`g`);
    this.tableSVG = d3.select(this.$refs.table);

    if (this.g) {
      this.drawTables();
    } else {
      alert(`SVG is incorrect`);
    }

    this.isLoading = false;
  },
  methods: {
    drawTables() {
      const svgTablesGroupPlace = this.g
        .append("g")
        .classed("groupPlaces", true);

      this.tables.forEach((table) => {
        const targetSeat = svgTablesGroupPlace
          .append(`g`)
          .attr(`transform`, `translate(${table.x}, ${table.y}) scale(0.5)`)
          .attr(`id`, table._id)
          .classed(`employer-place`, true);

        targetSeat
          .append(`g`)
          .attr(`transform`, `rotate(${table.rotate || 0})`)
          .attr("group_id", table.group_id)
          .classed("table", true)
          .html(this.tableSVG.html())
          .attr(
            "fill",
            this.legend.find((it) => it.group_id === table.group_id)?.color ??
              "transparent"
          );
      });
    },
    updateLegendCounts() {
      const tablesCount = this.tables.reduce((acc, table) => {
        acc[table.group_id] = acc[table.group_id] ? acc[table.group_id] + 1 : 1;
        return acc;
      }, {});

      this.$emit(
        `update:legend`,
        this.legend.map((it, index) => {
          it.counter = tablesCount[index];
          return it;
        })
      );
    },
  },
};
</script>

<style scoped>
.map {
  height: 100%;
  width: 100%;
  padding: 24px;
  overflow: hidden;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}

.map-root {
  height: 100%;
  width: 100%;
  overflow: hidden;
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

::v-deep svg {
  height: 100%;
  width: 100%;
}

::v-deep .table {
  cursor: pointer;
}
</style>
