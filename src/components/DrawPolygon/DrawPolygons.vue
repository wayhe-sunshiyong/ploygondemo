<template>
  <div class="drawPolygons">
    <div id="map"></div>
    <div class="calculation-box">
      <p>使用右上角绘图工具绘制多边形</p>
      <div id="calculated-area"></div>
    </div>
    <div class="save" @click="upDateMapData(descDataIDs)">保存</div>
    <p
      style="
        background: white;
        position: fixed;
        left: 0;
        bottom: 0;
        width: 500px;
        height: 300px;
        display: none;
      "
    >
      {{ descDataIDs }}---{{ descMapData }}---{{ descPloygon }}
    </p>
  </div>
</template>

<script>
import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";
import "mapbox-gl-draw/dist/mapbox-gl-draw.css";
import MapboxDraw from "mapbox-gl-draw";
import turf from "turf/turf";
import axios from "axios";

export default {
  data() {
    return {
      list: [],
      draw: null,
      // mapDataString:
      mapDataCon: "",
      featureIds: [],
    };
  },
  props: ["mapData", "dataIDs"],

  mounted() {
    mapboxgl.accessToken =
      "pk.eyJ1IjoieGlhb3NoaXRvdTEiLCJhIjoiY2tpNnF4NnV2MDFodjJ5cGdtNzF2enpkaiJ9.4bssGVafNq1pfM818m0jHA";
    var map = new mapboxgl.Map({
      container: "map", // container id
      style: "mapbox://styles/mapbox/streets-zh-v1", //hosted style id
      // center: [-91.874, 42.76], // starting position
      // center:[116.20,39.56],
      center: [116.4, 39.9],
      zoom: 12, // starting zoom
    });

    this.draw = new MapboxDraw({
      displayControlsDefault: false,
      controls: {
        polygon: true,
        trash: true,
      },
    });
    map.addControl(this.draw);

    /* var feature = { type: 'Point', coordinates: [116.40, 39.90] };
		var featureIds = draw.add(feature);
		console.log(featureIds); */

    /* var feature = {
			geometry: {coordinates: Array(1), type: "Polygon"}
			id: "a1e629b9245f38b04c868749dcdaafb8"
			properties: {}
			type: "Feature"
		};
		var featureIds = draw.add(feature)
		console.log(featureIds); */
    // var feature = {
    //   id: "3384bf8ba11d11b4eb6c6bfa50a861ba",
    //   type: "Feature",
    //   properties: {},
    //   geometry: {
    //     coordinates: [
    //       [
    //         [116.38858451842788, 39.92073844940944],
    //         [116.38927116393643, 39.907703585565855],
    //         [116.40781059264708, 39.90849364793152],
    //         [116.38858451842788, 39.92073844940944],
    //       ]/* ,
    //       [
    //         [117.38858451842788, 39.92073844940944],
    //         [118.38927116393643, 39.907703585565855],
    //         [119.40781059264708, 39.90849364793152],
    //         [117.38858451842788, 39.92073844940944],
    //       ],
    //       [
    //         [117.38858451842788, 39.92073844940944],
    //         [118.38927116393643, 39.907703585565855],
    //         [119.40781059264708, 39.90849364793152],
    //         [113.38858451842788, 39.92073844940944],
    //         [117.38858451842788, 39.92073844940944],
    //       ], */
    //     ],
    //     type: "Polygon",
    //   },
    // };

    setTimeout(() => {
      // var feature = {
      //   id: "3384bf8ba11d11b4eb6c6bfa50a861ba",
      //   type: "Feature",
      //   properties: {},
      //   geometry: {
      //     coordinates: [
      //       /* [
      //         [116.38858451842788, 39.92073844940944],
      //         [116.38927116393643, 39.907703585565855],
      //         [116.40781059264708, 39.90849364793152],
      //         [116.38858451842788, 39.92073844940944],
      //       ]  */
      //       this.mapData,
      //       /* ,
      //     [
      //       [117.38858451842788, 39.92073844940944],
      //       [118.38927116393643, 39.907703585565855],
      //       [119.40781059264708, 39.90849364793152],
      //       [117.38858451842788, 39.92073844940944],
      //     ],
      //     [
      //       [117.38858451842788, 39.92073844940944],
      //       [118.38927116393643, 39.907703585565855],
      //       [119.40781059264708, 39.90849364793152],
      //       [113.38858451842788, 39.92073844940944],
      //       [117.38858451842788, 39.92073844940944],
      //     ], */
      //     ],
      //     type: "Polygon",
      //   },
      // };
      // this.featureIds = this.draw.add(this.mapData)
      // this.featureIds.push(this.draw.add(this.mapData))
      /* for(let i of JSON.parse(this.mapData)) {
        this.featureIds.push(this.draw.add(i))
      } */
      // console.log(JSON.parse(this.mapData));
      // console.log(this.mapData);
      // var featureIds = this.draw.add(feature);
      // console.log(this.featureIds);
      // console.log(feature);
    }, 2000);
    // debugger
    map.on("draw.create", this.updateArea);
    map.on("draw.delete", this.updateArea);
    map.on("draw.update", this.updateArea);

    // this.btn_mapbox_click();

    // console.log(this.mapData);
    // this.getMapDataByIds()

    // console.log(this.dataIDs);
    // console.log(this.dataID);
    // console.log(this.descDataIDs);

    //mounted
  },

  computed: {
    descDataIDs() {
      // console.log(2);
      return this.dataIDs;
      // console.log(this.dataIDs);
    },
    descMapData() {
      // return this.mapData;
      return console.log(this.mapData);
    },
    descPloygon() {
      // this.featureIds = []

      if (this.mapData.length != 0) {
        let mapDataS = JSON.parse(this.mapData);
        // console.log(mapDataS);
        // this.featureIds.push(this.draw.add(this.mapData))
        // this.featureIds = ''
        this.draw.deleteAll();
        mapDataS.features.forEach((element) => {
          // console.log(element)
          this.featureIds.push(this.draw.add(element));
        });
        // console.log(this.featureIds);
        // this.draw.deleteAll()

        // console.log(this.featureIds)
        /* for(let i of mapDataS.features) {
          // this.featureIds.push(i)
          // this.featureIds = this.draw.add(i)
          this.featureIds.push(this.draw.add(i))
        } */
      } else {
        
        /* let mapDataS = JSON.parse(this.mapData);
        // console.log(mapDataS);
        // this.featureIds.push(this.draw.add(this.mapData))
        // this.featureIds = ''
        this.draw.deleteAll();
        mapDataS.features.forEach((element) => {
          // console.log(element)
          this.featureIds.push(this.draw.add(element));
        }); */
        
        console.log("mapData数据为空");
        /* let mapDataS = JSON.parse(this.mapData);
        console.log(mapDataS); */
        // console.log(this.featureIds);

        // this.draw.deleteAll();
        // return 1
      }




      
      /* let mapDataS = JSON.parse(this.mapData);
      // console.log(typeof(mapDataS.features[0].id));
      if(mapDataS.features[0].id == '26629ac106cd0b3c5b20b83856fac9f3'){
          // let mapDataS = JSON.parse(this.mapData);
        
        this.draw.deleteAll();
        mapDataS.features.forEach((element) => {
          // console.log(element)
          this.featureIds.push(this.draw.add(element));
        });
      } */



      // console.log(1);
      // return this.featureIds;
      return 1;
      // return console.log(1);
    },
  },

  /*  watch: {

  } */

  methods: {
    /* getMapDataByIds() {
      this.$emit('getMapDataById')

    }, */
    updateArea(e) {
      this.mapDataCon = this.draw.getAll();
      console.log(this.mapDataCon);
      console.log(typeof this.mapDataCon);
      var answer = document.getElementById("calculated-area");
      // console.log(JSON.stringify(this.mapDataCon.features));
      if (this.mapDataCon.features.length > 0) {
        var area = turf.area(this.mapDataCon);
        // restrict to area to 2 decimal points
        var rounded_area = Math.round(area * 100) / 100;
        answer.innerHTML =
          "<p><strong>" + rounded_area + "</strong></p><p>平方米</p>";
      } else {
        answer.innerHTML = "";
        if (e.type !== "draw.delete")
          alert("Use the draw tools to draw a polygon!");
      }
    },
    /* btn_mapbox_click() {
      var btn_mapbox = document.querySelector(
        ".mapbox-gl-draw_ctrl-draw-btn mapbox-gl-draw_polygon"
      );

      btn_mapbox.onclick = function () {
        //给元素增加点击事件

        console.log(1);
      };

      // btn_mapbox.click();
    }, */

    upDateMapData(descDataIDs) {
      axios
        .post("zi/collection/api/updateMapDataById", {
          dataId: descDataIDs,
          context: this.mapDataCon,
        })
        .then((res) => {
          console.log(res);
          // console.log(dataId);
          // console.log(this.mapData);
        });
      // console.log(this.dataID);
    },

    //methods
  },
};

