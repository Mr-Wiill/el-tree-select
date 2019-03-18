<template>
  <el-select :value="valueTitle" clearable @clear="clearHandle">
    <el-option :value="valueTitle" :label="valueTitle">
      <el-tree  
        accordion
        ref="selectTree"
        :data="data"
        :props="defaultProps"
        :node-key="defaultProps.value"    
        :default-expanded-keys="defaultExpandedKey"
        @node-click="handleNodeClick">
      </el-tree>
    </el-option>
  </el-select>
</template>

<script>
export default {
  name: "el-select-tree",
  // props: ['data', 'defaultProps', 'value'],
  props:{
    data:{
      type: Array,
      default: ()=>{
        return []
      }
    },
    defaultProps:{
      type: Object,
      default:()=>{
        return {
          value:'id',             // ID字段名
          label: 'title',         // 显示名称
          children: 'children'    // 子级字段名
        }
      }
    },
    valueKey:{
      type: Number,
      default: ()=>{
        return null
      }
    }
  },
  data() {
    return {
      value:this.valueKey,    // 初始值
      valueTitle:'',
      defaultExpandedKey:[]    
    }
  },
  mounted(){
    this.initHandle()
  },
  methods: {
    // 初始化值
    initHandle(){
      if(this.value){
        this.valueTitle = this.$refs.selectTree.getNode(this.value).data.title     // 初始化显示
        this.$refs.selectTree.setCurrentKey(this.value)       // 设置默认选中
        this.defaultExpandedKey = [this.value]      // 设置默认展开
      } 
      // else{
      //   this.clearHandle()
      // }
    },
    // 切换选项
    handleNodeClick(node){
      this.valueTitle = node[this.defaultProps.label]
      this.value = node[this.defaultProps.value]
      this.$emit('getValue',this.value)
      this.defaultExpandedKey = []
    },
    // 清除选中
    clearHandle(){
      this.valueTitle = ''
      this.value = null
      this.defaultExpandedKey = []
      this.$refs.selectTree.setCurrentKey(null)       // 设置默认选中
      this.$emit('getValue',null)
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .el-scrollbar .el-scrollbar__view .el-select-dropdown__item{
    height: auto;
    padding: 0;
  }
  .el-select-dropdown__item.selected{
    font-weight: normal;
  }
  ul li >>>.el-tree .el-tree-node__content{
    height:auto;
  }
  .el-tree-node__label{
    font-weight: normal;
  }
  .el-tree >>>.is-current .el-tree-node__label{
    color: #409EFF;
    font-weight: 700;
  }
  .el-tree >>>.is-current .el-tree-node__children .el-tree-node__label{
    color:#606266;
    font-weight: normal;
  }
</style>
