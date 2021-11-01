<template>
  <div class="map">
    <h3>Карта офиса</h3>

    <div v-if="!isLoading" class="map-root">
      <MapSVG ref="svg" @click="onClick" />
      <TableSVG v-show="false" ref="table" />
    </div>
    <div v-else>Loading...</div>
  </div>
</template>

<script>
import MapSVG from "../assets/images/map.svg";
import TableSVG from "../assets/images/workPlace.svg";
import * as d3 from "d3";
import tables from "../assets/data/tables.json";
import legend from "@/assets/data/legend.json";

export default {
  components: {
    MapSVG,
    TableSVG,
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
  mounted() {
    this.isLoading = true;

    this.svg = d3.select(this.$refs.svg);
    this.g = this.svg.select("g");
    this.tableSVG = d3.select(this.$refs.table);

    this.tables = tables;

    if (this.g) {
      this.drawTables();
    } else {
      console.log("ERROR - WRONG SVG");
    }

    //обнуляем счетчики в legend
    legend.forEach((element) => {
      element.counter = 0;
    });
    //заполняем счетчики в legend
    this.tables.forEach((element) => {
      let group = legend.find((it) => {
        return it.group_id == element.group_id;
      });
      if (group === undefined) {
        legend.push({
          group_id: element.group_id,
          color: "red",
          text: `Неучтенное подразделение id=${element.group_id}`,
          counter: 1,
        });
      } else {
        group.counter++;
      }
    });

    this.isLoading = false;
  },
  methods: {
    drawTables() {
      const svgTablesGroupPlace = this.g
        .append("g")
        .classed("groupPlaces", true);

      this.tables.map((table) => {
        const targetSeat = svgTablesGroupPlace
          .append("g")
          .attr("transform", `translate(${table.x}, ${table.y}) scale(0.5)`)
          .attr("id", table._id)
          .classed("employer-place", true);

        targetSeat
          .append("g")
          .attr("transform", `rotate(${table.rotate || 0})`)
          .attr("group_id", table.group_id)
          .classed("table_id", true)
          .html(this.tableSVG.html())
          .attr(
            "fill",
            legend.find((it) => it.group_id === table.group_id)?.color ??
              "transparent"
          );
      });
    },
    onClick(e) {
      this.$emit(
        "table-select",
        e.srcElement.closest(".employer-place")?.getAttribute("id")
      );
      // console.log(e.srcElement.closest(".table_id")?.getAttribute("group_id"));
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
