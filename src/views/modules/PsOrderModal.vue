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
            <a-form-model-item label="客户" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customId">
              <a-input v-model="model.customId" placeholder="请输入客户" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="手机号码" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customPhoneNum">
              <a-input v-model="model.customPhoneNum" placeholder="请输入手机号码" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="活动" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="activitId">
              <a-input v-model="model.activitId" placeholder="请输入活动" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="商品" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="productId">
              <a-input v-model="model.productId" placeholder="请输入商品" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="商品返利比例(%)" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="propAccount">
              <a-input-number v-model="model.propAccount" placeholder="请输入商品返利比例(%)" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="分类返利比例(%)" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="propCatAccount">
              <a-input-number v-model="model.propCatAccount" placeholder="请输入分类返利比例(%)" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="商品图片" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="productImgs">
              <a-input v-model="model.productImgs" placeholder="请输入商品图片" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="商品名称" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="productName">
              <a-input v-model="model.productName" placeholder="请输入商品名称" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="商品价格" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="productPrice">
              <a-input-number v-model="model.productPrice" placeholder="请输入商品价格" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="商品分类" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="productCatId">
              <a-input v-model="model.productCatId" placeholder="请输入商品分类" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="最总返利金额" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="zongfanhui">
              <a-input-number v-model="model.zongfanhui" placeholder="请输入最总返利金额" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="地址-收货人" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="addressName">
              <a-input v-model="model.addressName" placeholder="请输入地址-收货人" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="地址-电话" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="addressPhone">
              <a-input v-model="model.addressPhone" placeholder="请输入地址-电话" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="地址-省市区" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="addressProvi">
              <a-input v-model="model.addressProvi"  placeholder="请输入地址-省市区" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="地址-详细地址" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="addressDetails">
              <a-input v-model="model.addressDetails" placeholder="请输入地址-详细地址" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="地址-标签" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="addressTags">
              <a-input v-model="model.addressTags" placeholder="请输入地址-标签" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24"> 
            <a-form-model-item label="订单备注" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="remark">
              <a-input v-model="model.remark" placeholder="请输入订单备注" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="订单状态" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="orderStatus">
              <j-dict-select-tag type="list" v-model="model.orderStatus" dictCode="ps_order_status" placeholder="请选择订单状态" />
            </a-form-model-item>
          </a-col>
          <!-- <a-col :span="24">
            <a-form-model-item label="返利状态" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="succStatus">
              <j-dict-select-tag type="list" v-model="model.succStatus" dictCode="ps_order_succ" placeholder="请选择返利状态" />
            </a-form-model-item>
          </a-col> -->
        </a-row>
      </a-form-model>
    </a-spin>
  </j-modal>
</template>

<script>

  import { httpAction } from '@/api/manage'
  import { validateDuplicateValue } from '@/utils/util'

  export default {
    name: "PsOrderModal",
    components: {
    },
    props:{
      mainId:{
        type:String,
        required:false,
        default:''
      }
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
        },
        url: {
          add: "/module/psActivity/addPsOrder",
          edit: "/module/psActivity/editPsOrder",
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
            this.model['activitId'] = this.mainId

            console.log(this.model,'查看')
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
