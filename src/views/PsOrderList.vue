<template>
  <a-card :bordered="false" :class="'cust-erp-sub-tab'">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
          <a-col :xl="3" :lg="6" :md="8" :sm="24">
            <a-form-item label="客户ID">
              <a-input placeholder="请输入客户ID" v-model="queryParam.id"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="3" :lg="6" :md="8" :sm="24">
            <a-form-item label="手机号码">
              <a-input placeholder="请输入手机号码" v-model="queryParam.customPhoneNum"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="3" :lg="6" :md="8" :sm="24">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
              <a @click="handleToggleSearch" style="margin-left: 8px">
                {{ toggleSearchStatus ? '收起' : '展开' }}
                <a-icon :type="toggleSearchStatus ? 'up' : 'down'"/>
              </a>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>
    <!-- 查询区域-END -->

    <!-- 操作按钮区域 -->
    <div class="table-operator" v-if="mainId">

      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay">
          <a-menu-item key="1" @click="batchDel"><a-icon type="delete"/>删除</a-menu-item>
        </a-menu>
        <a-button style="margin-left: 8px"> 批量操作 <a-icon type="down" /></a-button>
      </a-dropdown>
    </div>

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


 <a-form-model-item label="返利状态" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="succStatus">
              <j-dict-select-tag type="list" v-model="details.succStatus" dictCode="ps_order_succ" placeholder="请选择返利状态" />
            </a-form-model-item>

    </a-modal>
    <psOrder-modal ref="modalForm" @ok="modalFormOk" :mainId="mainId"></psOrder-modal>

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

  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import PsOrderModal from './modules/PsOrderModal'
  import { httpAction,getAction } from '@/api/manage'

  export default {
    name: "PsOrderList",
    mixins:[JeecgListMixin],
    components: { PsOrderModal },
    props:{
      mainId:{
        type:String,
        default:'',
        required:false
      }
    },
    watch:{
      mainId:{
        immediate: true,
        handler(val) {
          if(!this.mainId){
            this.clearList()
          }else{
            this.queryParam['activitId'] = val
            this.loadData(1);
          }
        }
      }
    },
    data () {
      return {
        labelCol: {
          xs: { span: 24 },
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },
        orderId:'',
        copymodal:false,
        copylist:[],
        details:'',
        visible:false,
        description: '活动列表管理页面',
        disableMixinCreated:true,
        // 表头
        columns: [
          /* {
            title: '#',
            dataIndex: '',
            key:'rowIndex',
            width:60,
            align:"center",
            customRender:function (t,r,index) {
              return parseInt(index)+1;
            }
          }, */
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
          /* {
            title:'活动',
            align:"center",
            dataIndex: 'activitId'
          }, */
          /* {
            title:'活动标题',
            align:"center",
            dataIndex: 'activityTitile'
          }, */
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
            title:'下单时间',
            align:"center",
            sorter: true,
            dataIndex: 'createTime'
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
          list: "/module/psActivity/listPsOrderByMainId",
          delete: "/module/psActivity/deletePsOrder",
          deleteBatch: "/module/psActivity/deleteBatchPsOrder",
          exportXlsUrl: "/module/psActivity/exportPsOrder",
          importUrl: "/module/psActivity/importPsOrder",
        },
        dictOptions:{
         activityStatus:[],
        }
      }
    },
    created() {

    },
    computed: {
      importExcelUrl(){
        return `${window._CONFIG['domianURL']}/${this.url.importUrl}/${this.mainId}`;
      }
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
      }
      
    }
  }
</script>
<style scoped>
  @import '~@assets/less/common.less'
</style>
