<template>
  <div class="ploygon clean">
    <div class="projectList">
      <p>Project List</p>
      <ul>
        <!-- <li v-for="i in ploygonNameList2" :key="i.dataId" @click="getMapDataById($event)">{{ i }}</li> -->
        <li
          v-for="i in ploygonList"
          :key="i.dataId"
          @click="getMapDataById(i);"
        >
          <span>{{ i.name }}</span>
          <span @click="upDateMapDataName(i.id)">rename</span>

        </li>
        <!-- <li
          v-for="i in ploygonList2"
          :key="i.dataId"
          @click="getMapDataById(i)"
        >
          <span>{{ i.name }}</span>
        </li> -->
      </ul>
    </div>
    <div class="createProject">
      <div class="select">
        Please select state, city, county, school, or custom scope
      </div>
      <el-button class="el-button" type="text" @click="open"
        >Create custom scopes</el-button
      >
      <!--  <el-collapse
        v-model="activeNames"
        @change="handleChange"
        class="el-collapse-item"
      >
        <el-collapse-item name="2" class="el-collapse-item-country">
          <template slot="title" class="countryTitle" style="color: red">
            America
          </template>
          <el-collapse-item
            v-for="(i, index) in states"
            :key="i"
            :title="i"
            :name="index + 1"
          >
            <div v-for="(v, index) in counties[index]" :key="index">
              {{ v }}
            </div>
          </el-collapse-item>
        </el-collapse-item>
      </el-collapse> -->

      <el-tree
        :data="data"
        :props="defaultProps"
        @node-click="handleNodeClick"
      ></el-tree>

      <!-- <li class="mingcheng" v-for="i in ploygonNameList" :key="i">
        <div>{{ i }}</div> -->
      <!-- <button @click="del(i)">删除</button> -->
      <!-- </li> -->
    </div>
    <!-- <div> -->
      <DrawPolygons :mapData="mapDataCon" :dataIDs="dataID" ref="myChild"></DrawPolygons>
    <!-- </div> -->
  </div>
</template>

