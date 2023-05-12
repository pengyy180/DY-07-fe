<template>
  <div class="page-header-index-wide">
    <a-row :gutter="24">
      <a-col :sm="12" :md="8" :xl="8" :style="{ marginBottom: '24px' }">
        <chart-card :loading="loading" title="总充值金额" :total="'￥'+zongchongzhijine">
          <a-tooltip title="指标说明" slot="action">
            <a-icon type="info-circle-o" />
          </a-tooltip>
          <div>
            <trend style="margin-right: 16px;">
              <span slot="term">今日充值</span>
              ￥{{zongchongToDayzhijine}}
            </trend>
<!--            <trend>
              <span slot="term">昨日充值</span>
              ￥{{zongchongYezhijine}}
            </trend>-->
          </div>
          <template slot="footer"><span>以客户"真实充值金额"为准</span></template>
        </chart-card>
      </a-col>
      <a-col :sm="12" :md="8" :xl="8" :style="{ marginBottom: '24px' }">
        <chart-card :loading="loading" title="提现总金额" :total="'￥'+tixianSum">
          <a-tooltip title="指标说明" slot="action">
            <a-icon type="info-circle-o" />
          </a-tooltip>
          <div>
            <!--            <trend  style="margin-right: 16px;">
                          <span slot="term">昨日充值次数</span>
                          {{ customTodayCount }}
                        </trend>-->
            <trend>
              <span slot="term">今日提现金额</span>
              ￥{{tixianToDaySum}}
            </trend>
          </div>
          <template slot="footer"><span>以"审核通过"提现为准</span></template>
        </chart-card>
      </a-col>
      <a-col :sm="12" :md="8" :xl="8" :style="{ marginBottom: '24px' }">
        <chart-card :loading="loading" title="总赠送金额" :total="'￥'+zengsongzhijine">
          <a-tooltip title="指标说明" slot="action">
            <a-icon type="info-circle-o" />
          </a-tooltip>
          <div>
            <trend style="margin-right: 16px;">
              <span slot="term">今日赠送金额</span>
              ￥{{zengsongToDayzhijine}}
            </trend>
            <!--            <trend>
                          <span slot="term">昨日充值</span>
                          ￥{{zongchongYezhijine}}
                        </trend>-->
          </div>
          <template slot="footer"><span>以客户"虚拟金额"为准(不含实际充值金额)</span></template>
        </chart-card>
      </a-col>

<!--      <a-col :sm="12" :md="6" :xl="4" :style="{ marginBottom: '24px' }">
        <chart-card :loading="loading" title="充值总次数" :total="chongzhizongcishu">
          <a-tooltip title="指标说明" slot="action">
            <a-icon type="info-circle-o" />
          </a-tooltip>
          <div>
            <trend>
              <span slot="term">今日充值次数</span>
              {{chongzhizongToDaycishu}}
            </trend>
          </div>
          <template slot="footer"><span>以客户"真实充值次数"为准</span></template>
        </chart-card>
      </a-col>-->

      <a-col :sm="12" :md="8" :xl="8" :style="{ marginBottom: '24px' }">
        <chart-card :loading="loading" title="客户总数" :total="customCount">
          <a-tooltip title="指标说明" slot="action">
            <a-icon type="info-circle-o" />
          </a-tooltip>
          <div>
            <trend  style="margin-right: 16px;">
              <span slot="term">今日新增客户</span>
              {{ customTodayCount }}
            </trend>
<!--            <trend>
              <span slot="term">昨日新增客户</span>
              {{customYeCount}}
            </trend>-->
          </div>
          <template slot="footer"><span>以客户"注册时间"为准</span></template>
        </chart-card>
      </a-col>

      <a-col :sm="12" :md="8" :xl="8" :style="{ marginBottom: '24px' }">
        <chart-card :loading="loading" title="下单总数" :total="orderCount">
          <a-tooltip title="指标说明" slot="action">
            <a-icon type="info-circle-o" />
          </a-tooltip>
          <div>
            <trend  style="margin-right: 16px;">
              <span slot="term">今日下单数</span>
              {{ orderToDayCount }}
            </trend>
