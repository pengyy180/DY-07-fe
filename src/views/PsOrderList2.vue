<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="客户名称">
              <a-input placeholder="请输入客户" v-model="queryParam.customId"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="手机号码">
              <a-input placeholder="请输入手机号码" v-model="queryParam.customPhoneNum"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="活动">

              <j-dict-select-tag placeholder="请选择活动" v-model="queryParam.activitId" dictCode="ps_activity,activity_titile,id"/>
            </a-form-item>
          </a-col>
          <template v-if="toggleSearchStatus">

          </template>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
              <!-- <a @click="handleToggleSearch" style="margin-left: 8px">
                {{ toggleSearchStatus ? '收起' : '展开' }}
                <a-icon :type="toggleSearchStatus ? 'up' : 'down'"/>
              </a> -->
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>
    <!-- 查询区域-END -->

    <!-- 操作按钮区域 -->
    <!-- <div class="table-operator">
      <a-button @click="handleAdd" type="primary" icon="plus">新增</a-button>
      <a-button type="primary" icon="download" @click="handleExportXls('订单列表')">导出</a-button>
      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">
        <a-button type="primary" icon="import">导入</a-button>
      </a-upload>

      <j-super-query :fieldList="superFieldList" ref="superQueryModal" @handleSuperQuery="handleSuperQuery"></j-super-query>
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay">
          <a-menu-item key="1" @click="batchDel"><a-icon type="delete"/>删除</a-menu-item>
        </a-menu>
        <a-button style="margin-left: 8px"> 批量操作 <a-icon type="down" /></a-button>
      </a-dropdown>
    </div> -->

    <!-- table区域-begin -->
    <div>
      <!-- <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择 <a style="font-weight: 600">{{ selectedRowKeys.length }}</a>项
        <a style="margin-left: 24px" @click="onClearSelected">清空</a>
      </div> -->

      <a-table
        ref="table"
        size="middle"
        :scroll="{x:true}"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
        class="j-table-force-nowrap"
        @change="handleTableChange">
         <span slot="orderstatus" slot-scope="text, record">
          {{record.orderstatus==1?'支付成功':'已完成'}}

        </span>
        <span slot="succStatus" slot-scope="text, record">

          <span v-if="record.succStatus==0">未结算</span>
          <span v-if="record.succStatus==1">成功</span>
          <span v-if="record.succStatus==2">失败</span>

        </span>

        <template slot="htmlSlot" slot-scope="text">
          <div v-html="text"></div>
        </template>
        <template slot="imgSlot" slot-scope="text,record">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无图片</span>
          <img v-else :src="getImgView(text)" :preview="record.id" height="25px" alt="" style="max-width:80px;font-size: 12px;font-style: italic;"/>
        </template>
        <template slot="fileSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无文件</span>
          <a-button
            v-else
            :ghost="true"
            type="primary"
            icon="download"
            size="small"
            @click="downloadFile(text)">
            下载
          </a-button>
        </template>
        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>
          <a-divider type="vertical" />
          <a-dropdown>
            <a class="ant-dropdown-link">更多 <a-icon type="down" /></a>
            <a-menu slot="overlay">

              <a-menu-item>

                  <a @click="checkorder(record.id),copymodal=true">克隆订单</a>

              </a-menu-item>
              <a-menu-item v-if="record.orderStatus == 1">
                <a-popconfirm title="确定撤回订单吗?" @confirm="() => chorder(record.id)">
                  <a>撤回订单</a>
                </a-popconfirm>
              </a-menu-item>
              <a-menu-item  v-if="record.orderStatus == 2" >
                <a-popconfirm title="确定删除订单吗?" @confirm="() => delorder(record.id)">
                  <a>删除订单</a>
                </a-popconfirm>
              </a-menu-item>


               <a-menu-item  v-if="record.orderStatus == 1" @click="details = record,visible=true">
                  <a  >设置结算{{text}}</a>
              </a-menu-item>

              <a-menu-item v-if="record.orderStatus == 2 && record.succStatus == 2 " @click="chenggong(record.id)">
                  <a>失败转成功</a>
              </a-menu-item>
            </a-menu>
          </a-dropdown>
        </span>

      </a-table>
    </div>
 <a-modal
    title="设置返利状态"
    :width="800"
    :visible="visible"
    :maskClosable="false"
    @ok="handleOk"
    @cancel="handleCancel"
    cancelText="关闭">


 <a-form-model-item label="返利状态"   prop="succStatus">
              <j-dict-select-tag type="list" v-model="details.succStatus" dictCode="ps_order_succ" placeholder="请选择返利状态" />
            </a-form-model-item>

    </a-modal>

     <a-modal
    title="克隆订单"
    :width="1200"
    :visible="copymodal"
    :maskClosable="false"
    @ok="copymodal = false"
    @cancel="copymodal = false"
    cancelText="关闭">
       <a-table
        ref="table"
        size="middle"
        :scroll="{x:true}"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="copylist"
        :pagination="false"
        class="j-table-force-nowrap"
       >
        <span slot="orderstatus" slot-scope="text, record">
          {{record.orderstatus==1?'支付成功':'已完成'}}

        </span>
        <span slot="succStatus" slot-scope="text, record">

          <span v-if="record.succStatus==0">未结算</span>
          <span v-if="record.succStatus==1">成功</span>
          <span v-if="record.succStatus==2">失败</span>

        </span>
         <span slot="action" slot-scope="text, record">
          <a @click="copyapi(record)">操作-克隆订单</a>
        </span>
        </a-table>


    </a-modal>
  </a-card>
