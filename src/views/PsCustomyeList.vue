<template>
  <a-card :bordered="false" :class="'cust-erp-sub-tab'">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
        </a-row>
      </a-form>
    </div>
    <!-- 查询区域-END -->

    <!-- 操作按钮区域 -->
    <!-- <div class="table-operator" v-if="mainId">
      <a-button @click="handleAdd" type="primary" icon="plus">新增</a-button>
      <a-button type="primary" icon="download" @click="handleExportXls('操作记录')">导出</a-button>
      <a-upload
        name="file"
        :showUploadList="false"
        :multiple="false"
        :headers="tokenHeader"
        :action="importExcelUrl"
        @change="handleImportExcel">
          <a-button type="primary" icon="import">导入</a-button>
      </a-upload>
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
        bordered
        rowKey="id"
        :scroll="{x:true}"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"

        @change="handleTableChange">

        <!-- <template slot="htmlSlot" slot-scope="text">
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
        </template> -->

        <span slot="action" slot-scope="text, record">
          <!-- <a @click="handleEdit(record)">编辑</a>
          <a-divider type="vertical" /> -->
          <a-popconfirm title="确定删除吗?" @confirm="() => delorder(record)">
            <a>删除记录</a>
          </a-popconfirm>
        </span>

      </a-table>
    </div>

    <psCustomLog-modal ref="modalForm" @ok="modalFormOk" :mainId="mainId"></psCustomLog-modal>
  </a-card>
</template>

<script>

  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import PsCustomLogModal from './modules/PsCustomLogModal'
  import { getAction, deleteAction } from '@api/manage'

  export default {
    name: "PsCustomLogList",
    mixins:[JeecgListMixin],
    components: { PsCustomLogModal },
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
            this.queryParam['customId'] = val
            this.loadData(1);
          }
        }
      }
    },
    data () {
      return {
        description: '客户列表管理页面',
        disableMixinCreated:true,
        // 表头
        columns: [
          {
            title:'类型',
            align:"center",
            dataIndex: 'accountType_dictText'
          },
          {
            title:'标题',
            align:"center",
            dataIndex: 'accountTitle'
          },
          {
            title:'金额',
            align:"center",
            dataIndex: 'accountBigdec'
          },

          {
            title:'描述',
            align:"center",
            dataIndex: 'accountDesc'
          },
          // {
          //   title:'图片',
          //   align:"center",
          //   dataIndex: 'imgs' ,
          //   scopedSlots: {customRender: 'imgSlot'}

          // },
          {
            title:'审核状态',
            align:"center",
            dataIndex: 'accountStatus_dictText'
          },
          
          {
            title:'创建时间',
            align:"center",
            dataIndex: 'createTime'
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
           list: "/module/psPayLog/list",
          delete: "/module/psCustomMain/deletePsCustomLog",
          deleteBatch: "/module/psCustomMain/deleteBatchPsCustomLog",
          exportXlsUrl: "/module/psCustomMain/exportPsCustomLog",
          importUrl: "/module/psCustomMain/importPsCustomLog",
        },
        dictOptions:{
         customLevel:[],
         customStatus:[],
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
       //删除订单
      delorder(data){

        if(data.accountType == 1 || data.accountType ==2){
          deleteAction('/module/psPayLog/delete', {id:data.id}).then((res) => {
            if(res.success == true){
                this.$message.success('操作成功')
              }else{
                this.$message.error('操作失败')
              }
              setTimeout(()=>{
              this.loadData()
              },1000)
          })
        }else if(data.accountType == 3 || data.accountType ==4){
           getAction('/bg/norder/delOrder', {orderId:data.orderId}).then((res) => {
            if(res.result == true){
                this.$message.success('操作成功')
              }else{
                this.$message.error('操作失败')
              }
              setTimeout(()=>{
              this.loadData()
              },1000)
          })
        } 
       
      },
      clearList(){
        this.dataSource=[]
        this.selectedRowKeys=[]
        this.ipagination.current = 1
      }

    }
  }
</script>
<style scoped>
  @import '~@assets/less/common.less'
</style>
