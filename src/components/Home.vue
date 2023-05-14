<template>
<div>
<el-upload
  class="upload-demo"
  action="http://localhost:8080/uploadfile"
  :on-preview="handlePreview"
  :on-remove="handleRemove"
  :before-remove="beforeRemove"
  multiple
  :limit="3"
  :on-exceed="handleExceed"
  :file-list="fileList">
  <el-button size="small" type="primary">上传任务配置文件</el-button>
</el-upload>
<div id="container"></div>
</div>
</template>

<script>
import G6 from '@antv/g6';
export default {
    data() {
      return {
        fileList: [],
        newdata:{
          nodes: [
    {
      id: '1',
      dataType: 'alps',
      name: 'alps_file1',
      conf: [
        {
          label: 'conf',
          value: 'pai_graph.conf',
        },
        {
          label: 'dot',
          value: 'pai_graph.dot',
        },
        {
          label: 'init',
          value: 'init.rc',
        },
      ],
    },
    {
      id: '2',
      dataType: 'alps',
      name: 'alps_file2',
      conf: [
        {
          label: 'conf',
          value: 'pai_graph.conf',
        },
        {
          label: 'dot',
          value: 'pai_graph.dot',
        },
        {
          label: 'init',
          value: 'init.rc',
        },
      ],
    },
    {
      id: '3',
      dataType: 'alps',
      name: 'alps_file3',
      conf: [
        {
          label: 'conf',
          value: 'pai_graph.conf',
        },
        {
          label: 'dot',
          value: 'pai_graph.dot',
        },
        {
          label: 'init',
          value: 'init.rc',
        },
      ],
    },
    {
      id: '4',
      dataType: 'sql',
      name: 'sql_file1',
      conf: [
        {
          label: 'conf',
          value: 'pai_graph.conf',
        },
        {
          label: 'dot',
          value: 'pai_graph.dot',
        },
        {
          label: 'init',
          value: 'init.rc',
        },
      ],
    },
    {
      id: '5',
      dataType: 'sql',
      name: 'sql_file2',
      conf: [
        {
          label: 'conf',
          value: 'pai_graph.conf',
        },
        {
          label: 'dot',
          value: 'pai_graph.dot',
        },
        {
          label: 'init',
          value: 'init.rc',
        },
      ],
    },
    {
      id: '6',
      dataType: 'feature_etl',
      name: 'feature_etl_1',
      conf: [
        {
          label: 'conf',
          value: 'pai_graph.conf',
        },
        {
          label: 'dot',
          value: 'pai_graph.dot',
        },
        {
          label: 'init',
          value: 'init.rc',
        },
      ],
    },
    {
      id: '7',
      dataType: 'feature_etl',
      name: 'feature_etl_1',
      conf: [
        {
          label: 'conf',
          value: 'pai_graph.conf',
        },
        {
          label: 'dot',
          value: 'pai_graph.dot',
        },
        {
          label: 'init',
          value: 'init.rc',
        },
      ],
    },
    {
      id: '8',
      dataType: 'feature_extractor',
      name: 'feature_extractor',
      conf: [
        {
          label: 'conf',
          value: 'pai_graph.conf',
        },
        {
          label: 'dot',
          value: 'pai_graph.dot',
        },
        {
          label: 'init',
          value: 'init.rc',
        },
      ],
    },
  ],
  edges: [
    {
      source: '1',
      target: '2',
    },
    {
      source: '1',
      target: '3',
    },
    {
      source: '2',
      target: '4',
    },
    {
      source: '3',
      target: '4',
    },
    {
      source: '4',
      target: '5',
    },
    {
      source: '5',
      target: '6',
    },
    {
      source: '6',
      target: '7',
    },
    {
      source: '6',
      target: '8',
    },
  ]
        }
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
        offset: 30,
      },
    ],
  },
  fitView: true,
});
graph.data(this.newdata);
graph.render();

if (typeof window !== 'undefined')
  window.onresize = () => {
    if (!graph || graph.get('destroyed')) return;
    if (!container || !container.scrollWidth || !container.scrollHeight) return;
    graph.changeSize(container.scrollWidth, container.scrollHeight);
  };
      }
    },
    mounted()
    {
      this.initG6();
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>