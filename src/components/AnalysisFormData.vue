<template>
  <div class="analysis-form-data">
    <el-form ref="form" :model="formData" label-width="80px" class="user-form">

      <draggable v-model="templateData" :options="{group:{name: 'template', pull: false}, handle: '.from-item' }" class="draggable">
<!--        <transition-group>-->
          <div v-for="(item, index) in templateData" :key="item.key + index" class="form-element" :class="{'active-form-item': index === activeIndex, 'required': item.isRequired}" @click="handleItemClick(index)">
            <div class="operate-list">
              <span class="title">{{ item.label + '组件：' }}</span>
              <span class="btn">编辑</span>
              <span class="icon">|</span>
              <span class="btn" @click="handleCopyItem(item, index)">复制</span>
              <span class="icon">|</span>
              <span class="btn" @click="handleDeleteItem(index)">删除</span>
            </div>
            <el-form-item v-if="item.type === 'text'" class="from-item" :label="item.label">
              <el-input v-model="formData[item.key]" :placeholder="'请输入'+ item.label" :maxlength="item.maxLength || 20" readonly></el-input>
            </el-form-item>

            <el-form-item v-if="item.type === 'textarea'" class="from-item" :label="item.label">
              <el-input type="textarea" :rows="item.rows" v-model="formData[item.key]" :placeholder="'请输入'+ item.label" :maxlength="item.maxLength || 100" readonly></el-input>
            </el-form-item>

            <el-form-item v-if="item.type === 'number'" class="from-item" :label="item.label">
              <el-input type="number" v-model="formData[item.key]" :placeholder="item.placeholder" :maxlength="item.maxLength || 100" readonly></el-input>
            </el-form-item>

            <el-form-item v-if="item.type === 'select'" class="from-item" :label="item.label">
              <el-select v-model="formData[item.key]" :placeholder="'请输入'+ item.label" disabled>
                <el-option
                    v-for="value in item.options"
                    :key="value.value"
                    :label="value.label"
                    :value="value.value"
                    disabled
                >
                </el-option>
              </el-select>
            </el-form-item>

            <el-form-item v-if="item.type === 'date'" class="from-item" :label="item.label">
              <el-date-picker type="date" :placeholder="'请输入'+ item.label" v-model="formData[item.key]" style="width: 100%;" readonly></el-date-picker>
            </el-form-item>

            <el-form-item v-if="item.type === 'time'" class="from-item" :label="item.label">
              <el-date-picker type="datetime" :placeholder="'请输入'+ item.label" v-model="formData[item.key]" style="width: 100%;" readonly></el-date-picker>
            </el-form-item>

            <el-form-item v-if="item.type === 'radio'" class="from-item" :label="item.label">
              <el-radio-group v-model="formData[item.key]">
                <el-radio v-for="(value, index) in item.radioList" :key="value.label + index"  :label="value.label" :value="value.value"></el-radio>
              </el-radio-group>
            </el-form-item>

            <el-form-item v-if="item.type === 'checkbox'" class="from-item" :label="item.label" prop="type">
              <el-checkbox-group v-model="checkList">
                <el-checkbox v-for="(value, index) in item.checkboxList" :key="value.label + index" :label="value.label" :name="value.label"></el-checkbox>
              </el-checkbox-group>
            </el-form-item>

            <div class="edit-box" :class="{'active': activeIndex === index}">
              <div class="top-title">
                {{ item.typeName + '组件' }}
              </div>
              <div class="form-attribute">
                <el-form ref="ruleForm" label-width="50px" class="demo-ruleForm">
                  <el-form-item label="标题" prop="title">
                    <el-input type="input" v-model="item.label" @input="handleInput(item.label)"></el-input>
                  </el-form-item>
                  <el-form-item label="提示" prop="placeholder">
                    <el-input type="input" v-model="item.placeholder"></el-input>
                  </el-form-item>

                  <el-form-item v-if="item.type === 'radio'" class="from-item radio-item" :label="item.label">
                    <el-radio-group v-model="item.value">
                      <el-radio v-for="(value, index) in item.radioList" :key="value.label + index" :value="value.value">
                        <el-input type="input" v-model="value.label"></el-input>
                        <span class="btn" @click="item.radioList.splice(index, 1)">删除</span>
                      </el-radio>
                    </el-radio-group><br />
                    <span class="btn" @click="item.radioList.push({label: '', value: item.radioList.length})">添加项</span>
                    <span class="btn" @click="item.radioList.push({label: '其他', value: item.radioList.length})">添加其他项</span>
                  </el-form-item>

                  <el-form-item v-if="item.type === 'checkbox'" class="from-item checkbox-item" :label="item.label" prop="type">
                    <el-checkbox-group v-model="checkList">
                      <el-checkbox v-for="(value, index) in item.checkboxList" :key="value.label + index" :label="value.label" :name="value.label">
                        <el-input type="input" v-model="value.label"></el-input>
                        <span class="btn" @click="item.checkboxList.splice(index, 1)">删除</span>
                      </el-checkbox><br />
                      <span class="btn" @click="item.checkboxList.push({label: '', value: item.checkboxList.length})">添加项</span>
                      <span class="btn" @click="item.checkboxList.push({label: '其他', value: item.checkboxList.length})">添加其他项</span>
                    </el-checkbox-group>
                  </el-form-item>

                  <el-form-item label="其他">
                    <el-checkbox v-model="item.isRequired">必填</el-checkbox>
                  </el-form-item>
                  <el-form-item>
                    <div class="btn" @click.stop="handleConfirm">确定</div>
                  </el-form-item>
                </el-form>
              </div>
            </div>

          </div>
