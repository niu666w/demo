<template>
 <div id="container">
   <div id="graph"></div>
 </div>
</template>

<script>
import G6 from '@antv/g6';
import graphData from './data'
export default {
  name: 'HelloWorld',
  data () {
    return {
      
    }
  },
  mounted(){
    this.createGraph()
  },
  methods:{
    createGraph(){
      // 定义数据源
      const data = {
        nodes:[
        {id:'1',name:'节点1',label:'节点文本1',collpse:false},
        {id:'2',name:'节点2',label:'节点文本2',collpse:false},
        {id:'3',name:'节点3',label:'节点文本3',collpse:false},
        {id:'4',name:'节点4',label:'节点文本4',collpse:false},
        {id:'5',name:'节点5',label:'节点文本5',collpse:false},
        {id:'6',name:'节点6',label:'节点文本6',collpse:false},
        {id:'7',name:'节点7',label:'节点文本7',collpse:false},
        {id:'8',name:'节点8',label:'节点文本8',collpse:false},
    ],
    edges:[
        {source:'1',target:'2'},
        {source:'1',target:'3'},
        {source:'2',target:'4'},
        {source:'3',target:'5'},
        {source:'3',target:'6'},
        {source:'6',target:'7'},
    ]
      };

      // 创建 G6 图实例
      const graph = new G6.Graph({
        container: 'graph', // 指定图画布的容器 id，与第 9 行的容器对应
        // 画布宽高
        width: document.body.clientWidth,
        height: window.screen.height,
        // width: container.offsetWidth,
        // height: container.offsetHeight,
        fitView:true,
        fitViewPadding:[20],
        // 定义布局
        layout: {
          center:[500,300],
          type: 'force',
          workerEnabled: true,
          // type: 'dagre', // 布局类型
          // direction: 'LR', // 自左至右布局，可选的有 H / V / LR / RL / TB / BT
          // nodeSep: 250, // 节点之间间距
          // rankSep: 200, // 每个层级之间的间距,
          preventOverlap:true,
          nodeSize:[100,50],
          nodeSpacing:50
        },
        defaultNode:{
          type:'rect',
          size:[100,50],
          style:{
            cursor:'pointer'
          }
        },
        defaultEdge:{
          style:{
            stroke:'#09c084',
            lineWidth:3,
            cursor:'pointer'
          }
        },
        modes:{
          default: ['drag-node','click-select', 'drag-canvas',
            {
              type: 'tooltip',
              formatText(model) {
                console.log(111,model)
                return `
                <div class="ceshi">标签:${model.label}</div>
                <div class="ceshi">名称:${model.name}</div>
                `;
              },
              offset: 10,
            },
            {
              type:'edge-tooltip',
              formatText(model) {
                let sourceId=model.source
                let msg=graph.findById(sourceId)
                let content=msg._cfg.model
                console.log(222,msg)
                return `
                <div class="ceshi">标签:${content.label}</div>
                <div class="ceshi">名称:${content.name}</div>
                `;
              },
              offset: 10,
            }
          ]
        }
      });
      // 读取数据
      graph.data(data);
      // 渲染图
      graph.render();
      // graph.fitCenter()
      graph.on('tooltipchange', e => {
        if(!e.action){
          setTimeout(()=>{
            return
          },4000)
        }
      })
      // 监听鼠标进入节点事件
      graph.on('node:mouseenter', (evt) => {
        const node = evt.item;
        console.log('节点',node)
        // 激活该节点的 hover 状态
        // graph.setItemState(node, 'hover', true);
      });
      // 监听鼠标离开节点事件
      graph.on('node:mouseleave', (evt) => {
        const node = evt.item;
        // 关闭该节点的 hover 状态
        // graph.setItemState(node, 'hover', false);
      });
      graph.on('edge:mouseenter',(evt)=>{
        console.log('边',evt)
      })
    },
  }
}
</script>


<style>
 /* 提示框的样式 */
  .g6-tooltip {
    border: 1px solid #e2e2e2;
    border-radius: 4px;
    font-size: 20px;
    color: #545454;
    background-color: rgba(255, 255, 255, 0.9);
    padding: 20px;
    box-shadow: rgb(174, 174, 174) 0px 0px 10px;
  }
  .ceshi{
    display: flex;
    justify-content: flex-start;
    margin-top: 10px;
  }
</style>