<!--            <trend>
              <span slot="term">昨日下单数</span>
              {{customYeCount}}
            </trend>-->
          </div>
          <template slot="footer"><span>以订单"创建时间"为准</span></template>
        </chart-card>
      </a-col>
      
      <a-col :sm="12" :md="8" :xl="8" :style="{ marginBottom: '24px' }">
        <chart-card :loading="loading" title="当前活动" :total="activityTitile">
          <a-tooltip title="指标说明" slot="action">
            <a-icon type="info-circle-o" />
          </a-tooltip>
          <div>
            <trend  style="margin-right: 16px;">
              <span slot="term">今日有效活动数</span>
              {{ activityYouToDayTitile }}
            </trend>
            <trend>
              <span slot="term">有效活动总数</span>
              {{activityYouCount}}
            </trend>
          </div>
          <template slot="footer"><span>有效活动数: 有客户参与下单的活动数量</span></template>
        </chart-card>
      </a-col>

<!--      <a-col :sm="12" 8 :style="{ marginBottom: '24px' }">
        <chart-card :loading="loading" title="操作总数" :total="customLogCount">
          <a-tooltip title="指标说明" slot="action">
            <a-icon type="info-circle-o" />
          </a-tooltip>
          <div>
            <trend  style="margin-right: 16px;">
              <span slot="term">今日客户操作记录</span>
              {{ customLogToDayCount }}
            </trend>
&lt;!&ndash;            <trend>
              <span slot="term">昨日客户操作记录</span>
              {{customYeCount}}
            </trend>&ndash;&gt;
          </div>
          <template slot="footer"><span>以客户"操作记录"为准</span></template>
        </chart-card>
      </a-col>-->
      
    </a-row>

    <a-row :gutter="24">


<!--      <a-col :sm="12" :md="6" :xl="4" :style="{ marginBottom: '24px' }">
        <chart-card :loading="loading" title="赠送总次数" :total="zengsongzongcishu">
          <a-tooltip title="指标说明" slot="action">
            <a-icon type="info-circle-o" />
          </a-tooltip>
          <div>
            &lt;!&ndash;            <trend  style="margin-right: 16px;">
                          <span slot="term">昨日充值次数</span>
                          {{ customTodayCount }}
                        </trend>&ndash;&gt;
            <trend>
              <span slot="term">今日赠送次数</span>
              {{zengsongToDaycishu}}
            </trend>
          </div>
          <template slot="footer"><span>以客户"虚拟金额"为准(不含实际充值金额)</span></template>
        </chart-card>
      </a-col>-->



<!--        <a-col :sm="12" :md="6" :xl="4" :style="{ marginBottom: '24px' }">
          <chart-card :loading="loading" title="提现总次数" :total="tixianCount">
            <a-tooltip title="指标说明" slot="action">
              <a-icon type="info-circle-o" />
            </a-tooltip>
            <div>
              &lt;!&ndash;            <trend  style="margin-right: 16px;">
                            <span slot="term">昨日充值次数</span>
                            {{ customTodayCount }}
                          </trend>&ndash;&gt;
              <trend>
                <span slot="term">今日提现次数</span>
                {{tixianTodayCount}}
              </trend>
            </div>
            <template slot="footer"><span>以审核通过提现为准</span></template>
          </chart-card>
        </a-col>-->

<!--      <a-col :sm="12" :md="6" :xl="4" :style="{ marginBottom: '24px' }">
        <chart-card :loading="loading" title="提现待审核条数" :total="tixianCount2">
          <a-tooltip title="指标说明" slot="action">
            <a-icon type="info-circle-o" />
          </a-tooltip>
          <div>
            &lt;!&ndash;            <trend  style="margin-right: 16px;">
                          <span slot="term">昨日充值次数</span>
                          {{ customTodayCount }}
                        </trend>&ndash;&gt;
            <trend>
              <span slot="term">提现待审核金额</span>
              ￥{{tixianTodayCount2}}
            </trend>
          </div>
          <template slot="footer"><span>以用户"提交提现未审核"次数为准</span></template>
        </chart-card>
      </a-col>-->

    </a-row>

    <a-row >
      <a-col :sm="24" :md="12" :xl="12" style="margin: 10px 0;background-color:#fff">
        <rank-list title="客户充值金额排行榜" :list="customMainList" style="padding:30px 60px"/>
      </a-col>
      <a-col :sm="24" :md="12" :xl="12" style="margin: 10px 0;background-color:#fff">
        <rank-list title="客户赠送金额排行榜" :list="customMainGiveList" style="padding:30px 60px"/>
      </a-col>
