<template>
  <a-card :bordered="false" :class="'cust-erp-sub-tab'">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
              <a-col :xl="3" :lg="6" :md="8" :sm="24">
              <a-form-item label="商品名称">
                <a-input placeholder="请输入商品名称" v-model="queryParam.productTitle"></a-input>
              </a-form-item>
              </a-col>
              <a-col :xl="10" :lg="11" :md="12" :sm="24">
              <a-form-item label="商品价格">
                <a-input placeholder="请输入最小值" class="query-group-cust" v-model="queryParam.productPrice_begin"></a-input>
                <span class="query-group-split-cust"></span>
                <a-input placeholder="请输入最大值" class="query-group-cust" v-model="queryParam.productPrice_end"></a-input>
              </a-form-item>
              </a-col>
          <!-- <template v-if="toggleSearchStatus">
                <a-col :xl="3" :lg="6" :md="8" :sm="24">
                <a-form-item label="开关">
                  <a-input placeholder="请输入开关" v-model="queryParam.productStatus"></a-input>
                </a-form-item>
                </a-col>
          </template> -->
            <a-col :xl="3" :lg="6" :md="8" :sm="24">
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
    <div class="table-operator" v-if="mainId">
      <a-button @click="handleAdd" type="primary" icon="plus">新增</a-button>
      <!-- <a-button type="primary" icon="download" @click="handleExportXls('商品列表')">导出</a-button>
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
      </a-dropdown> -->
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
        bordered
        rowKey="id"
        :scroll="{x:true}"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
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

        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>
          <a-divider type="vertical" />
          <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
            <a>删除</a>
          </a-popconfirm>
        </span>

      </a-table>
    </div>

    <psProductMain-modal ref="modalForm" @ok="modalFormOk" :mainId="mainId"></psProductMain-modal>
  </a-card>
</template>

<script>

  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import PsProductMainModal from './modules/PsProductMainModal'

  export default {
    name: "PsProductMainList",
    mixins:[JeecgListMixin],
    components: { PsProductMainModal },
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
            this.queryParam['productCatId'] = val
            this.loadData(1);
          }
        }
      }
    },
    data () {
      return {
        description: '商品分类管理页面',
        disableMixinCreated:true,
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
            title:'商品图片',
            align:"center",
            sorter: true,
            dataIndex: 'productImgs',
            scopedSlots: {customRender: 'imgSlot'}
          },
          {
            title:'商品分类',
            align:"center",
            sorter: true,
            dataIndex: 'productCatId_dictText',
          },
          {
            title:'排序',
            align:"center",
            dataIndex: 'orderNum'
          },
          {
            title:'置顶',
            align:"center",
            dataIndex: 'isTop',
            customRender: (text) => (!text ? "" : (text == "Y" ? "是" : "否"))
          },
          {
            title:'商品名称',
            align:"center",
            sorter: true,
            dataIndex: 'productTitle'
          },
          {
            title:'商品价格',
            align:"center",
            sorter: true,
            dataIndex: 'productPrice'
          },
          {
            title:'返利比例(%)',
            align:"center",
            dataIndex: 'propAccount'
          },
          {
            title:'开关',
            align:"center",
            sorter: true,
            dataIndex: 'productStatus',
            customRender: (text) => (!text ? "" : (text == "Y" ? "是" : "否"))
          },
          {
            title: '操作',
            dataIndex: 'action',
            align:"center",
            fixed:"right",
            width:147,
            scopedSlots: { customRender: 'action' },
          }
        ],
        url: {
          list: "/module/psProductCat/listPsProductMainByMainId",
          delete: "/module/psProductCat/deletePsProductMain",
          deleteBatch: "/module/psProductCat/deleteBatchPsProductMain",
          exportXlsUrl: "/module/psProductCat/exportPsProductMain",
          importUrl: "/module/psProductCat/importPsProductMain",
        },
        dictOptions:{
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
