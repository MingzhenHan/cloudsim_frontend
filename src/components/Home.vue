<template>
<div>
<el-upload
  class="upload-demo"
  action="http://localhost:8080/uploadfile"
  :on-preview="handlePreview"
  :on-remove="handleRemove"
  :before-remove="beforeRemove"
  :on-success="saveName"
  multiple
  :limit="3"
  :on-exceed="handleExceed"
  :file-list="fileList">
  <el-button size="small" type="primary">上传配置文件</el-button>
</el-upload>

<!-- <el-radio-group v-model="radio">
    <el-radio :label="6">FCFS</el-radio>
    <el-radio :label="2">MINMIN</el-radio>
    <el-radio :label="1">MAXMIN</el-radio>
    <el-radio :label="7">ROUNDROBIN</el-radio>
</el-radio-group> -->
<br>
<el-button class="button" type="success" round @click="run">Start Cloud Simulation</el-button>
<div id="container"></div>
<el-table
    class="table"
    :data="tableData"
    height="600"
    border
    style="width: 80%">
    <el-table-column
      prop="jobid"
      label="Job ID"
      width="180">
    </el-table-column>
    <el-table-column
      prop="status"
      label="STATUS"
      width="180">
    </el-table-column>
    <el-table-column
      prop="vmid"
      label="VM ID">
    </el-table-column>
    <el-table-column
      prop="time"
      label="Time"
      width="180">
    </el-table-column>
    <el-table-column
      prop="starttime"
      label="Start Time"
      width="180">
    </el-table-column>
    <el-table-column
      prop="finishtime"
      label="Finish Time"
      width="180">
    </el-table-column>
  </el-table>
</div>
</template>

<script>
import G6 from '@antv/g6';
import axios from 'axios';
axios.defaults.baseURL = 'http://localhost:8082';
export default {
    data() {
      return {
        fileList: [],
        filename:"",
        radio:1,
        tableData:[],
        newdata:{}
      };
    },
    methods: {
      handleRemove(file, fileList) { 
        console.log(file, fileList);
      },
      handlePreview(file) {
        console.log(file);
      },
      handleExceed(files, fileList) {
        this.$message.warning(`当前限制选择 3 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
      },
      beforeRemove(file, fileList) {
        return this.$confirm(`确定移除 ${ file.name }？`);
      },
      saveName(response, file, fileList){
        this.filename=file.name;
      },
      async run(){
        // console.log(this.filename);
        var url= '/run';
        console.log(url);
        await axios.get(url)
          .then(response => {
          this.tableData=response.data.data;
        })
        .catch(error => {
          console.log(error);
        });
      },
      initG6(){
        G6.registerNode(
  'sql',
  {
    drawShape(cfg, group) {
      const rect = group.addShape('rect', {
        attrs: {
          x: -75,
          y: -25,
          width: 150,
          height: 50,
          radius: 10,
          stroke: '#5B8FF9',
          fill: '#C6E5FF',
          lineWidth: 3,
        },
        name: 'rect-shape',
      });
      if (cfg.name) {
        group.addShape('text', {
          attrs: {
            text: cfg.name,
            x: 0,
            y: 0,
            fill: '#00287E',
            fontSize: 14,
            textAlign: 'center',
            textBaseline: 'middle',
            fontWeight: 'bold',
          },
          name: 'text-shape',
        });
      }
      return rect;
    },
  },
  'single-node',
);

const container = document.getElementById('container');
const width = container.scrollWidth;
const height = container.scrollHeight || 500;
const graph = new G6.Graph({
  container: 'container',
  width,
  height :675,
  layout: {
    type: 'dagre',
    nodesepFunc: (d) => {
      if (d.id === '3') {
        return 500;
      }
      return 50;
    },
    ranksep: 70,
    controlPoints: true,
  },
  defaultNode: {
    type: 'sql',
  },
  defaultEdge: {
    type: 'polyline',
    style: {
      radius: 20,
      offset: 45,
      endArrow: true,
      lineWidth: 2,
      stroke: '#C2C8D5',
    },
  },
  nodeStateStyles: {
    selected: {
      stroke: '#d9d9d9',
      fill: '#5394ef',
    },
  },
  modes: {
    default: [
      'drag-canvas',
      'zoom-canvas',
      'click-select',
      {
        type: 'tooltip',
        formatText(model) {
          const cfg = model.conf;
          const text = [];
          cfg.forEach((row) => {
            text.push(row.label + ':' + row.value + '<br>');
          });
          return text.join('\n');
        },
        offset: 5
      },
    ],
  },
  fitView: true,
});
console.log("qing");
console.log(this.newdata);
graph.data(this.newdata);
graph.render();

if (typeof window !== 'undefined')
  window.onresize = () => {
    if (!graph || graph.get('destroyed')) return;
    if (!container || !container.scrollWidth || !container.scrollHeight) return;
    graph.changeSize(container.scrollWidth, container.scrollHeight);
  };
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.upload-demo {
  margin-bottom: 20px;
}
.button{
  margin-top: 20px;
}
#container {
  position: relative;
}
.g6-tooltip {
    border-radius: 6px;
    font-size: 12px;
    color: #fff;
    background-color: #000;
    padding: 2px 8px;
    text-align: center;
}
.table {
  margin-left: 135px;
  margin-top: 25px;
}
</style>