<template>
  <div>
    <el-tabs v-model="activeName" @tab-click="handleClick">
      <el-tab-pane label="家庭区位选择" name="first">
        <br>
        <el-button type="primary" @click="runHLCM">运行家庭区位选择</el-button>
        <br>
        <span>{{msgHLCM }}</span>

    </el-tab-pane>
      <el-tab-pane label="就业区位选择" name="second">
        <br>
        <el-button type="primary" @click="runELCM">运行就业区位选择</el-button>
        <br>
        <span>{{msgELCM }}</span>
      </el-tab-pane>
      <MapOlLcm :geojsonData="geojsonData"></MapOlLcm>
      <TableComponent v-if="plotData" :dataFromParent="plotData"/> 
      <!-- v-if plotData，是为了保证只有在plotData在非null值时才渲染元素 -->

    </el-tabs>
  </div>
  </template>
  <script>
    import axios from 'axios';
    import TableComponent from '../pages/TableComponent.vue'

    import 'ol/ol.css'
    import MapOlLcm from './MapOlLcm.vue';
    export default {
    data() {
        return {  
            activeName: 'first',
            householdsTable:null,
            msgHLCM :null,
            msgELCM:null,
            plotData:null,
            geojsonData:{},
        };
    },
    methods: {
        handleClick(tab, event) {
            console.log(tab, event);
        },
        runHLCM(){
            // 家庭区位选择
            console.log('运行家庭区位选择模型')
            // 向后端发送请求
            axios.post('http://localhost:5000/run/hlcm').then(response =>{
                // 处理响应
                this.householdsTable = JSON.parse(response.data.data);
                this.msgHLCM = response.data.msg;
                this.plotData = this.householdsTable 
                this.geojsonData = response.data.geojson_plot_data; 
                console.log("hlcm geojsonData",this.geojsonData)
            })
        },
        runELCM(){
            console.log('运行就业区位选择')
            axios.post('http://localhost:5000/run/elcm').then(response =>{
                this.JobsTable = JSON.parse(response.data.data);
                this.msgELCM = response.data.msg;
                this.plotData = this.JobsTable 
            })
        },
    },
    components:{
      TableComponent,
      MapOlLcm
    },

};
  </script>


<style scoped>
/* local styles */
.el-tabs{
  width: 1200px;
}
.el-tabs__nav-scroll .el-tabs__content .mapboxgl-canvas{
  height: 500px;
  width: 100%;
}

</style>
