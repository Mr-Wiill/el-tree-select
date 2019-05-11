<template>
  <el-select v-model="valuesTitle" 
    :clearable="clearable" 
    :multiple="multiple" 
    @clear="clearHandle"
    @remove-tag="removeTag">
    <el-option :value="valuesTitle"  class="options">
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
export default {
  name: "el-tree-select",
  props:{
    // 配置项
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
    options:{ type: Array, default: ()=>[] },             // 选项列表数据(树形结构的对象数组且必选) 
    
    // value:{ type: Number, default: ()=>null },            // 初始值（可选）
    value:{ },
    
    clearable:{ type:Boolean, default: ()=>true },        // 可清空选项（可选）
    
    accordion:{ type:Boolean, default: ()=>false },       // 自动收起（可选）
    
    multiple:{ type: Boolean, default: ()=>false }      // 多选（可选）
  },
  data() {
    return {
      valuesId: this.multiple ? [] : '',    // 初始值
      valuesTitle: this.multiple ? [] : '',
      defaultExpandedKey:[]    
    }
  },
  mounted(){
    this.initHandle()
  },
  methods: {
    // 初始化值
    initHandle(){
      if (!this.valuesId) return 
      if(!this.multiple){
        this.valuesTitle = this.$refs.selectTree.getNode(this.valuesId).data[this.props.label]     // 初始化显示
        this.$refs.selectTree.setCurrentKey(this.valuesId)       // 设置默认选中
        this.defaultExpandedKey = [this.valuesId]      // 设置默认展开
      } else {
        console.log('multiple')
      }
      this.initScroll()
    },

    // 初始化滚动条
    initScroll(){
      this.$nextTick(()=>{
        let scrollWrap = document.querySelectorAll('.el-scrollbar .el-select-dropdown__wrap')[0]
        let scrollBar = document.querySelectorAll('.el-scrollbar .el-scrollbar__bar')
        scrollWrap.style.cssText = 'margin: 0px; max-height: none; overflow: hidden;'
        scrollBar.forEach(ele => ele.style.width = 0)
      })
    },

    // 切换选项
    handleNodeClick(data,node,com){
      // this.valuesTitle = node[this.props.label]
      // this.valuesId = node[this.props.value]
      // this.$emit('getValue',this.valuesId)
      // this.defaultExpandedKey = []
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
        // console.log(this.valuesId,111111111);
        // let index = this.valuesId.length>0 ? this.valuesId.indexOf(data[this.props.value]) : -1
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

    // 添加选中样式
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

    // 清空选中样式
    clearSelected(){
      // let allNode = document.querySelectorAll('#tree-option .el-tree-node')
      // allNode.forEach((element)=>element.classList.remove('is-selected'))
      let allNode = document.querySelectorAll('#tree-option .el-tree-node__content')
      allNode.forEach((ele)=>ele.classList.remove('is-selected'))
    },

    // 清除选中
    clearHandle(){
      // this.valuesTitle = ''
      // this.valuesId = null
      if (!this.multiple) {
        this.valuesTitle = ''
        this.valuesId = null
      } else {
        this.valuesTitle = []
        this.valuesId = []
      }
      this.defaultExpandedKey = []
      this.clearSelected()
      this.$emit('getValue',null)
    },

    // 移除单个选项（多选）
    removeTag(node){
      console.log(node)
    }

  },
  watch: {
    value(){
      console.log(this.value, this.valuesId, 'watch_value');
      // this.valuesId = this.value
      this.value ? (this.valuesId = this.value) : (this.multiple ? this.valuesId = [] : this.valuesId = '')
      this.initHandle()
    }
  },
};
</script>

<style scoped>
  .el-scrollbar .el-scrollbar__view .el-select-dropdown__item{
    height: auto;
    max-height: 274px;
    padding: 0;
    overflow: hidden;
    overflow-y: auto;
  }
  .el-select-dropdown__item.selected{
    font-weight: normal;
  }
  ul li >>>.el-tree .el-tree-node__content{
    height:auto;
    padding: 0 20px;
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

  /* 开发禁用 */
  /* .el-tree-node:focus>.el-tree-node__content{
    background-color:transparent;
    background-color: #f5f7fa;
    color: #c0c4cc;
    cursor: not-allowed;
  }
  .el-tree-node__content:hover{
    background-color: #f5f7fa;
  } */
</style>