<!--      <a-col :sm="24" :md="12" :xl="6" style="margin: 10px 0;background-color:#fff">
        <rank-list title="客户充值次数排行榜" :list="customMainPayLogList" style="padding:30px 60px"/>
      </a-col>-->
<!--      <a-col :sm="24" :md="12" :xl="6" style="margin: 10px 0;background-color:#fff">
        <rank-list title="客户操作排行榜" :list="customMainLogList" style="padding:30px 60px"/>
      </a-col>-->
    </a-row>

    <a-row >
<!--      <a-col :sm="24" :md="12" :xl="6" style="margin: 10px 0;background-color:#fff">
        <rank-list title="客户下单次数排行榜" :list="customMainOrderList" style="padding:30px 60px"/>
      </a-col>-->
<!--      <a-col :sm="24" :md="12" :xl="6" style="margin: 10px 0;background-color:#fff">
        <rank-list title="商品购买次数排行榜" :list="productCountList" style="padding:30px 60px"/>
      </a-col>
      <a-col :sm="24" :md="12" :xl="6" style="margin: 10px 0;background-color:#fff">
        <rank-list title="活动参与人数排行榜" :list="activityCountLogList" style="padding:30px 60px"/>
      </a-col>
      <a-col :sm="24" :md="12" :xl="6" style="margin: 10px 0;background-color:#fff">
        <rank-list title="活动消费金额排行榜" :list="activityAccontLogList" style="padding:30px 60px"/>
      </a-col>-->
    </a-row>

<!--    <a-row>
      <a-col :sm="24" :md="14" :xl="14" :style="{ marginBottom: '24px',marginRight:'0'}">
        <a-card :loading="loading" :bordered="false" :body-style="{padding: '0'}">
          <div class="salesCard">
            <a-tabs default-active-key="1" size="large" :tab-bar-style="{marginBottom: '23px', paddingLeft: '16px'}">
              <div class="extra-wrapper" slot="tabBarExtraContent">
                <div class="extra-item">
                  <a>今日</a>
                  <a>本周</a>
                  <a>本月</a>
                </div>
                <a-range-picker :style="{width: '256px'}" />
              </div>
              <a-tab-pane loading="true" tab="销售额" key="1">
                <a-row>
                  <a-col :xl="16" :lg="12" :md="12" :sm="24" :xs="24">
                    <bar title="每日销售额图表" :dataSource="barData"/>
                  </a-col>
                  <a-col :xl="8" :lg="12" :md="12" :sm="24" :xs="24">
                    <rank-list title="客户充值排行榜" :list="customMainList" style="padding:0 10px 10px"/>
                  </a-col>
                </a-row>
              </a-tab-pane>
            </a-tabs>
          </div>
        </a-card>
      </a-col>
      <a-col :sm="24" :md="10" :xl="10" :style="{}">
        <a-card :loading="loading" :bordered="false" title="最近一周访问量统计" :body-style="{padding: '0'}">
          <a-row>
            <a-col :span="6">
              <head-info title="今日IP" :content="loginfo.todayIp"></head-info>
            </a-col>
            <a-col :span="2">
              <a-spin class='circle-cust'>
                <a-icon slot="indicator" type="environment" style="font-size: 24px"  />
              </a-spin>
            </a-col>
            <a-col :span="6">
              <head-info title="今日访问" :content="loginfo.todayVisitCount"></head-info>
            </a-col>
            <a-col :span="2">
              <a-spin class='circle-cust'>
                <a-icon slot="indicator" type="team" style="font-size: 24px"  />
              </a-spin>
            </a-col>
            <a-col :span="6">
              <head-info title="总访问量" :content="loginfo.totalVisitCount"></head-info>
            </a-col>
            <a-col :span="2">
              <a-spin class='circle-cust'>
                <a-icon slot="indicator" type="rise" style="font-size: 24px"  />
              </a-spin>
            </a-col>
          </a-row>
          <line-chart-multid :fields="visitFields" :dataSource="visitInfo"></line-chart-multid>
        </a-card>
      </a-col>
    </a-row>-->



