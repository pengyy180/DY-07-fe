<template>
  <a-spin :spinning="confirmLoading">
    <j-form-container :disabled="formDisabled">
      <!-- 主表单区域 -->
      <a-form-model ref="form" :model="model" :rules="validatorRules" slot="detail">
        <a-row>
          <a-col :span="8" >
            <a-form-model-item label="创建人" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="createBy">
              <j-select-user-by-dep v-model="model.createBy" disabled/>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="创建日期" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="createTime">
              <j-date placeholder="请选择创建日期" v-model="model.createTime" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" disabled/>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="更新日期" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="updateTime">
              <j-date placeholder="请选择更新日期" v-model="model.updateTime" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" disabled/>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="头像" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="headPortrait">
              <j-image-upload isMultiple  v-model="model.headPortrait" ></j-image-upload>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="等级" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customLevel">
              <j-dict-select-tag type="list" v-model="model.customLevel" dictCode="ps_custom_main_level" placeholder="请选择等级" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="手机号码" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customPhoneNum">
              <a-input v-model="model.customPhoneNum" placeholder="请输入手机号码" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="登录密码" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customPwd">
	          <a-input-password v-model="model.customPwd" placeholder="请输入登录密码" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="客户名称" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customName">
              <a-input v-model="model.customName" placeholder="请输入客户名称" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="状态" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customStatus">
              <j-dict-select-tag type="radio" v-model="model.customStatus" dictCode="ps_custom_main_status" placeholder="请选择状态" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="跟进业务" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="followBy">
              <j-select-user-by-dep v-model="model.followBy" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="登录IP" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="loginIp">
              <a-input v-model="model.loginIp" placeholder="请输入登录IP" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="登录地址" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="loginAddress">
              <a-input v-model="model.loginAddress" placeholder="请输入登录地址" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="登录时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="loginTime">
              <j-date placeholder="请选择登录时间" v-model="model.loginTime" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="支付密码" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="payPwd">
              <a-input v-model="model.payPwd" placeholder="请输入支付密码" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="虚拟金额" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="givenAccount">
<!--              <a-input-number v-model="model.givenAccount" placeholder="请输入虚拟金额" style="width: 100%" disabled/>-->
              <a-input-number v-model="model.givenAccount" placeholder="请输入虚拟金额" style="width: 100%" disabled/>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="真实充值金额" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="realAccount">
              <a-input-number v-model="model.realAccount" placeholder="请输入真实充值金额" style="width: 100%" disabled/>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="生日" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customBirth">
              <j-date placeholder="请选择生日" v-model="model.customBirth" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="邮箱" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customEmail">
              <a-input v-model="model.customEmail" placeholder="请输入邮箱" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="性别" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customSex">
              <j-dict-select-tag type="list" v-model="model.customSex" dictCode="sex" placeholder="请选择性别" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="播放次数" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="playCount">
              <a-input-number v-model="model.playCount" placeholder="请输入播放次数" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="打码量" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="damaNum">
              <a-input-number v-model="model.damaNum" placeholder="请输入打码量" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="地址-收货人" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="addressName">
              <a-input v-model="model.addressName" placeholder="请输入地址-收货人" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="地址-电话" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="addressPhone">
              <a-input v-model="model.addressPhone" placeholder="请输入地址-电话" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="地址-省市区" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="addressProvi">
		      <j-area-linkage type="cascader" v-model="model.addressProvi" placeholder="请输入省市区" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="地址-详细地址" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="addressDetails">
              <a-textarea v-model="model.addressDetails" rows="4" placeholder="请输入地址-详细地址" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="地址-标签" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="addressTags">
              <a-input v-model="model.addressTags" placeholder="请输入地址-标签" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="银行名称" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="bankName">
              <a-input v-model="model.bankName" placeholder="请输入银行名称" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="开户行" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="bankNameDesc">
              <a-input v-model="model.bankNameDesc" placeholder="请输入银行开户行" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="银行卡号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="bankNum">
              <a-input v-model="model.bankNum" placeholder="请输入银行卡-卡号" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="持卡人" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="bankCustomName">
              <a-input v-model="model.bankCustomName" placeholder="请输入银行卡-持卡人" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8" >
            <a-form-model-item label="备注" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="remark">
              <a-textarea v-model="model.remark" rows="4" placeholder="请输入备注" />
            </a-form-model-item>
          </a-col>
        </a-row>
      </a-form-model>
    </j-form-container>
      <!-- 子表单区域 -->
    <a-tabs v-model="activeKey" @change="handleChangeTabs">
      <a-tab-pane tab="操作记录" :key="refKeys[0]" :forceRender="true">
        <j-vxe-table
          keep-source
          :ref="refKeys[0]"
          :loading="psCustomLogTable.loading"
          :columns="psCustomLogTable.columns"
          :dataSource="psCustomLogTable.dataSource"
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
    name: 'PsCustomMainForm',
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
           customLevel: [
              { required: true, message: '请输入等级!'},
           ],
           customPhoneNum: [
              { required: false},
              { validator: (rule, value, callback) => validateDuplicateValue('ps_custom_main', 'custom_phone_num', value, this.model.id, callback)},
           ],
           customPwd: [
              { required: true, message: '请输入登录密码!'},
           ],
           customStatus: [
              { required: true, message: '请输入状态!'},
           ],
           followBy: [
              { required: true, message: '请输入跟进业务!'},
           ]
          /* ,
           givenAccount: [
              { required: false},
              { pattern: /^(([1-9][0-9]*)|([0]\.\d{0,2}|[1-9][0-9]*\.\d{0,2}))$/, message: '请输入正确的金额!'},
           ],
           realAccount: [
              { required: false},
              { pattern: /^(([1-9][0-9]*)|([0]\.\d{0,2}|[1-9][0-9]*\.\d{0,2}))$/, message: '请输入正确的金额!'},
           ], */
        },
        refKeys: ['psCustomLog', ],
        tableKeys:['psCustomLog', ],
        activeKey: 'psCustomLog',
        // 操作记录
        psCustomLogTable: {
          loading: false,
          dataSource: [],
          columns: [
            {
              title: '操作人',
              key: 'createBy',
              type: JVXETypes.userSelect,
              disabled: true,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '创建日期',
              key: 'createTime',
              type: JVXETypes.datetime,
              disabled:true,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '类型',
              key: 'logType',
              type: JVXETypes.select,
              options:[],
              dictCode:"ps_custom_log_type",
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '描述',
              key: 'msg',
               type: JVXETypes.textarea,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
          ]
        },
        url: {
          add: "/module/psCustomMain/add",
          edit: "/module/psCustomMain/edit",
          queryById: "/module/psCustomMain/queryById",
          /* psCustomLog: {
            list: '/module/psCustomMain/queryPsCustomLogByMainId'
          }, */
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
        this.psCustomLogTable.dataSource=[]
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
          this.requestSubTableData(this.url.psCustomLog.list, params, this.psCustomLogTable)
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
          psCustomLogList: allValues.tablesValue[0].tableData,
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