<template>
  <j-modal
    :title="title"
    :width="width"
    :visible="visible"
    :confirmLoading="confirmLoading"
    switchFullscreen
    @ok="handleOk"
    @cancel="handleCancel"
    cancelText="关闭">
    <a-spin :spinning="confirmLoading">
      <a-form-model ref="form" :model="model" :rules="validatorRules">
        <a-row>
          <a-col :span="24">
            <a-form-model-item label="活动状态" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="activityStatus">
              <j-dict-select-tag type="list" v-model="model.activityStatus" dictCode="ps_activity_status" placeholder="请选择活动状态" disabled/>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="活动期数" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="activityNper">
              <a-input-number v-model="model.activityNper" placeholder="请输入活动期数" style="width: 100%" disabled/>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="活动标题 " :labelCol="labelCol" :wrapperCol="wrapperCol" prop="activityTitile">
              <a-input v-model="model.activityTitile" placeholder="请输入活动标题 " disabled></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="活动开始时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="activityStartTime">
              <j-date placeholder="请选择活动开始时间" v-model="model.activityStartTime" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" disabled/>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="活动结束时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="activityEndTime">
              <j-date placeholder="请选择活动结束时间" v-model="model.activityEndTime" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" disabled/>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="活动结算时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="activitySettlementTime">
              <j-date placeholder="请选择活动结算时间" v-model="model.activitySettlementTime" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" disabled/>
            </a-form-model-item>
          </a-col>
        </a-row>
      </a-form-model>
    </a-spin>
  </j-modal>
</template>

<script>

  import { httpAction } from '@/api/manage'
  import { validateDuplicateValue } from '@/utils/util'

  export default {
    name: "PsActivityModal",
    components: { 
    },
    data () {
      return {
        title:"操作",
        width:800,
        visible: false,
        model:{
        },
        labelCol: {
          xs: { span: 24 },
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },

        confirmLoading: false,
        validatorRules: {
           activityStatus: [
              { required: true, message: '请输入活动状态!'},
           ],
           activityStartTime: [
              { required: true, message: '请输入活动开始时间!'},
           ],
           activityEndTime: [
              { required: true, message: '请输入活动结束时间!'},
           ],
           activitySettlementTime: [
              { required: true, message: '请输入活动结算时间!'},
           ],
        },
        url: {
          add: "/module/psActivity/add",
          edit: "/module/psActivity/edit",
        }
     
      }
    },
    created () {
    //备份model原始值
      this.modelDefault = JSON.parse(JSON.stringify(this.model));
    },
    methods: {
      add () {
        this.edit(this.modelDefault);
      },
      edit (record) {
        this.model = Object.assign({}, record);
        this.visible = true;
      },
      close () {
        this.$emit('close');
        this.visible = false;
        this.$refs.form.clearValidate();
      },
      handleOk () {
        const that = this;
        // 触发表单验证
        this.$refs.form.validate(valid => {
          if (valid) {
            that.confirmLoading = true;
            let httpurl = '';
            let method = '';
            if(!this.model.id){
              httpurl+=this.url.add;
              method = 'post';
            }else{
              httpurl+=this.url.edit;
               method = 'put';
            }
            httpAction(httpurl,this.model,method).then((res)=>{
              if(res.success){
                that.$message.success(res.message);
                that.$emit('ok');
              }else{
                that.$message.warning(res.message);
              }
            }).finally(() => {
              that.confirmLoading = false;
              that.close();
            })
          }else{
             return false
          }
        })
      },
      handleCancel () {
        this.close()
      },

      
    }
  }
</script>