<!--    <a-row>
      <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
        <chart-card :loading="loading" title="订单量" :total="8846 | NumberFormat">
          <a-tooltip title="指标说明" slot="action">
            <a-icon type="info-circle-o" />
          </a-tooltip>
          <div>
            <mini-area />
          </div>
          <template slot="footer">日订单量<span> {{ '1234' | NumberFormat }}</span></template>
        </chart-card>
      </a-col>
            <a-col ::sm="12" :md="6" :xl="4" :style="{ marginBottom: '24px' }">
        <chart-card :loading="loading" title="支付笔数" :total="6560 | NumberFormat">
          <a-tooltip title="指标说明" slot="action">
            <a-icon type="info-circle-o" />
          </a-tooltip>
          <div>
            <mini-bar :height="40" />
          </div>
          <template slot="footer">转化率 <span>60%</span></template>
        </chart-card>
      </a-col>
      <a-col :sm="12" :md="6" :xl="4" :style="{ marginBottom: '24px' }">
        <chart-card :loading="loading" title="运营活动效果" total="78%">
          <a-tooltip title="指标说明" slot="action">
            <a-icon type="info-circle-o" />
          </a-tooltip>
          <div>
            <mini-progress color="rgb(19, 194, 194)" :target="80" :percentage="78" :height="8" />
          </div>
          <template slot="footer">
            <trend  style="margin-right: 16px;">
              <span slot="term">同周比</span>
              12%
            </trend>
            <trend>
              <span slot="term">日环比</span>
              80%
            </trend>
          </template>
        </chart-card>
      </a-col>
    </a-row>-->
  </div>
</template>

