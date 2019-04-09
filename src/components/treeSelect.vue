<template>
  <el-select v-model="valuesTitle" 
    :clearable="clearable" 
    :multiple="multiple" 
    @clear="clearHandle" 
    @remove-tag="removeTag">
    <el-option :value="valuesTitle">
      <el-tree  id="tree-option"
        ref="selectTree"
        :accordion="accordion"
        :data="options"
        :props="props"
        :node-key="props.value"    
        :default-expanded-keys="defaultExpandedKey"
        @node-click="handleNodeClick">
      </el-tree>
    </el-option>
  </el-select>
</template>

<script>
import Vue from 'vue'
export default {
  name: "el-tree-select",
  props:{
    /* 配置项 */
    props:{
      type: Object,
      default:()=>{
        return {
          value:'id',             // ID字段名
          label: 'title',         // 显示名称
          children: 'children'    // 子级字段名
        }
      }
    },
    /* 选项列表数据 */
    options:{
      type: Array,        // 必须是树形结构的对象数组
      default: ()=>{ return [] }
    },
    /* 初始值 */
    value:{
      default: ()=>{ return null }
    },
    /* 可清空选项 */
    clearable:{
      type:Boolean,
      default:()=>{ return true }
    },
    /* 自动收起 */
    accordion:{
      type:Boolean,
      default:()=>{ return false }
    },
    /* 可多选 */
    multiple:{
      type:Boolean,
      default:()=>{  return false }
    }
  },
  data() {
    return {
      valuesId: this.multiple ? [] : '',     // 初始值
      valuesTitle: this.multiple ? [] : '',
      defaultExpandedKey: [],    
    }
  },
  mounted(){
    this.valuesId = this.value
    this.initHandle()
  },
  methods: {
    // 初始化值
    initHandle(){
      if (!this.valuesId) return 
      if(!this.multiple){
        this.valuesTitle = this.$refs.selectTree.getNode(this.valuesId).data.title     // 初始化显示
        this.$refs.selectTree.setCurrentKey(this.valuesId)       // 设置默认选中
        this.defaultExpandedKey = [this.valuesId]      // 设置默认展开
      } else {
        console.log('multiple')
      }
    },
    // 切换选项
    handleNodeClick(data,node,com){
      // if (!this.valuesId) return 
      this.setNodeData(data)
      this.addSelected(com)
    },
    // 设置选项
    setNodeData(data){
      if (!this.multiple) {         // 单选
        this.valuesTitle = data[this.props.label]
        this.valuesId = data[this.props.value]
        this.$emit('getValue',this.valuesId)
        this.defaultExpandedKey = []
      } else {                    // 多选
        let index = this.valuesId.indexOf(data[this.props.value])
        if (index <= -1) {
          this.valuesId.push(data[this.props.value])
          this.valuesTitle.push(data[this.props.label])
          this.$emit('getValue',this.valuesId)
        } else {
          this.valuesId.splice(index, 1)
          this.valuesTitle.splice(index, 1)
          this.$emit('getValue',this.valuesId)
        }
      }
    },
    /* 添加选中样式 */
    addSelected(com){
      this.$nextTick(()=>{
        let classList = com.$el.querySelectorAll('.el-tree-node__content')[0].classList;
        const isSelected = /is-selected/g.test(classList.value)
        if (!this.multiple) {
          this.clearSelected()
          classList.add('is-selected')
        } else {
          if (!isSelected) {
            classList.add('is-selected')
          } else {
            classList.remove('is-selected')
          }
        }
      })
    },
    /* 清空选中样式 */
    clearSelected(){
      let allNode = document.querySelector('#tree-option').querySelectorAll('.el-tree-node__content')
      allNode.forEach((element)=>{
        element.classList.remove('is-selected')
      })
    },
    // 清空选项
    clearHandle(){
      if (!this.multiple) {
        this.valuesTitle = ''
        this.valuesId = null
      } else {
        this.valuesTitle = []
        this.valuesId = []
      }
      this.defaultExpandedKey = []
      this.clearSelected()
      // this.$refs.selectTree.setCurrentKey(null)       // 设置默认选中
      this.$emit('getValue',null)
    },
    // 移除单个选项（多选）
    removeTag(node){
      console.log(node)
    }
  },
  watch:{
    value(){
      this.valuesId = this.value
    }
  }
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
 .el-tree >>>.is-selected .el-tree-node__label{
    color: #409EFF;
    font-weight: 700;
  }
  .el-tree >>>.is-selected .el-tree-node__children .el-tree-node__label{
    color:#606266;
    font-weight: normal;
  }
  .el-select >>> .el-select__tags .el-tag .el-tag__close{
    display: none;
  }
</style>