</template>

<script>

  import '@/assets/less/TableExpand.less'
  import { mixinDevice } from '@/utils/mixin'
  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import PsOrderModal from './modules/PsOrderModal'
  import {filterMultiDictText} from '@/components/dict/JDictSelectUtil'
  import { getAction, httpAction } from '@api/manage'

  export default {
    name: 'PsOrderList',
    mixins:[JeecgListMixin, mixinDevice],
    components: {
      PsOrderModal
    },
    data () {
      return {
        orderId:'',
        copymodal:false,
        copylist:[],
        details:'',
        visible:false,
        description: '订单列表管理页面',
        // 表头
        columns: [
          {
            title: '#',
            dataIndex: '',
            key:'rowIndex',
            width:60,
            align:"center",
            customRender:function (t,r,index) {
              return parseInt(index)+1;
            }
          },
          {
            title:'客户名称',
            align:"center",
            dataIndex: 'customName'
          },
          {
            title:'手机号码',
            align:"center",
            dataIndex: 'customPhoneNum'
          },
          {
            title:'活动',
            align:"center",
            dataIndex: 'activitId'
          },
          {
            title:'活动标题',
            align:"center",
            dataIndex: 'activityTitile'
          },
          {
            title:'礼品',
            align:"center",
            dataIndex: 'giftDic'
          },
          {
            title:'支付金额',
            align:"center",
            dataIndex: 'orderPrice'
          },
          {
            title:'总返利',
            align:"center",
            dataIndex: 'zongfanhui'
          },
          {
            title:'订单状态',
            align:"center",
            dataIndex: 'orderStatus_dictText',
            scopedSlots: { customRender: 'orderstatus' },

          },
          {
            title:'返利状态',
            align:"center",
            dataIndex: 'succStatus_dictText',
            scopedSlots: { customRender: 'succStatus' },

          },
          {
            title: '操作',
            dataIndex: 'action',
            align:"center",
            fixed:"right",
            width:147,
            scopedSlots: { customRender: 'action' }
          }
        ],
        url: {
          list: "/module/psOrder/list",
          delete: "/module/psOrder/delete",
          deleteBatch: "/module/psOrder/deleteBatch",
          exportXlsUrl: "/module/psOrder/exportXls",
          importExcelUrl: "module/psOrder/importExcel",

        },
        dictOptions:{},
        superFieldList:[],
      }
    },
    created() {
    this.getSuperFieldList();

    },
    computed: {
      importExcelUrl: function(){
        return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
      },
    },
    methods: {
      checkorder(id){
          this.orderId = id
         getAction('/bg/norder/findAcitOrder', {orderId:id}).then((res) => {
          this.copylist = res.result

        })
      },
      copyapi(item){
          let data ={
            orderId:this.orderId,
            targetOrderId:item.id
          }
          httpAction('bg/norder/kelongOrder',data,'post').then((res)=>{
                    if(res.success == true){
                      this.$message.success('操作成功')
                    }else{
                      this.$message.error('操作失败')
                    }
                    setTimeout(()=>{
                      this.copymodal = false
                      this.loadData()
                    },1000)
                  })
      },
      //撤回订单
      chorder(id){
        getAction('/bg/norder/withdrawOrder', {orderId:id}).then((res) => {
          if(res.result == true){
              this.$message.success('操作成功')
            }else{
              this.$message.error('操作失败')
            }
            setTimeout(()=>{
             this.loadData()
            },1000)
        })
      },
      //删除订单
      delorder(id){
        getAction('/bg/norder/delOrder', {orderId:id}).then((res) => {
          if(res.result == true){
              this.$message.success('操作成功')
            }else{
              this.$message.error('操作失败')
            }
            setTimeout(()=>{
             this.loadData()
            },1000)
        })
      },
      handleOk(){
        let data = {
          succStatus:this.details.succStatus,
          id:this.details.id
        }
        let _this = this
        httpAction('/module/psActivity/editPsOrder',data,'post').then((res)=>{
          if(res.success == true){
            this.$message.success('操作成功')

          }else{
            this.$message.error('操作失败')
          }
          setTimeout(()=>{
           this.visible = false
            this.loadData(1)
          },1000)
        })
      },
      handleCancel(){
        this.visible = false

      },
      //失败转成功
      chenggong(id){
        let _this = this
        this.$confirm({
          title: '失败转成功',
          content: '确定要失败转成功吗?',
          onOk: function () {
            getAction('/bg/norder/orderSussOk',{orderId:id}).then((res)=>{
              console.log(res)
              if(res.success == true){
                _this.$message.success('操作成功')
              }else{
                _this.$message.error('操作失败')
              }
              setTimeout(()=>{


                _this.loadData(1)
              },1000)
            })
          }
        })
      },
      //撤回
      chehui(id){
        let _this = this
        this.$confirm({
          title: '撤回订单',
          content: '确定要撤回订单吗?',
          onOk: function () {
            getAction('/bg/norder/withdrawOrder',{orderId:id}).then((res)=>{
              console.log(res)
              if(res.success == true){
                _this.$message.success('操作成功')
              }else{
                _this.$message.error('操作失败')
              }
              setTimeout(()=>{


                _this.loadData(1)
              },1000)
            })
          }
        })
      },
      clearList(){
        this.dataSource=[]
        this.selectedRowKeys=[]
        this.ipagination.current = 1
      },
      initDictConfig(){
      },
      getSuperFieldList(){
        let fieldList=[];
        fieldList.push({type:'string',value:'customId',text:'客户',dictCode:"ps_custom_main,custom_name,id"})
        fieldList.push({type:'string',value:'customPhoneNum',text:'手机号码',dictCode:''})
        fieldList.push({type:'string',value:'activitId',text:'活动',dictCode:"ps_activity,activity_titile,id"})
        fieldList.push({type:'string',value:'productId',text:'商品',dictCode:"ps_product_main,product_title,id"})
        fieldList.push({type:'BigDecimal',value:'propAccount',text:'商品返利比例(%)',dictCode:''})
        fieldList.push({type:'BigDecimal',value:'propCatAccount',text:'分类返利比例(%)',dictCode:''})
        fieldList.push({type:'Text',value:'productImgs',text:'商品图片',dictCode:''})
        fieldList.push({type:'string',value:'productName',text:'商品名称',dictCode:''})
        fieldList.push({type:'BigDecimal',value:'productPrice',text:'商品价格',dictCode:''})
        fieldList.push({type:'int',value:'productNum',text:'购买数量',dictCode:''})
        fieldList.push({type:'string',value:'productCatId',text:'商品分类',dictCode:"ps_product_cat,product_cat_name,id"})
        fieldList.push({type:'BigDecimal',value:'zongfanhui',text:'最总返利金额',dictCode:''})
        fieldList.push({type:'string',value:'addressName',text:'地址-收货人',dictCode:''})
        fieldList.push({type:'string',value:'addressPhone',text:'地址-电话',dictCode:''})
        fieldList.push({type:'string',value:'addressProvi',text:'地址-省市区',dictCode:''})
        fieldList.push({type:'string',value:'addressDetails',text:'地址-详细地址',dictCode:''})
        fieldList.push({type:'string',value:'addressTags',text:'地址-标签',dictCode:''})
        fieldList.push({type:'Text',value:'remark',text:'订单备注',dictCode:''})
        fieldList.push({type:'int',value:'orderStatus',text:'订单状态',dictCode:'ps_order_status'})
        fieldList.push({type:'int',value:'succStatus',text:'返利状态',dictCode:'ps_order_succ'})
        this.superFieldList = fieldList
      },
      loadData(arg) {
        if(!this.url.list){
          this.$message.error("请设置url.list属性!")
          return
        }
        //加载数据 若传入参数1则加载第一页的内容
        if (arg === 1) {
          this.ipagination.current = 1;
        }
        var params = this.getQueryParams();//查询条件
        this.loading = true;
        getAction(this.url.list, params).then((res) => {
          if (res.success) {
            //update-begin---author:zhangyafei    Date:20201118  for：适配不分页的数据列表------------

            for (let i = 0; i < res.result.records.length; i++) {
						
						let gift = res.result.records[i].giftDic.split("[")
						 gift = gift[1].split("]")
						 gift = gift[0].split(",")
						if(gift[0] == '1'){
							gift[0] = '玫瑰&'
						}else if(gift[0] == '2'){
							gift[0] = '糖心&'
						}else if(gift[0] == '3'){
							gift[0] = '火箭&'
						}else if(gift[0] == '4'){
							gift[0] = '烟花&'
						}
						if(gift[1] == '1'){
							gift[1] = '玫瑰&'
						}else if(gift[1] == '2'){
							gift[1] = '糖心&'
						}else if(gift[1] == '3'){
							gift[1] = '火箭&'
						}else if(gift[1] == '4'){
							gift[1] = '烟花&'
						}
            if(gift[2] == '1'){
							gift[2] = '玫瑰&'
						}else if(gift[2] == '2'){
							gift[2] = '糖心&'
						}else if(gift[2] == '3'){
							gift[2] = '火箭&'
						}else if(gift[2] == '4'){
							gift[2] = '烟花&'
						}
            if(gift[3] == '1'){
							gift[3] = '玫瑰'
						}else if(gift[3] == '2'){
							gift[3] = '糖心'
						}else if(gift[3] == '3'){
							gift[3] = '火箭'
						}else if(gift[3] == '4'){
							gift[3] = '烟花'
						}
            if(gift == ''){
              gift = '无'
            }
						 res.result.records[i].giftDic = gift
					
					}
            this.dataSource = res.result.records||res.result;


            if(res.result.total)
            {
              this.ipagination.total = res.result.total;
            }else{
              this.ipagination.total = 0;
            }
            //update-end---author:zhangyafei    Date:20201118  for：适配不分页的数据列表------------
          }else{
            this.$message.warning(res.message)
          }
        }).finally(() => {
          this.loading = false
        })
      },
    }
  }
</script>
<style scoped>
  @import '~@assets/less/common.less';
</style>