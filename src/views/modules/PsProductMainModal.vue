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
            <a-form-model-item label="商品图片" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="productImgs">
              <j-image-upload isMultiple  v-model="model.productImgs"></j-image-upload>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="商品分类" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="productCatId">
              <j-dict-select-tag type="list" v-model="model.productCatId" dictCode="ps_product_cat,product_cat_name,id" placeholder="请选择商品分类" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="商品名称" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="productTitle">
              <a-input v-model="model.productTitle" placeholder="请输入商品名称" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="商品价格" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="productPrice">
              <a-input-number v-model="model.productPrice" placeholder="请输入商品价格" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="返利比例(%)" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="propAccount">
              <a-input-number v-model="model.propAccount" placeholder="请输入返利比例(%)" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="开关" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="productStatus">
              <j-switch v-model="model.productStatus" ></j-switch>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="置顶" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="isTop">
              <j-switch v-model="model.isTop" ></j-switch>
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="排序" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="orderNum">
              <a-input-number v-model="model.orderNum" placeholder="请输入排序" style="width: 100%" />
            </a-form-model-item>
          </a-col>
        
          <a-col :span="24">
            <a-form-model-item label="商品详情" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="productDescString">
              <j-editor v-model="model.productDescString" />
            </a-form-model-item>
          </a-col>

          <a-col :span="24">
            <a-form-model-item label="创建日期" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="createTime">
              <j-date placeholder="请选择创建日期" v-model="model.createTime" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24">
            <a-form-model-item label="更新日期" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="updateTime">
              <j-date placeholder="请选择更新日期" v-model="model.updateTime" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" />
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
    name: "PsProductMainModal",
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
           productImgs: [
              { required: true, message: '请输入商品图片!'},
           ],
           productCatId: [
              { required: true, message: '请输入商品分类!'},
           ],
           productTitle: [
              { required: true, message: '请输入商品名称!'},
           ],
           productPrice: [
              { required: false},
              { pattern: /^(([1-9][0-9]*)|([0]\.\d{0,2}|[1-9][0-9]*\.\d{0,2}))$/, message: '请输入正确的金额!'},
           ],
           propAccount: [
              { required: false},
              { pattern: /^(([1-9][0-9]*)|([0]\.\d{0,2}|[1-9][0-9]*\.\d{0,2}))$/, message: '请输入正确的金额!'},
           ],
           productStatus: [
              { required: true, message: '请输入开关!'},
           ],
           productDesc: [
              { required: true, message: '请输入商品详情!'},
           ],
            orderNum: [
              { required: true, message: '请输入排序!'},
              { pattern: /^-?\d+$/, message: '请输入整数!'},
           ],
           isTop: [
              { required: true, message: '请输入置顶!'},
           ],
        },
        url: {
          add: "/module/psProductCat/addPsProductMain",
          edit: "/module/psProductCat/editPsProductMain",
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
            this.model['productCatId'] = this.mainId
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
