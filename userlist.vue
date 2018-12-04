<template>
  <div>
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="48">
          <a-col :md="8" :sm="24">
            <a-form-item label="部门" :labelCol="{ span: 5 }" :wrapperCol="{ span: 12 }">
              <a-tree-select showSearch style="width:100%" :value="value" :dropdownStyle="{ maxHeight: '400px', overflow: 'auto' }" placeholder='Please select' allowClear treeDefaultExpandAll @change="onChange">
                <a-tree-select-node value='parent 1' title='parent 1' key='0-1'>
                  <a-tree-select-node value='parent 1-0' title='parent 1-0' key='0-1-1'>
                    <a-tree-select-node value='leaf1' title='my leaf' key='random' />
                    <a-tree-select-node value='leaf2' title='your leaf' key='random1' />
                  </a-tree-select-node>
                  <a-tree-select-node value='parent 1-1' title='parent 1-1' key='random2'>
                    <a-tree-select-node value='sss' key='random3'>
                      <b style="color: #08c" slot="title">sss</b>
                    </a-tree-select-node>
                  </a-tree-select-node>
                </a-tree-select-node>
              </a-tree-select>
            </a-form-item>
          </a-col>
          <a-col :md="8" :sm="24">
            <a-form-item label="在职状态" :labelCol="{ span: 5 }" :wrapperCol="{ span: 12 }">
              <a-select placeholder="请选择" default-value="0">
                <a-select-option value="0">全部</a-select-option>
                <a-select-option value="1">关闭</a-select-option>
                <a-select-option value="2">运行中</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <template v-if="advanced">
            <a-col :md="8" :sm="24">
              <a-form-item label="离职类型" :labelCol="{ span: 5 }" :wrapperCol="{ span: 12 }">
                <a-select placeholder="请选择" default-value="0">
                  <a-select-option value="0">全部</a-select-option>
                  <a-select-option value="1">关闭</a-select-option>
                  <a-select-option value="2">运行中</a-select-option>
                </a-select>
              </a-form-item>
            </a-col>
            <a-col :md="8" :sm="24">
              <a-form-item label="入职时间段" :labelCol="{ span: 5 }" :wrapperCol="{ span: 12 }">
                <!--<a-range-picker @change="onChange" style="width: 100%;" v-model='time' format='YYYY-MM-DD' />-->
                <attd-date-picker :show.sync="time"></attd-date-picker>
              </a-form-item>
            </a-col>
            <a-col :md="8" :sm="24">
              <a-form-item label="离职时间段" :labelCol="{ span: 5 }" :wrapperCol="{ span: 12 }">
                <a-range-picker @change="onChange" style="width: 100%;" />
              </a-form-item>
            </a-col>
            <a-col :md="8" :sm="24">
              <a-form-item label="姓名" :labelCol="{ span: 5 }" :wrapperCol="{ span: 12 }">
                <a-input placeholder="请输入" />
              </a-form-item>
            </a-col>
            <a-col :md="8" :sm="24">
              <a-form-item label="手机号码" :labelCol="{ span: 5 }" :wrapperCol="{ span: 12 }">
                <a-input placeholder="请输入" />
              </a-form-item>
            </a-col>
            <a-col :md="8" :sm="24">
              <a-form-item label="卡号" :labelCol="{ span: 5 }" :wrapperCol="{ span: 12 }">
                <a-input placeholder="请输入" />
              </a-form-item>
            </a-col>
          </template>
          <a-col :md="8" :sm="24">
            <span class="table-page-search-submitButtons" :style="advanced && { float: 'right', overflow: 'hidden' } || {} ">
              <a-button type="primary">查询</a-button>
              <a-button style="margin-left: 8px">重置</a-button>
              <a @click="toggleAdvanced" style="margin-left: 8px">
                {{ advanced ? '收起' : '展开' }}
                <a-icon :type="advanced ? 'up' : 'down'"/>
              </a>
            </span>
          </a-col>
        </a-row>

      </a-form>

      <a-modal title="Title" :visible="visible" :width="800">
        <a-form :autoFormCreate="(form)=>{this.form = form}">

          <a-form-item label="备注" :labelCol="{span: 4}" :wrapperCol="{span: 20}">
            <a-textarea rows="4" placeholder="" />
          </a-form-item>

          <a-form-item label="请输入" :labelCol="{span:4}" :wrapperCol="{span: 20}">
            <a-input placeholder="" />
          </a-form-item>

          </a-form-item>
       
          <a-form-item label="备注" :labelCol="{span: 4}" :wrapperCol="{span: 20}" fieldDecoratorId="myabctime" >
            <a-date-picker size="large" />
          </a-form-item>
        </a-form>
      </a-modal>
      <div @click="onshow">1222</div>
      <!--<a-date-picker size="large" v-model="time" />-->
    </div>
  </div>
</template>

<script>
  import TagsView from '@/components/TagsView'
  import attdDatePicker from '@/components/attdDatePicker'
  //import DatePicker from 'ant-design-vue/lib/date-picker'
  // import ARangePicker from 'ant-design-vue/lib/date-picker/RangePicker'
  import {
    mapGetters
  } from 'vuex'
  export default {
    name: "GlobalView",
    data() {
      return {
        value: undefined,
        advanced: false,
        dateFormat: 'YYYY/MM/DD',
        time: [],
        visible: false,
        time: ''
        

      }
    },
    components: {
      TagsView,
      attdDatePicker
      //    'ARangePicker':ARangePicker
    },
    created() {
      this.mytime = moment('2015/01/01', this.dateFormat);
    /*  console.log(moment(new Date("Aa")).format(""))
      moment.updateLocale(moment.locale(), { invalidDate: "Invalid Date Updated" })
console.log(moment(new Date("Aa")).format(""));*/
    },
    methods: {
      onChange(value) {
        console.log(arguments)
        this.value = value
      },
      toggleAdvanced() {
        this.advanced = !this.advanced
      },
      onshow() {
        this.visible = true;
        this.$nextTick(() => {
          this.form.setFieldsValue({
            myabctime:moment('',this.dateFormat)
          })
        })
      }
    },

  }
</script>

<style scoped lang="scss">
  .ant-form-inline .ant-form-item {
    width: 100%;
    display: flex;
    /deep/ .ant-form-item-control-wrapper {
      flex: 1;
    }
  }
</style>