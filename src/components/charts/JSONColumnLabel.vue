<template>
  <apexchart
    v-if="loaded"
    type="bar"
    :options="options"
    :series="series"
    :height="chartheight"
  ></apexchart>
</template>

<script>
import VueApexCharts from "vue3-apexcharts";

export default {
  name: 'ColumnLabel',
  components: {
    apexchart: VueApexCharts,
  },
  props:{
    basepath:{
      default: "bdd_json"
    },
    selectorname:{
      default: "Stat"
    },
    filename:{
      default: "midipile01_stat_semaine.json"
    },
    columnxaxiscategories:{
      default: [0],
    },
    columnseriesname:{
      default: 'Serie'
    },
    columnseriesdata:{
      default: [0,1,2,3],
    },
    columnborderradius: {
      default: 10
    },
    chartheight:{
      default: 300
    },
    chartid:{
      default: 'columnlabel'
    },
    colorvalue:{
      default: ["#7198be"]
    },
    horizontal:{
      default: false
    }
  },
  data () {
    return {
      loaded: false,
      options: {
        chart: {
          id: this.chartid
        },
        plotOptions: {
            bar: {
              horizontal: this.horizontal,
              borderRadius: this.columnborderradius
            },
        },
        xaxis: {
          categories: this.columnxaxiscategories
        },
        fill: {
          colors: this.colorvalue
        }
      },
      series: [{
        name: this.columnseriesname,
        data: this.columnseriesdata
      }]
    }
  },
  methods: {
    getdatafromjson(basepath, selectorname, filename) {
      const jsondatapath = basepath + '/' + selectorname + '/' + filename;

      console.log('JSON path: ' + jsondatapath);

      try {
        fetch(jsondatapath)
          .then(response => response.json())
          .then(result => this.plotdata(result));
      } catch (e) {
        console.log('Error fetch JSON: ' + e);
        return;
      }
    },
    plotdata(jsondata) {
      console.log('In extractpath');
      console.log(jsondata);

      if (jsondata != undefined) {
        this.series = [{
            name: 'Distance hebdomadaire (km)',
            data: jsondata.stat_distance_km
          }],
        this.options = {
          xaxis: {
            categories: jsondata.Semaine,
            labels: {
              show: false
            }
          }
        }
        this.loaded = true
        console.log('Load graph')
      }
    }
  },
  created() {
    this.getdatafromjson(this.basepath, this.selectorname, this.filename)
  }
}
</script>

<style>
</style>