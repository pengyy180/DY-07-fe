<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
          <!-- <a-col :xl="3" :lg="6" :md="8" :sm="24">
            <a-form-item label="类型">
              <j-dict-select-tag placeholder="请选择类型" v-model="queryParam.accountType" dictCode="ps_pay_log_type"/>
            </a-form-item>
          </a-col> -->
          <a-col :xl="3" :lg="6" :md="8" :sm="24">
            <a-form-item label="审核状态">
              <j-dict-select-tag placeholder="请选择审核状态" v-model="queryParam.accountStatus" dictCode="ps_pay_log_status"/>
            </a-form-item>
          </a-col>
          
          <a-col :xl="3" :lg="6" :md="8" :sm="24">
            <a-form-item label="客户名称">
              <a-input placeholder="请输入客户名称" v-model="queryParam.customName"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="选择客户">
              <j-dict-select-tag placeholder="请选择客户" v-model="queryParam.customId" dictCode="ps_custom_main,custom_name,id"/>
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
    <!-- <div class="table-operator">
      <a-button @click="handleAdd" type="primary" icon="plus">新增</a-button>
      <a-button type="primary" icon="download" @click="handleExportXls('资金列表')">导出</a-button>
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

        <span slot="action" slot-scope="text, record"  >
          <!-- <a @click="handleEdit(record)">编辑</a> -->
             <a @click="copydate(record)">复制</a>
                <a-divider v-if="record.accountStatus == 0" type="vertical" />

                <a v-if="record.accountStatus == 0" @click="details = record,visible=true">审核</a>
             
                <a-divider type="vertical" />
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
          <!-- <a-divider type="vertical" />
          <a-dropdown>
            <a class="ant-dropdown-link">更多 <a-icon type="down" /></a>
            <a-menu slot="overlay">
              <a-menu-item>
                <a @click="handleDetail(record)">详情</a>
              </a-menu-item>
              <a-menu-item>
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>
            </a-menu>
          </a-dropdown> -->
        </span>

      </a-table>
    </div>
       <a-modal
    title="审核状态"
    :width="800"
    :visible="visible"
    :maskClosable="false"
    @ok="handleOk"
    @cancel="handleCancel"
    cancelText="关闭">


 <a-form-model-item label="返利状态" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="accountStatus">
              <j-dict-select-tag type="list" v-model="details.accountStatus" dictCode="ps_pay_log_status" placeholder="请选择状态" />
            </a-form-model-item>

    </a-modal>
    <ps-pay-log-modal ref="modalForm" @ok="modalFormOk"></ps-pay-log-modal>
  </a-card>
</template>

<script>

  import '@/assets/less/TableExpand.less'
  import { mixinDevice } from '@/utils/mixin'
  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import PsPayLogModal from './modules/PsPayLogModal'
  import {filterMultiDictText} from '@/components/dict/JDictSelectUtil'
  import { httpAction,getAction } from '@/api/manage'

  export default {
    name: 'PsPayLogList',
    mixins:[JeecgListMixin, mixinDevice],
    components: {
      PsPayLogModal
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
        details:'',
        visible:false,
        description: '资金列表管理页面',
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
            title:'类型',
            align:"center",
            dataIndex: 'accountType_dictText'
          },
          // {
          //   title:'客户名',
          //   align:"center",
          //   dataIndex: 'customId_dictText'
          // },
          // {
          //   title:'持卡人',
          //   align:"center",
          //   dataIndex: 'bankCustomName'
          // },
          {
            title:'客户名',
            align:"center",
            dataIndex: 'customName'
          },
         
          {
            title:'银行名称',
            align:"center",
            dataIndex: 'bankName'
          },
          {
            title:'开户行',
            align:"center",
            dataIndex: 'bankNameDesc'
          },
          {
            title:'卡号',
            align:"center",
            dataIndex: 'bankNum'
          },
          {
            title:'金额',
            align:"center",
            dataIndex: 'accountBigdec'
          },
          {
            title:'标题',
            align:"center",
            dataIndex: 'accountTitle'
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
            title:'提现时间',
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
          list: "/module/psPayLog/list?accountType=2",
          delete: "/module/psPayLog/delete",
          deleteBatch: "/module/psPayLog/deleteBatch",
          exportXlsUrl: "/module/psPayLog/exportXls",
          importExcelUrl: "module/psPayLog/importExcel",

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
      //复制信息
      copydate(data){
         getAction('/bg/zijin/fuzhi',{id:data.id}).then((res)=>{
                console.log(res.result,'新学校')


let copytext = 
    '用户名字:'+res.result.psCustomMain.customName+
    '\r\n手机帐号:'+res.result.psCustomMain.customPhoneNum+
    '\r\n————————'+
    '\r\n银行名称:'+res.result.psPayLog.bankName+
    '\r\n开户行:'+res.result.psPayLog.bankNameDesc+
    '\r\n银行卡号:'+res.result.psPayLog.bankNum+
    '\r\n持卡人:'+res.result.psPayLog.bankCustomName+
    '\r\n提款金额:'+res.result.psPayLog.accountBigdec+
    '\r\n————————'+
    '\r\n用户ID:'+data.customId+
    '\r\n记录ID:'+data.id
  


                this.copy(copytext)

        })
      },
      copy(codeData){
       const textarea = document.createElement('textarea'); // 直接构建textarea  「注意：这里为了实现换行，需要创建textarea，如果用input的话，实现不了换行。」
       textarea.value = codeData; // 设置内容    「注意： \r\n 是 换行 符号」
       document.body.appendChild(textarea); // 添加临时实例
       textarea.select(); // 选择实例内容
       document.execCommand('Copy'); // 执行复制
       document.body.removeChild(textarea); // 删除临时实例
                  this.$message.success('复制成功')


},
     
      handleOk(){

        let data = {
          accountStatus:this.details.accountStatus,
          logId:this.details.id
        }
        httpAction('/bg/zijin/zijishenhe',data,'post').then((res)=>{
                if(res.success == true){
                  this.$message.success('操作成功')
                }else{
                  this.$message.error('操作失败')
                }
                setTimeout(()=>{
                  this.visible = false

                  this.loadData(1)
    // this.searchReset();

                },1000)
        })
      },
      handleCancel(){
        this.visible = false

      },
      initDictConfig(){
      },
      getSuperFieldList(){
        let fieldList=[];
        fieldList.push({type:'int',value:'accountType',text:'类型',dictCode:'ps_pay_log_type'})
        fieldList.push({type:'string',value:'customId',text:'客户',dictCode:"ps_custom_main,custom_name,id"})
        fieldList.push({type:'string',value:'accountTitle',text:'标题',dictCode:''})
        fieldList.push({type:'BigDecimal',value:'accountBigdec',text:'金额',dictCode:''})
        fieldList.push({type:'Text',value:'accountDesc',text:'描述',dictCode:''})
        fieldList.push({type:'Text',value:'imgs',text:'图片',dictCode:''})
        fieldList.push({type:'int',value:'accountStatus',text:'审核状态',dictCode:'ps_pay_log_status'})
               fieldList.push({type:'string',value:'customName',text:'客户名称',dictCode:''})

        fieldList.push({type:'Text',value:'remark',text:'备注',dictCode:''})
        this.superFieldList = fieldList
      }
    }
  }
</script>
<style scoped>
  @import '~@assets/less/common.less';
</style>