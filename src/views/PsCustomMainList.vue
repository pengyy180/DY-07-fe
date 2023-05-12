<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">

      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
        <a-col :xl="2" :lg="4" :md="6" :sm="24">
            <a-form-item label="在线人数">
                <div style="color:green;width: 100%;">{{customnum}}人</div>

            </a-form-item>
          </a-col>
<!--          <a-col :xl="3" :lg="6" :md="8" :sm="24">
            <a-form-item label="客户ID">
              <a-input placeholder="请输入客户ID" v-model="queryParam.id"></a-input>
            </a-form-item>
          </a-col>-->
          <a-col :xl="3" :lg="6" :md="8" :sm="24">
            <a-form-item label="等级">
              <j-dict-select-tag placeholder="请选择等级" v-model="queryParam.customLevel" dictCode="ps_custom_main_level"/>
            </a-form-item>
          </a-col>
          <a-col :xl="3" :lg="6" :md="8" :sm="24">
            <a-form-item label="手机号码">
              <a-input placeholder="请输入手机号码" v-model="queryParam.customPhoneNum"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="3" :lg="6" :md="8" :sm="24">
            <a-form-item label="客户名称">
              <a-input placeholder="请输入客户名称" v-model="queryParam.customName"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="3" :lg="6" :md="8" :sm="24">
            <a-form-item label="状态">
              <j-dict-select-tag placeholder="请选择状态" v-model="queryParam.customStatus" dictCode="ps_custom_main_status"/>
            </a-form-item>
          </a-col>
          <a-col :xl="3" :lg="6" :md="8" :sm="24">
            <a-form-item label="跟进业务">
              <j-select-user-by-dep placeholder="请选择跟进业务" v-model="queryParam.followBy"/>
            </a-form-item>
          </a-col>
          <template v-if="toggleSearchStatus">

          </template>
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
      <a-button type="primary" icon="download" @click="handleExportXls('客户列表')">导出</a-button>
      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">
        <a-button type="primary" icon="import">导入</a-button>
      </a-upload>

      <j-super-query :fieldList="superFieldList" ref="superQueryModal" @handleSuperQuery="handleSuperQuery"></j-super-query>
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
        class="j-table-force-nowrap"
        :scroll="{x:true}"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange, type:'radio'}"
        :customRow="clickThenSelect"
        @change="handleTableChange">
        <span slot="action2" slot-scope="text, record">
          {{record.customStatus_dictText}}<span v-if="record.caozuoMiao==true" style="color:green;">(在线)</span><span v-if="record.caozuoMiao==false" style="color:#ccc;">(离线)</span>

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
        <template slot="pcaSlot" slot-scope="text">
          <div>{{ getPcaText(text) }}</div>
        </template>

        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>

          <a-divider type="vertical" />
          <a-dropdown>
            <a class="ant-dropdown-link">更多 <a-icon type="down" /></a>
            <a-menu slot="overlay">
            
              <a-menu-item>
                <a-popconfirm title="确定下线吗?" @confirm="() => downuser(record.id)">
                  <a>踢人</a>
                </a-popconfirm>
              </a-menu-item>
              <a-menu-item>
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>
              <a-menu-item @click="title='打码量',visible3=true,userinfo=record,damaNum = record.damaNum">
                  <a>打码量</a>
              </a-menu-item>
              <a-menu-item @click="title='赠送观影券',visible2=true,userinfo=record">
                  <a>赠送观影券</a>
              </a-menu-item>
              <a-menu-item @click="title='下分',visible=true,userinfo=record">
                  <a>下分</a>
              </a-menu-item>
              <a-menu-item @click="title='充值',visible=true,userinfo=record">
                  <a>充值</a>
              </a-menu-item>
              <a-menu-item @click="title='赠送',visible=true,userinfo=record">
                  <a>赠送</a>
              </a-menu-item>
               <a-menu-item @click="lahei(record.id)">
                  <a>拉黑</a>
              </a-menu-item>
            </a-menu>
          </a-dropdown>
        </span>

      </a-table>
    </div>

    <a-modal
    :title="title"
    :width="800"
    :visible="visible"
    :maskClosable="false"
    @ok="handleOk"
    @cancel="handleCancel"
    cancelText="关闭">

     <a-form-model-item label="金额" prop="createBy">

              <a-input v-model="account" placeholder="请输入金额" required></a-input>

      </a-form-model-item>

    </a-modal>

    <a-modal
    title="赠送观影券数量"
    :width="800"
    :visible="visible2"
    :maskClosable="false"
    @ok="handleOk2"
    @cancel="visible2 = false"
    cancelText="关闭">

     <a-form-model-item label="赠送观影券数量" prop="createBy">

              <a-input v-model="moviecount" placeholder="请输入赠送观影券" required></a-input>

      </a-form-model-item>

    </a-modal>

    <a-modal
    title="打码量"
    :width="800"
    :visible="visible3"
    :maskClosable="false"
    @ok="handleOk3"
    @cancel="visible3 = false"
    cancelText="关闭">

     <a-form-model-item label="修改打码量" prop="createBy">

              <a-input v-model="damaNum" placeholder="请输入打码量" required></a-input>

      </a-form-model-item>

    </a-modal>
    <a-tabs defaultActiveKey="1">
      <a-tab-pane tab="操作记录" key="1" >
        <PsCustomLogList :mainId="selectedMainId" />
      </a-tab-pane>
      <a-tab-pane tab="账单明细" key="2" >
        <PsCustomyeList :mainId="selectedMainId" />
      </a-tab-pane>
    </a-tabs>

    <psCustomMain-modal ref="modalForm" @ok="modalFormOk"></psCustomMain-modal>
  </a-card>