/* var btn_mapbox = document.querySelector(
      ".mapbox-gl-draw_ctrl-draw-btn mapbox-gl-draw_polygon"
    );

    btn_mapbox.onclick = function () {
      //给元素增加点击事件

      alert(1);
    };

    btn_mapbox.click(); */

/* window.onload = function () {
  var btn_mapbox = document.querySelector(
    ".mapbox-gl-draw_ctrl-draw-btn mapbox-gl-draw_polygon"
  );

  btn_mapbox.onclick = function () {
    //给元素增加点击事件

    alert(1);
  };

  // btn_mapbox.click();
}; */
// location.reload()
/* let shuaxin = setTimeout(() => {
  location.reload()
}, 3000);
clearTimeout(shuaxin) */
/* let shuaxin = setTimeout(() => {
  // clearTimeout(shuaxin)
  alert(2);
  location.reload();
  alert(1);
}, 3000);

window.onload = function () {
  clearTimeout(shuaxin);
}; */

/* window.onload = function() {
  // location.reload()
} */

// history.go(0)

// location.assign('')

// history.go(-1)
// history.go(+1)
</script>

<style>
.calculation-box {
  height: 75px;
  width: 150px;
  position: absolute;
  bottom: 40px;
  left: 10px;
  background-color: rgba(255, 255, 255, 0.9);
  padding: 15px;
  text-align: center;
}

p {
  font-family: "Open Sans";
  margin: 0;
  font-size: 13px;
}

#map {
  width: 80vw;
  height: 80vh;
}

.calculation-box {
  /* margin-left:200px;
  margin-top:100px; */
  position: absolute;
  left: 360px;
  /* top:850px; */
  bottom: 25px;
}

.save {
  position: absolute;
  top: 18vh;
  right: 1vh;
  padding: 8px;
  background: rgba(35, 98, 216, 0.6);
  border-radius: 10px;
  /* color:white; */
  font-weight: bolder;
}
</style>