<script>
import axios from "axios";
import statesCounties from "../../static/us_states_counties.json";
import DrawPolygons from "./DrawPolygon/DrawPolygons";
export default {
  data() {
    return {
      ploygonNameList: [],
      ploygonNameList2: [],
      ploygonList: [],
      // ploygonList2: [],
      mapDataCon: "",
      mapDataCon2: "",
      mapDataCon3: [
        [116.38858451842788, 39.92073844940944],
        [116.38927116393643, 39.907703585565855],
        [116.40781059264708, 39.90849364793152],
        [116.38858451842788, 39.92073844940944],
      ],
      dataID: "",

      activeNames: ["1"],
      // state: [],
      statesCounties,
      countyTurn: {},
      states: [],
      counties: [],
      styleobj: {
        fontWeight: "bolder",
      },
      // result:'',

      data: [
        {
          label: "一级 1",
          children: [
            {
              label: "二级 1-1",
              children: [
                {
                  label: "三级 1-1-1",
                },
              ],
            },
          ],
        },
        {
          label: "一级 2",
          children: [
            {
              label: "二级 2-1",
              children: [
                {
                  label: "三级 2-1-1",
                },
              ],
            },
            {
              label: "二级 2-2",
              children: [
                {
                  label: "三级 2-2-1",
                },
              ],
            },
          ],
        },
        {
          label: "一级 3",
          children: [
            {
              label: "二级 3-1",
              children: [
                {
                  label: "三级 3-1-1",
                },
              ],
            },
            {
              label: "二级 3-2",
              children: [
                {
                  label: "三级 3-2-1",
                },
              ],
            },
          ],
        },
      ],
      defaultProps: {
        children: "children",
        label: "label",
      },
    };
  },
  // computed: {
  //   upDateMapDataName(v) {
  //     // console.log(v);
  //       this.$prompt('rename', 'Prompt', {
  //         confirmButtonText: 'save',
  //         cancelButtonText: 'cancel',
  //         // inputPattern: /[\w!#$%&'*+/=?^_`{|}~-]+(?:\.[\w!#$%&'*+/=?^_`{|}~-]+)*@(?:[\w](?:[\w-]*[\w])?\.)+[\w](?:[\w-]*[\w])?/,
  //         // inputErrorMessage: '邮箱格式不正确'
  //       }).then(({ value }) => {
  //         this.$message({
  //           type: 'success',
  //           message: 'Project name: ' + value
  //         });

  //         if(this.ploygonNameList.indexOf(value) == -1) {
  //           axios.post('/zi/collection/api/updateMapDataById',{
  //             dataId:v,
  //             name:value,
  //           })
  //           .then((res) => {
  //             console.log(res);
  //             // this.getMapDataList()
  //             this.ploygonNameList2 = this.ploygonNameList
  //             // this.$nextTick(() => {this.ploygonNameList2 = this.ploygonNameList})
  //           })
  //         } else {
  //           alert("The name already exists, please change it");
            
  //         }
  //       }).catch(() => {
  //         this.$message({
  //           type: 'info',
  //           message: 'Cancel changes'
  //         });       
  //       });
  //       return 1
  //     }
      
  
  //computed
  // },
  methods: {
    upDateMapDataName(v) {
      // console.log(v);
        this.$prompt('rename', 'Prompt', {
          confirmButtonText: 'save',
          cancelButtonText: 'cancel',
          // inputPattern: /[\w!#$%&'*+/=?^_`{|}~-]+(?:\.[\w!#$%&'*+/=?^_`{|}~-]+)*@(?:[\w](?:[\w-]*[\w])?\.)+[\w](?:[\w-]*[\w])?/,
          // inputErrorMessage: '邮箱格式不正确'
        }).then(({ value }) => {
          this.$message({
            type: 'success',
            message: 'Project name: ' + value
          });

          if(this.ploygonNameList.indexOf(value) == -1) {
            axios.post('/zi/collection/api/updateMapDataById',{
              dataId:v,
              name:value,
            })
            .then((res) => {
              console.log(res);
              this.getMapDataList()
              // this.ploygonNameList2 = this.ploygonNameList
              // this.$nextTick(() => {this.ploygonNameList2 = this.ploygonNameList})
            })
          } else {
            alert("The name already exists, please change it");
            
          }
        }).catch(() => {
          this.$message({
            type: 'info',
            message: 'Cancel changes'
          });       
        });
      },




    drawGraph() {
      /* let that = this;
      that.$nextTick(function() {
        console.log(that.$refs.myChild.DrawPolygons());
      }) */

      this.$nextTick(function() {
        this.$refs.myChild.descPloygon()
      })
      
      /* setTimeout(() => {
        // this.$refs.myChild.DrawPolygons()
        console.log(this.$refs.myChild.descPloygon());
      }, 10); */

      // console.log(1);
    },
    
    handleNodeClick(data) {
      console.log(data);
    },

    open() {
      // this.$nextTick(() => {
      //   // this.ploygonNameList2 = this.ploygonNameList/* .reverse() */; //--
      //   this.ploygonList2 = this.ploygonList;
      //   // this.getMapDataList();
      // });
      this.$prompt("Please enter the name", "Prompt", {
        confirmButtonText: "save",
        cancelButtonText: "cancel",
        // inputPattern: /[\w!#$%&'*+/=?^_`{|}~-]+(?:\.[\w!#$%&'*+/=?^_`{|}~-]+)*@(?:[\w](?:[\w-]*[\w])?\.)+[\w](?:[\w-]*[\w])?/,
        // inputErrorMessage: "邮箱格式不正确",
      })
        .then(({ value }) => {
          /* this.$message({
            type: "success",
            message: "你的项目名称是: " + value,
          }); */
          // this.ploygonNameList = value;
          // this.ploygonNameList.push(value)

          if (this.ploygonNameList.indexOf(value) == -1) {
            /* this.ploygonNameList.push(value);
            this.$message({
              type: "success",
              // message: "你的项目名称是: " + value,
              // message: confirmButton,
            }); */
            axios
              .post("/zi/collection/api/addMapData", {
                // params: {
                name: value,
                context: "",
                // }
              })
              .then((res) => {
                if (res.data.url) {
                  window.location.href = "/zi/app/login";
                }
                if (res.status == 200) {
                  this.$message({
                    type: "success",
                    message: "Project name:" + value,
                  });
                  // this.ploygonNameList.push(value);
                }
                // console.log(res);
                // console.log(JSON.parse(res.config.data).name);
                // console.log(res.config.data);
                // console.log(JSON.parse(res.config.data));
                // console.log(typeof(res.config.data));
                this.ploygonNameList.push(JSON.parse(res.config.data).name); //--
                this.ploygonList.push(JSON.parse(res.config.data));
                // console.log(this.ploygonNameList);
                // this.$nextTick(() => {
                //   // this.ploygonNameList2 = this.ploygonNameList; //--
                //   this.ploygonList2 = this.ploygonList;
                //   // this.getMapDataList();
                //   // console.log(this.ploygonList2);
                // });
              });
          } else {
            alert("The name already exists, please change it");
            // type:"error"
          }
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "取消输入",
          });
        });
    },

    /* del(todo) {
      const index = this.ploygonNameList.findIndex((i) => i.id == todo.id);
      this.ploygonNameList.splice(index, 1);
    }, */

    /* open() {
      this.$confirm("此操作将永久删除该文件, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          this.$message({
            type: "success",
            message: "删除成功!",
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除",
          });
        });
    }, */

    handleChange(val) {
      console.log(val);
    },

    /* getState() {
      axios.get('../../static/us_states_counties.json')
      .then((res) => {
        console.log(res);
      })
    } */

    /* turn() {
      console.log(1);
      this.countyTurn = this.statesCounties
      console.log(this.countyTurn);
    } */

    /* getStates() {
      for (let state in this.statesCounties) {
        this.states.push(state);
        this.counties.push(this.statesCounties[state]);
      }
      // console.log(this.states);
      console.log(this.counties);
    }, */

    /* getCounties(i) {
      for (let b in i) {
        this.counties.push(i[b]);
      }
      console.log(this.counties);
    }, */
    getMapDataList() {
        // console.log(this.featureIds);
        this.ploygonNameList = []
        this.ploygonList = []
      axios.get("/zi/collection/api/getMapDataList").then((res) => {
        if (res.data.url) {
          window.location.href = "/zi/app/login?p=/";
        }
        // console.log(res);
        // console.log(res.data);
        // console.log(res.data.dataInfo);
        // alert(res)

        for (let i of res.data.dataInfo) {
          // this.ploygonNameList = []
          // this.ploygonList = []
          this.ploygonNameList.push(i.name); //--
          this.ploygonList.push(i);
          // console.log(typeof(i));
        }
        // console.log(this.ploygonNameList);
        // this.ploygonNameList2 = this.ploygonNameList.reverse(); //--
        this.ploygonList = this.ploygonList.reverse();
        // console.log(this.ploygonList2);
        /* for(let i of this.ploygonList2) {
          console.log(i.name);
        } */
      });


      /* if(this.mapDataCon.length == 0) {
            this.mapDataCon = '{"type":"FeatureCollection","features":[{"id":"26629ac106cd0b3c5b20b83856fac9f3","type":"Feature","properties":{},"geometry":{"coordinates":[[[116.41493453979638,39.916262109191734],[116.39828338623221,39.90098768757869],[116.43175735473784,39.90164613805547],[116.41493453979638,39.916262109191734]]],"type":"Polygon"}}]}'
          } */
    },

    /* getMapDataList() {
      
    } */

    getMapDataById(i) {
      // console.log(i.id);
      // let id = i.id
      this.dataID = i.id;
      // console.log(this.dataID);
      axios
        .get("/zi/collection/api/getMapDataById?dataId=" + i.id)
        .then((res) => {
          // console.log(res);
          // console.log(typeof(res.data.dataInfo.context));
          this.mapDataCon = res.data.dataInfo.context;
          // console.log(typeof(this.mapDataCon));
          console.log(res.data.dataName);
          // console.log(this.mapDataCon);
          // console.log(this.dataID);
          // console.log(typeof(this.mapDataCon));
          /* if(this.mapDataCon.length == 0) {
            this.mapDataCon = '{"type":"FeatureCollection","features":[{"id":"26629ac106cd0b3c5b20b83856fac9f3","type":"Feature","properties":{},"geometry":{"coordinates":[[[116.41493453979638,39.916262109191734],[116.39828338623221,39.90098768757869],[116.43175735473784,39.90164613805547],[116.41493453979638,39.916262109191734]]],"type":"Polygon"}}]}'
          } */
          // console.log(this.mapDataCon);
          this.drawGraph()
        });
    },

    //这里是methods
  },

  // watch: {
  // open() {
  //   this.$prompt("请输入名称", "提示", {
  //     confirmButtonText: "保存",
  //     cancelButtonText: "取消",
  //     // inputPattern: /[\w!#$%&'*+/=?^_`{|}~-]+(?:\.[\w!#$%&'*+/=?^_`{|}~-]+)*@(?:[\w](?:[\w-]*[\w])?\.)+[\w](?:[\w-]*[\w])?/,
  //     // inputErrorMessage: "邮箱格式不正确",
  //   })
  //     .then(({ value }) => {
  //       /* this.$message({
  //         type: "success",
  //         message: "你的项目名称是: " + value,
  //       }); */
  //       // this.ploygonNameList = value;
  //       // this.ploygonNameList.push(value)
  //       if (this.ploygonNameList.indexOf(value) == -1) {
  //         /* this.ploygonNameList.push(value);
  //         this.$message({
  //           type: "success",
  //           // message: "你的项目名称是: " + value,
  //           // message: confirmButton,
  //         }); */
  //         axios
  //           .post("http://52.83.173.80/zi/collection/api/addMapData", {
  //             // params: {
  //             name: value,
  //             context: "",
  //             // }
  //           })
  //           .then((res) => {
  //             if (res.status == 200) {
  //               this.$message({
  //                 type: "success",
  //                 message: "你的项目名称是" + value,
  //               });
  //               // this.ploygonNameList.push(value);
  //             }
  //             // console.log(res);
  //           });
  //       } else {
  //         alert("名称已存在，请换一个名称");
  //         // type:"error"
  //       }
  //     })
  //     .catch(() => {
  //       this.$message({
  //         type: "info",
  //         message: "取消输入",
  //       });
  //     });
  // },

  /* getMapDataList() {
      axios
        .get("http://52.83.173.80/zi/collection/api/getMapDataList")
        .then((res) => {
          console.log(res);
          // console.log(res.data);
          console.log(res.data.dataInfo);
          // alert(res)

          for (let i of res.data.dataInfo) {
            this.ploygonNameList.push(i.name);
          }
          console.log(this.ploygonNameList);
        });
    }, */

  //watch
  // },
  mounted() {
    // this.getState()
    // this.turn()
    // console.log()
    /* for (let states in this.statesCounties) {
        console.log(states)
    } */
    // this.getStates();
    // this.getCounties();
    this.getMapDataList();
    // this.$nextTick()


  },

  components: {
    DrawPolygons,
  },
};
</script>