<!--        </transition-group>-->
      </draggable>
    </el-form>
  </div>
</template>

<script>
import draggable from 'vuedraggable'
export default {
  name: "AnalysisFormData",
  components: {
    draggable
  },
  props: {
    template: {
      type: Array,
      default: function () {
        return []
      }
    }
  },
  data() {
    return {
      checkList: [],
      activeIndex: null,
      templateData: JSON.parse(JSON.stringify(this.template)),
      formData: {}
    }
  },
  methods: {
    handleInput(label) {
      console.log(label)
    },
    // 点击表单元素
    handleItemClick(index) {
      this.activeIndex = index
    },
    // 复制
    handleCopyItem(item, index) {
      this.templateData.splice(index, 0, JSON.parse(JSON.stringify(item)))
    },
    // 删除
    handleDeleteItem(index) {
      this.templateData.splice(index, 1)
    },
    // 点击确定
    handleConfirm() {
      this.activeIndex = null
    }
  },
  created() {
    this.templateData = JSON.parse(JSON.stringify(this.template))
  },
  updated() {
    console.log(this.templateData)
  }
}
</script>

<style scoped lang="scss">
.analysis-form-data {
  width: 300px;
  height: 100%;
  background: pink;
  .user-form {
    widht: 100%;
    height: 100%;
    .draggable {
      width: 100%;
      height: 100%;
      cursor: pointer;
      .form-element {
        position: relative;
        //padding: 20px 0;
        &.required::after {
          position: absolute;
          left: 70px;
          top: 31px;
          display: block;
          content: '*';
          color: #e64340;
        }
        &:hover {
          border-radius: 5px;
          border: 1px dashed #166bc7;
          .operate-list {
            display: block;
          }
        }
        &.active-form-item {
          border: 1px solid #166bc7;
          .operate-list {
            display: block;
          }
        }
        .from-item {
          padding: 20px 0;
        }
        .operate-list {
          display: none;
          position: absolute;
          z-index: 1;
          right: 8px;
          top: -16px;
          align-items: center;
          padding: 4px 4px 4px 8px;
          height: 28px;
          line-height: 28px;
          background-color: #fff;
          font-size: 12px;
          font-weight: 700;
          border-radius: 4px;
          color: #333;
          box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
          overflow: hidden;
          .btn {
            color: #166bc7;
            cursor: pointer;
          }
          .icon {
            padding: 0 3px;
          }
        }
        .edit-box {
          display: none;
          position: absolute;
          right: calc(-100% - 17px);
          top: -16px;
          z-index: 1;
          border-radius: 2px;
          box-shadow: 0 1px 5px rgba(0, 0, 0, 0.2);
          &.active {
            display: block;
            //z-index: -1;
          }
          .top-title {
            position: relative;
            font-size: 14px;
            padding: 12px 24px;
            background-color: #f8f8f8;
            border-radius: 5px 5px 0 0;
            color: #333;
            font-weight: 700;
            border-bottom: 1px solid hsla(210,8%,51%,.13);
            &::before {
              display: block;
              position: absolute;
              content: " ";
              background-color: #f8f8f8;
              width: 15px;
              height: 15px;
              left: -7px;
              top: 15px;
              transform: rotate(45deg);
              z-index: -1;
              box-shadow: 0 1px 5px rgba(0, 0, 0, 0.2);
            }
          }
          .form-attribute {
            width: 258px;
            padding: 10px;
            border-top: 0;
            border-radius: 2px;
            box-shadow: 0 1px 5px rgba(0, 0, 0, 0.2);
            .demo-ruleForm {
              .radio-item,
              .checkbox-item {
                .el-input {
                  padding-right: 10px;
                }
                .btn {
                  color: #3989d4;
                  font-size: 12px;
                  background: #fff;
                  padding-right: 8px;
                }
              }
              .btn {
                width: 90px;
                height: 30px;
                line-height: 30px;
                color: #fff;
                background-color: #3989d4;
                border-color: #3989d4;
                border-radius: 5px;
                text-align: center;
              }
            }
          }
        }
        .el-radio {
          display: block;
        }
      }
    }
  }
}
.from-item {
  margin-right: 50px;
}
</style>
