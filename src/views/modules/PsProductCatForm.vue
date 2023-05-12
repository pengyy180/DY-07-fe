<template>
  <a-spin :spinning="confirmLoading">
    <j-form-container :disabled="formDisabled">
      <!-- 主表单区域 -->
      <a-form-model ref="form" :model="model" :rules="validatorRules" slot="detail">
        <a-row>
          <a-col :span="24" >
            <a-form-model-item label="封面" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="coverImg">
              <j-image-upload isMultiple  v-model="model.coverImg" ></j-image-upload>
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="分类名称" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="productCatName">
              <a-input v-model="model.productCatName" placeholder="请输入分类名称" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="开关" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="switchState">
              <j-switch v-model="model.switchState"  ></j-switch>
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="返利结果开关" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="autoCalculate">
              <j-switch v-model="model.autoCalculate"  ></j-switch>
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="反利比例(%)" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="rebatePercentage">
              <a-input-number v-model="model.rebatePercentage" placeholder="请输入反利比例(%)" style="width: 100%" />
            </a-form-model-item>
          </a-col>
        </a-row>
      </a-form-model>
    </j-form-container>
      <!-- 子表单区域 -->
    <a-tabs v-model="activeKey" @change="handleChangeTabs">
      <a-tab-pane tab="商品列表" :key="refKeys[0]" :forceRender="true">
        <j-vxe-table
          keep-source
          :ref="refKeys[0]"
          :loading="psProductMainTable.loading"
          :columns="psProductMainTable.columns"
          :dataSource="psProductMainTable.dataSource"
          :maxHeight="300"
          :disabled="formDisabled"
          :rowNumber="true"
          :rowSelection="true"
          :toolbar="true"
          />
      </a-tab-pane>
    </a-tabs>
  </a-spin>
</template>

<script>

  import { getAction } from '@/api/manage'
  import { JVxeTableModelMixin } from '@/mixins/JVxeTableModelMixin.js'
  import { JVXETypes } from '@/components/jeecg/JVxeTable'
  import { getRefPromise,VALIDATE_FAILED} from '@/components/jeecg/JVxeTable/utils/vxeUtils.js'
  import { validateDuplicateValue } from '@/utils/util'
  import JFormContainer from '@/components/jeecg/JFormContainer'

  export default {
    name: 'PsProductCatForm',
    mixins: [JVxeTableModelMixin],
    components: {
      JFormContainer,
    },
    data() {
      return {
        labelCol: {
          xs: { span: 24 },
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },
        model:{
         },
        // 新增时子表默认添加几行空数据
        addDefaultRowNum: 1,
        validatorRules: {
           productCatName: [
              { required: true, message: '请输入分类名称!'},
           ],
           switchState: [
              { required: true, message: '请输入开关!'},
           ],
           rebatePercentage: [
              { required: true, message: '请输入反利比例(%)!'},
              { pattern: /^(([1-9][0-9]*)|([0]\.\d{0,2}|[1-9][0-9]*\.\d{0,2}))$/, message: '请输入正确的金额!'},
           ],
        },
        refKeys: ['psProductMain', ],
        tableKeys:['psProductMain', ],
        activeKey: 'psProductMain',
        // 商品列表
        psProductMainTable: {
          loading: false,
          dataSource: [],
          columns: [
            {
              title: '商品图片',
              key: 'productImgs',
              type: JVXETypes.image,
              token:true,
              responseName:"message",
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
              validateRules: [{ required: true, message: '${title}不能为空' }],
            },
            {
              title: '商品分类',
              key: 'productCatId',
              type: JVXETypes.select,
              options:[],
              dictCode:"ps_product_cat,product_cat_name,id",
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
              validateRules: [{ required: true, message: '${title}不能为空' }],
            },
            {
              title: '商品名称',
              key: 'productTitle',
               type: JVXETypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
              validateRules: [{ required: true, message: '${title}不能为空' }],
            },
            {
              title: '商品价格',
              key: 'productPrice',
               type: JVXETypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
              validateRules: [{ pattern: "money", message: "${title}格式不正确" }],
            },
            {
              title: '返利比例(%)',
              key: 'propAccount',
               type: JVXETypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
              validateRules: [{ pattern: "money", message: "${title}格式不正确" }],
            },
            {
              title: '开关',
              key: 'productStatus',
              type: JVXETypes.checkbox,
                customValue: ['Y', 'N'],
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
              validateRules: [{ required: true, message: '${title}不能为空' }],
            },
            {
              title: '商品详情',
              key: 'productDescString',
               type: JVXETypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
              validateRules: [{ required: true, message: '${title}不能为空' }],
            },
            {
              title: '创建日期',
              key: 'createTime',
              type: JVXETypes.datetime,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '更新日期',
              key: 'updateTime',
              type: JVXETypes.datetime,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
          ]
        },
        url: {
          add: "/module/psProductCat/add",
          edit: "/module/psProductCat/edit",
          queryById: "/module/psProductCat/queryById",
          psProductMain: {
            list: '/module/psProductCat/queryPsProductMainByMainId'
          },
        }
      }
    },
    props: {
      //表单禁用
      disabled: {
        type: Boolean,
        default: false,
        required: false
      }
    },
    computed: {
      formDisabled(){
        return this.disabled
      },
    },
    created () {
    },
    methods: {
      addBefore(){
        this.psProductMainTable.dataSource=[]
      },
      getAllTable() {
        let values = this.tableKeys.map(key => getRefPromise(this, key))
        return Promise.all(values)
      },
      /** 调用完edit()方法之后会自动调用此方法 */
      editAfter() {
        this.$nextTick(() => {
        })
        // 加载子表数据
        if (this.model.id) {
          let params = { id: this.model.id }
          this.requestSubTableData(this.url.psProductMain.list, params, this.psProductMainTable)
        }
      },
      //校验所有一对一子表表单
        validateSubForm(allValues){
            return new Promise((resolve,reject)=>{
              Promise.all([
              ]).then(() => {
                resolve(allValues)
              }).catch(e => {
                if (e.error === VALIDATE_FAILED) {
                  // 如果有未通过表单验证的子表，就自动跳转到它所在的tab
                  this.activeKey = e.index == null ? this.activeKey : this.refKeys[e.index]
                } else {
                  console.error(e)
                }
              })
            })
        },
      /** 整理成formData */
      classifyIntoFormData(allValues) {
        let main = Object.assign(this.model, allValues.formValue)
        return {
          ...main, // 展开
          psProductMainList: allValues.tablesValue[0].tableData,
        }
      },
      validateError(msg){
        this.$message.error(msg)
      },

    }
  }
</script>

<style scoped>
</style>