<script>
  import ChartCard from '@/components/ChartCard'
  import ACol from "ant-design-vue/es/grid/Col"
  import ATooltip from "ant-design-vue/es/tooltip/Tooltip"
  import MiniArea from '@/components/chart/MiniArea'
  import MiniBar from '@/components/chart/MiniBar'
  import MiniProgress from '@/components/chart/MiniProgress'
  import RankList from '@/components/chart/RankList'
  import Bar from '@/components/chart/Bar'
  import LineChartMultid from '@/components/chart/LineChartMultid'
  import HeadInfo from '@/components/tools/HeadInfo.vue'

  import Trend from '@/components/Trend'
  import { getLoginfo,getVisitInfo } from '@/api/api'
  import { getAction } from '@/api/manage'


  const rankList = []
  for (let i = 0; i < 7; i++) {
    rankList.push({
      name: (i+1) + ' 号彭于晏',
      total: 1234.56 - i * 100
    })
  }
  const barData = []
  for (let i = 0; i < 12; i += 1) {
    barData.push({
      x: `${i + 1}号`,
      y: Math.floor(Math.random() * 1000) + 200
    })
  }
  export default {
    name: "IndexChart",
    components: {
      ATooltip,
      ACol,
      ChartCard,
      MiniArea,
      MiniBar,
      MiniProgress,
      RankList,
      Bar,
      Trend,
      LineChartMultid,
      HeadInfo
    },
    data() {
      return {
        loading: true,
        center: null,
        rankList,
        barData,
        loginfo:{},
        visitFields:['ip','visit'],
        visitInfo:[],

        zongchongzhijine:"",
        zongchongToDayzhijine:"",
        // chongzhizongcishu:"",
        chongzhizongToDaycishu:"",
        customCount:"",
        customTodayCount:"",
        orderCount:"",
        orderToDayCount:"",
        activityTitile:"",
        activityYouToDayTitile:"",
        activityYouCount:"",
        // customLogCount:"",
        customLogToDayCount:"",
        zengsongzhijine:"",
        zengsongToDayzhijine:"",
        // zengsongzongcishu:"",
        // zengsongToDaycishu:"",

        tixianSum:"",
        tixianToDaySum:"",
        // tixianCount:"",
        // tixianTodayCount:"",
        tixianCount2:"",
        tixianTodayCount2:"",


        customMainList:[],
        customMainGiveList:[],
        // customMainPayLogList:[],
        // customMainLogList:[],

        // customMainOrderList:[],
        // productCountList:[],
        // activityCountLogList:[],
        // activityAccontLogList:[],

        indicator: <a-icon type="loading" style="font-size: 24px" spin />
      }
    },
    created() {
      setTimeout(() => {
        this.loading = !this.loading
      }, 1000)
      this.initLogInfo();
      this.checkdata()

    },
     onload(){
      this.checkdata()
    },
    methods: {
      //数据
       checkdata(){
        getAction('/bg/index/findIndexData').then((res)=>{
               console.log(res)
               this.zongchongzhijine = res.result.zongchongzhijine
               this.zongchongToDayzhijine = res.result.zongchongToDayzhijine
               // this.chongzhizongcishu = res.result.chongzhizongcishu
               this.chongzhizongToDaycishu = res.result.chongzhizongToDaycishu
               this.customCount = res.result.customCount
               this.customTodayCount = res.result.customTodayCount
               this.orderCount = res.result.orderCount
               this.orderToDayCount = res.result.orderToDayCount
               this.activityTitile = res.result.activityTitile
               this.activityYouToDayTitile = res.result.activityYouToDayTitile
               this.activityYouCount = res.result.activityYouCount
               // this.customLogCount = res.result.customLogCount
               this.customLogToDayCount = res.result.customLogToDayCount

               this.zengsongzhijine = res.result.zengsongzhijine
               this.zengsongToDayzhijine = res.result.zengsongToDayzhijine
               this.zengsongzongcishu = res.result.zengsongzongcishu
               // this.zengsongToDaycishu = res.result.zengsongToDaycishu

               this.tixianSum = res.result.tixianSum
               this.tixianToDaySum = res.result.tixianToDaySum
               // this.tixianCount = res.result.tixianCount
               // this.tixianTodayCount = res.result.tixianTodayCount
               this.tixianCount2 = res.result.tixianCount2
               this.tixianTodayCount2 = res.result.tixianTodayCount2

               this.customMainList = res.result.customMainList
               // this.customMainPayLogList = res.result.customMainPayLogList
               // this.customMainLogList = res.result.customMainLogList
               // this.customMainOrderList = res.result.customMainOrderList
               // this.activityAccontLogList = res.result.activityAccontLogList
               // this.activityCountLogList = res.result.activityCountLogList
               // this.productCountList = res.result.productCountList
               this.customMainGiveList = res.result.customMainGiveList
        })
      },
      initLogInfo () {
        getLoginfo(null).then((res)=>{
          if(res.success){
            Object.keys(res.result).forEach(key=>{
              res.result[key] =res.result[key]+""
            })
            this.loginfo = res.result;
          }
        })
        getVisitInfo().then(res=>{
          if(res.success){
             this.visitInfo = res.result;
           }
         })
      },
    }
  }
</script>

<style lang="less" scoped>
  .circle-cust{
    position: relative;
    top: 28px;
    left: -100%;
  }
  .extra-wrapper {
    line-height: 55px;
    padding-right: 24px;

    .extra-item {
      display: inline-block;
      margin-right: 24px;

      a {
        margin-left: 24px;
      }
    }
  }

  /* 首页访问量统计 */
  .head-info {
    position: relative;
    text-align: left;
    padding: 0 32px 0 0;
    min-width: 125px;

    &.center {
      text-align: center;
      padding: 0 32px;
    }

    span {
      color: rgba(0, 0, 0, .45);
      display: inline-block;
      font-size: .95rem;
      line-height: 42px;
      margin-bottom: 4px;
    }
    p {
      line-height: 42px;
      margin: 0;
      a {
        font-weight: 600;
        font-size: 1rem;
      }
    }
  }
</style>