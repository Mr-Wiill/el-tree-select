<template>
  <div id="app">
    <!-- <h1>{{`基于Element-UI组件改造的树形选择器`}}</h1> -->
    <div class="checkout">
      <el-button @click="singleSelect">单选</el-button> <el-button @click="multipleSelect">多选</el-button>
    </div>
    <SelectTree 
      :options="list" 
      :props="props"
      :value="valueId"
      :clearable="isClearable"
      :accordion="isAccordion"
      :multiple="isMultiple"
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
      isClearable:true,
      isAccordion:false,
      isMultiple:true,
      valueId:[],      // 选项的初始ID
      props:{
        value: 'id',
        label: 'title',
        children: 'children'
      },
      list:[
        {
          id:1,
          title:'Vue.js',
          parentId:0
        },{
          id:2,
          title:'Element-UI',
          parentId:0,
          children:[
            {
              id:4,
              title:'el-select',
              parentId:2,
              children:[
                {
                  id:5,
                  title:'el-tree',
                  parentId:4
                }
              ]
            }
          ]
        },{
          id:3,
          title:'JavaScript',
          parentId:0,
          children:[
            {
              id:6,
              title:'选择器',
              parentId:3,
              children:[
                {
                  id:7,
                  title:'树形选择器',
                  parentId:6  
                },
                {
                  id:8,
                  title:'树形下拉框',
                  parentId:6  
                }
              ]
            }
          ]
        }
      ]
    };
  },
  mounted(){
    // this.singleSelect()
  },
  methods:{
    /* 单选 */
    singleSelect(){
      this.isMultiple = false
    },
    /* 多选 */
    multipleSelect(){
      this.isMultiple = true
      this.valueId = [5,6]
    },
    // 取值
    getValue(value){
      this.valueId = value
      // console.log(this.valueId);
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
.checkout{
  margin-bottom:30px;
}
</style>
