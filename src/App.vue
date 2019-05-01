<template>
  <div id="app">
    <h1>{{`基于Element-UI组件改造的树形选择器`}}</h1>
    <!-- 调用树形下拉框组件 -->
    <SelectTree 
      :props="props"
      :options="optionData"
      :value="valueId"
      :clearable="isClearable"
      :accordion="isAccordion"
      @getValue="getValue($event)"/>
  </div>
</template>

<script>
import SelectTree from "./components/treeSelect.vue";
export default {
  name: "app",
  components: {
    SelectTree
  },
  data() {
    return { 
      isClearable:true,     // 可清空（可选）
      isAccordion:true,     // 可收起（可选）
      valueId:20,            // 初始ID（可选）
      props:{               // 配置项（必选）
        value: 'id',
        label: 'name',
        children: 'children',
        // disabled:true
      },
      // 选项列表（必选）
      list:[
        {id:1,parentId:0,name:"一级菜单A",rank:1},
          {id:2,parentId:0,name:"一级菜单B",rank:1},
          {id:3,parentId:0,name:"一级菜单C",rank:1},
          {id:4,parentId:1,name:"二级菜单A-A",rank:2},
          {id:5,parentId:1,name:"二级菜单A-B",rank:2},
          {id:6,parentId:2,name:"二级菜单B-A",rank:2},
          {id:7,parentId:4,name:"三级菜单A-A-A",rank:3},
          {id:8,parentId:7,name:"四级菜单A-A-A-A",rank:4},
          {id:9,parentId:8,name:"五级菜单A-A-A-A-A",rank:5},
          {id:10,parentId:9,name:"六级菜单A-A-A-A-A-A",rank:6},
          {id:11,parentId:10,name:"七级菜单A-A-A-A-A-A-A",rank:7},
          {id:12,parentId:11,name:"八级菜单A-A-A-A-A-A-A-A",rank:8},
          {id:13,parentId:12,name:"九级菜单A-A-A-A-A-A-A-A-A",rank:9},
          {id:14,parentId:13,name:"十级菜单A-A-A-A-A-A-A-A-A-A",rank:10},
          {id:15,parentId:0,name:"一级菜单C",rank:1},
          {id:16,parentId:0,name:"一级菜单C",rank:1},
          {id:17,parentId:0,name:"一级菜单C",rank:1},
          {id:18,parentId:0,name:"一级菜单C",rank:1},
          {id:19,parentId:0,name:"一级菜单C",rank:1},
          {id:20,parentId:0,name:"一级菜单C",rank:1},
          {id:21,parentId:0,name:"一级菜单C",rank:1},
          {id:22,parentId:0,name:"一级菜单C",rank:1},
          {id:23,parentId:0,name:"一级菜单C",rank:1},
          {id:24,parentId:0,name:"一级菜单C",rank:1},
          {id:25,parentId:0,name:"一级菜单C",rank:1},
          {id:26,parentId:0,name:"一级菜单C",rank:1},
          {id:27,parentId:0,name:"一级菜单C",rank:1},
          {id:28,parentId:0,name:"一级菜单C",rank:1},
          {id:29,parentId:0,name:"一级菜单C",rank:1},
          {id:30,parentId:0,name:"一级菜单C",rank:1},
          {id:31,parentId:0,name:"一级菜单C",rank:1},
          {id:32,parentId:0,name:"一级菜单C",rank:1},
          {id:33,parentId:0,name:"一级菜单C",rank:1},
          {id:34,parentId:0,name:"一级菜单C",rank:1},
          {id:35,parentId:0,name:"一级菜单C",rank:1},
          {id:36,parentId:0,name:"一级菜单C",rank:1},
          {id:37,parentId:0,name:"一级菜单C",rank:1},
          {id:38,parentId:0,name:"一级菜单C",rank:1},
          {id:39,parentId:0,name:"一级菜单C",rank:1},
          {id:40,parentId:0,name:"一级菜单end",rank:1}
      ]
    };
  },
  computed:{
    optionData(){
      let cloneData = JSON.parse(JSON.stringify(this.list))      // 对源数据深度克隆
      return  cloneData.filter(father=>{                      // 循环所有项，并添加children属性
          let branchArr = cloneData.filter(child=>father.id == child.parentId);       // 返回每一项的子级数组
          branchArr.length>0 ? father.children=branchArr : ''   //给父级添加一个children属性，并赋值
          return father.parentId==0;      //返回第一层
      });
    }
  },
  methods:{
    // 取值
    getValue(value){
      this.valueId = value
      console.log(this.valueId);
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

</style>