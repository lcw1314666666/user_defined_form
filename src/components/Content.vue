<template>
  <div class="content">
    <div class="left-list">

      <draggable v-model="list" :options="{group:{name: 'template', pull:'clone'}, sort: false, disabled: false, ghostClass: 'active'}" :clone="handleClone">
        <transition-group>
          <div v-for="element in list" :key="element.key" class="origin-item" @click="handleItemClick(element)">
            {{ element.typeName }}
          </div>
        </transition-group>
      </draggable>

    </div>
    <div class="container">
      <AnalysisFormData :template="template"></AnalysisFormData>
    </div>
  </div>
</template>

<script>
import AnalysisFormData from "./AnalysisFormData"
import draggable from 'vuedraggable'
export default {
  name: "IContent",
  components: {
    AnalysisFormData,
    draggable
  },
  data() {
    return {
      template: [],
      list: [
        {
          key: 'name',
          type: 'text',
          typeName: '单行文本',
          label: '单行文本',
          value: '',
          isRequired: false
        },
        {
          key: 'int',
          type: 'textarea',
          typeName: '多行文本',
          label: '多行文本',
          rows: '3',
          value: '',
          isRequired: false
        },
        {
          key: 'select',
          type: 'select',
          typeName: '下拉菜单',
          label: '下拉菜单',
          options: [{
            value: '选项1',
            label: '选项1'
          }],
          value: '',
          isRequired: false
        },
        {
          key: 'data',
          type: 'date',
          typeName: '日期',
          label: '日期',
          value: '',
          isRequired: false
        },
        {
          key: 'number',
          type: 'number',
          typeName: '数字',
          label: '数字',
          value: '',
          placeholder: '请填写number',
          isRequired: false
        },
        {
          key: 'time',
          type: 'time',
          typeName: '时间',
          label: '时间',
          value: '',
          maxlength: 8,
          placeholder: '请选择tiem',
          isRequired: false
        },
        {
          key: 'radio',
          type: 'radio',
          typeName: '单选',
          label: '单选',
          isRequired: false,
          radioList: [
            {
              label: '选项一',
              value: 1
            }
          ]
        },
        {
          key: 'checkbox',
          type: 'checkbox',
          typeName: '多选',
          label: '多选',
          isRequired: false,
          checkboxList: [
            {
              label: '选项一',
              value: 1
            }
          ]
        }
      ],
      tableData: {},
      sortable: null
    }
  },
  methods: {
    handleClone(data) {
      return JSON.parse(JSON.stringify(data))
    },
    handleItemClick(element) {
      this.template.push(element)
    }
  },
  updated() {
    console.log(this.list)
  }
}
</script>

<style lang="scss">
ul,li{
  padding:0;
  margin:0;
  list-style:none
}
.content {
  position: relative;
  width: 600px;
  height: 700px;
  margin: 0 auto;
  .left-list {
    position: absolute;
    display: inline-block;
    left: 0;
    height: 100%;
    widht: 25%;
    display: flex;
    justify-content: space-around;
    align-items: center;
    .active {
      color: red;
    }
    .origin-item {
      //width: 100px;
      //height: 25px;
      //padding: 5px;
      //background: rgba(0,0,0,.12);
      border-radius: 3px;
      box-shadow: inset 0 0 5px rgba(0, 21, 41, 0.05);
      border: 1px solid #e6e6e6;

      padding: 8px 12px;
      display: -ms-flexbox;
      display: flex;
      -ms-flex-direction: row;
      flex-direction: row;
      align-items: center;
      justify-content: left;
      font-size: 14px;
      color: #333;
      cursor: pointer;
      transition: all .2s;
    }
  }
  .container {
    position: absolute;
    display: inline-block;
    right: 0;
    width: 75%;
    height: 100%;
  }
}
</style>