</template>

<script>

  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import PsCustomMainModal from './modules/PsCustomMainModal'
  import { getAction,httpAction } from '@/api/manage'
  import PsCustomLogList from './PsCustomLogList'
  import PsCustomyeList from './PsCustomyeList'
  import {initDictOptions,filterMultiDictText} from '@/components/dict/JDictSelectUtil'
  import Area from '@/components/_util/Area'
  import '@/assets/less/TableExpand.less'

  export default {
    name: "PsCustomMainList",
    mixins:[JeecgListMixin],
    components: { 
      PsCustomLogList,
      PsCustomyeList,
      PsCustomMainModal
    },
    data () {
      return {
        //自定义代码
        damaNum:'',
        moviecount:'',
        account:'',
        userinfo:'',
        title:'',
        visible:false,
        visible2:false,
        visible3:false,
        //自定义代码

        description: '客户列表管理页面',
        // 表头
        columns: [
          /* {
            title:'客户ID',
            align:"center",
            sorter: true,
            dataIndex: 'id'
          }, */
          {
            title:'等级',
            align:"center",
            sorter: true,
            dataIndex: 'customLevel_dictText',
          },
          {
            title:'客户名称',
            align:"center",
            sorter: true,
            dataIndex: 'customName'
          },
          {
            title:'手机号码',
            align:"center",
            dataIndex: 'customPhoneNum'
          },
          {
            title:'创建日期',
            align:"center",
            sorter: true,
            dataIndex: 'createTime'
          },
          {
            title:'登录时间',
            align:"center",
            dataIndex: 'loginTime'
          },

          {
            title:'账号状态',
            align:"center",
            sorter: true,
            dataIndex: 'customStatus_dictText',
            scopedSlots: { customRender: 'action2' },

          },
          {
            title:'登录IP',
            align:"center",
            dataIndex: 'loginIp'
          },
          {
            title:'虚拟金额',
            align:"center",
            dataIndex: 'givenAccount'
          },
          {
            title:'赠送金额',
            align:"center",
            dataIndex: 'zengAccount'
          },
          {
            title:'真实充值金额',
            align:"center",
            dataIndex: 'realAccount'
          },
          {
            title:'打码量',
            align:"center",
            dataIndex: 'damaNum'
          },
          {
            title:'播放次数',
            align:"center",
            dataIndex: 'playCount'
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
          /* {
            title:'银行卡-持卡人',
            align:"center",
            dataIndex: 'bankCustomName'
          }, */
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
          list: "/module/psCustomMain/list",
          delete: "/module/psCustomMain/delete",
          deleteBatch: "/module/psCustomMain/deleteBatch",
          exportXlsUrl: "/module/psCustomMain/exportXls",
          importExcelUrl: "module/psCustomMain/importExcel",
        },
        dictOptions:{
         customLevel:[],
         customStatus:[],
        },
        /* 分页参数 */
        ipagination:{
          current: 1,
          pageSize: 5,
          pageSizeOptions: ['5', '10', '50'],
          showTotal: (total, range) => {
            return range[0] + "-" + range[1] + " 共" + total + "条"
          },
          showQuickJumper: true,
          showSizeChanger: true,
          total: 0
        },
        selectedMainId:'',
        superFieldList:[],
        psCustomLogMainId: '',
        customnum:''
      }
    },
    created() {
      this.pcaData = new Area()
      this.getSuperFieldList();
      this.checkuser()
    },
    computed: {
      importExcelUrl: function(){
        return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
      }
    },
    methods: {
      //查询用户在线数量
      checkuser(id){
        getAction('/bg/index/findCustomNum', {}).then((res) => {
          console.log(res)
          this.customnum = res.result
        })
      },
      //用户下线
      downuser(id){
        getAction('/bg/custom/xiaxian', {customId:id}).then((res) => {
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
      //确认操作3大操作
      handleOk(){
        console.log(this.title,this.visible,this.userinfo)
        let url = ''
        let data = {
          account: this.account,
          ids: [this.userinfo.id]
        }
        if(this.title == '下分'){
          url = '/bg/custom/xiafen'
        }else if(this.title == '充值'){
          url = '/bg/custom/chongzhi'
        }else if(this.title == '拉黑'){
          url = '/bg/custom/lahei'
        }else if(this.title == '赠送'){
          url = '/bg/custom/zengsong'
        }

        httpAction(url,data,'post').then((res)=>{
            if(res.result == true){
              this.$message.success('操作成功')
            }else{
              this.$message.error('操作失败')
            }
            setTimeout(()=>{
              this.visible = false
              this.account = 0
             this.loadData()
            },1000)
        })
      },
      //确认操作2大操作
      handleOk2(){
        console.log(this.title,this.visible,this.userinfo)
        let url = '/bg/custom/zengsongPlayCount'
        let data = {
          count: this.moviecount,
          ids: [this.userinfo.id]
        }
        httpAction(url,data,'post').then((res)=>{
            if(res.result == true){
              this.$message.success('操作成功')
            }else{
              this.$message.error('操作失败')
            }
            setTimeout(()=>{
              this.visible2 = false
              this.moviecount = 0
             this.loadData()
            },1000)
        })
      },
       //确认操作3大操作
       handleOk3(){
        console.log(this.title,this.visible,this.userinfo)
        let url = '/bg/custom/damaliang'
        let data = {
          count: this.damaNum,
          ids: [this.userinfo.id]
        }
        httpAction(url,data,'post').then((res)=>{
            if(res.result == true){
              this.$message.success('操作成功')
            }else{
              this.$message.error('操作失败')
            }
            setTimeout(()=>{
              this.visible3 = false
              this.damaNum = 0
             this.loadData()
            },1000)
        })
      },
      //拉黑
      lahei(id){
        console.log(id)
        this.$confirm({
          title: '拉黑',
          content: '确定要拉黑吗?',
          onOk: function () {
               httpAction('/bg/custom/lahei',{ids:[id]},'post').then((res)=>{
                if(res.result == true){
                  this.$message.success('操作成功')
                }else{
                  this.$message.error('操作失败')
                }
                setTimeout(()=>{
                  this.visible = false
                  this.account = 0
                this.loadData()
                },1000)
            })
          }
        })
      },
      //关闭弹窗
      handleCancel(){
        this.visible = false
      },
      getPcaText(code){
        return this.pcaData.getText(code);
      },
      initDictConfig(){
        initDictOptions('ps_custom_main_status').then((res) => {
          if (res.success) {
            this.$set(this.dictOptions, 'customStatus', res.result)
          }
        })
      },
      clickThenSelect(record) {
        return {
          on: {
            click: () => {
              this.onSelectChange(record.id.split(","), [record]);
            }
          }
        }
      },
      onClearSelected() {
        this.selectedRowKeys = [];
        this.selectionRows = [];
        this.selectedMainId=''
      },
      onSelectChange(selectedRowKeys, selectionRows) {
        this.selectedMainId=selectedRowKeys[0]
        this.selectedRowKeys = selectedRowKeys;
        this.selectionRows = selectionRows;
        this.psCustomLogMainId = selectionRows[0]['id']
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
        this.onClearSelected()
        var params = this.getQueryParams();//查询条件
        this.loading = true;
        getAction(this.url.list, params).then((res) => {
          if (res.success) {
            this.dataSource = res.result.records;
            this.ipagination.total = res.result.total;
          }
          if(res.code===510){
            this.$message.warning(res.message)
          }
          this.loading = false;
        })
      },
      getSuperFieldList(){
        let fieldList=[];
        fieldList.push({type:'sel_user',value:'createBy',text:'创建人'})
        fieldList.push({type:'datetime',value:'createTime',text:'创建日期'})
        fieldList.push({type:'datetime',value:'updateTime',text:'更新日期'})
        fieldList.push({type:'Text',value:'headPortrait',text:'头像',dictCode:''})
        fieldList.push({type:'int',value:'customLevel',text:'等级',dictCode:'ps_custom_main_level'})
        fieldList.push({type:'string',value:'customPhoneNum',text:'手机号码',dictCode:''})
        fieldList.push({type:'string',value:'customPwd',text:'登录密码',dictCode:''})
        fieldList.push({type:'string',value:'customName',text:'客户名称',dictCode:''})
        fieldList.push({type:'string',value:'customStatus',text:'状态',dictCode:'ps_custom_main_status'})
        fieldList.push({type:'sel_user',value:'followBy',text:'跟进业务'})
        fieldList.push({type:'string',value:'loginIp',text:'登录IP',dictCode:''})
        fieldList.push({type:'string',value:'loginAddress',text:'登录地址',dictCode:''})
        fieldList.push({type:'datetime',value:'loginTime',text:'登录时间'})
        fieldList.push({type:'string',value:'payPwd',text:'支付密码',dictCode:''})
        fieldList.push({type:'BigDecimal',value:'givenAccount',text:'虚拟金额',dictCode:''})
        fieldList.push({type:'BigDecimal',value:'realAccount',text:'真实充值金额',dictCode:''})
        fieldList.push({type:'date',value:'customBirth',text:'生日'})
        fieldList.push({type:'string',value:'customEmail',text:'邮箱',dictCode:''})
        fieldList.push({type:'int',value:'customSex',text:'性别',dictCode:'sex'})
        fieldList.push({type:'string',value:'addressName',text:'地址-收货人',dictCode:''})
        fieldList.push({type:'string',value:'addressPhone',text:'地址-电话',dictCode:''})
        fieldList.push({type:'pca',value:'addressProvi',text:'地址-省市区'})
        fieldList.push({type:'string',value:'addressDetails',text:'地址-详细地址',dictCode:''})
        fieldList.push({type:'string',value:'addressTags',text:'地址-标签',dictCode:''})
        fieldList.push({type:'string',value:'bankName',text:'银行名称',dictCode:''})
        fieldList.push({type:'string',value:'bankNameDesc',text:'开户行',dictCode:''})
        fieldList.push({type:'string',value:'bankNum',text:'银行卡-卡号',dictCode:''})
        fieldList.push({type:'string',value:'bankCustomName',text:'银行卡-持卡人',dictCode:''})
        fieldList.push({type:'Text',value:'remark',text:'备注',dictCode:''})
        this.superFieldList = fieldList
      }
    }
  }
</script>
<style scoped>
  @import '~@assets/less/common.less'
</style>