<style>
.el-row {
  margin-bottom: 20px;
  /* &:last-child {
    margin-bottom: 0;
  } */
}
.el-col {
  border-radius: 4px;
}
.bg-purple-dark {
  background: #99a9bf;
}
.bg-purple {
  background: #d3dce6;
}
.bg-purple-light {
  background: #e5e9f2;
}
.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
.row-bg {
  padding: 10px 0;
  background-color: #f9fafc;
}

.createProject {
  /* float:left; */
  position: absolute;
  left: 40px;
  bottom: 10px;
  /* color: white; */

  width: 17vw;
  height: 50vh;
  /* text-align: center; */
  /* margin:0 auto; */
  background: aliceblue;
}

.createProject .el-button {
  color: black;
  /* background: rgba(0, 255, 242, 0.514); */
  background: white;
  padding: 5px;
  border-radius: 10px;
  /* position:absolute; */
  /* left:0;
  right:0; */
  /* margin:0 auto; */
  /* position: relative; */
  /* left: -92px; */
  margin-top: 30px;
  width: 100%;
  font-size: 14px;
}

.drawPolygons {
  float: right;
}

.el-collapse-item-country {
  height: 40vh;
  /* background: aquamarine; */
  overflow: auto;
  /* color: ; */
  /* font-weight: bolder; */
}

.el-collapse {
  color: black;
  /* font-weight: bolder; */
}

.projectList {
  float: left;
  margin-left: 80px;
  height: 40vh;
  width: 20vh;
  background: aliceblue;
  overflow: auto;
}

.projectList ul li{
  padding-top: 5px;
  background: #99a9bf;
  border: solid 1px;
}

.projectList ul li span:first-child{
  padding-right: 15px;
}

.projectList ul li span:nth-child(2){
  background: antiquewhite;
  float: left;
}

.el-collapse-item-country .countryTitle {
  font-weight: bolder;
}
</style>