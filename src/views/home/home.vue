<template>
  <div class="home-main" style="display:block">
    <Row :gutter="10">
      <Col :md="24" :lg="8">
        <Row class-name="home-page-row1" :gutter="10">
          <Col :md="12" :lg="24" :style="{marginBottom: '10px'}">
            <Card>
              <Row type="flex" class="user-infor">
                <Col span="8">
                  <Row class-name="made-child-con-middle" type="flex" align="middle">
                    <img class="avator-img" :src="avatorPath" />
                  </Row>
                </Col>
                <Col span="16" style="padding-left:6px;">
                  <Row class-name="made-child-con-middle" type="flex" align="middle">
                    <div>
                      <b class="card-user-infor-name">Admin</b>
                      <p>super admin</p>
                    </div>
                  </Row>
                </Col>
              </Row>
              <div class="line-gray"></div>
              <Row class="margin-top-8">
                <Col span="8"><p class="notwrap">上次登录时间:</p></Col>
                <Col span="16" class="padding-left-8">2017.09.12-13:32:20</Col>
              </Row>
              <Row class="margin-top-8">
                <Col span="8"><p class="notwrap">上次登录地点:</p></Col>
                <Col span="16" class="padding-left-8">北京</Col>
              </Row>
            </Card>
          </Col>
          <Col :md="12" :lg="24" :style="{marginBottom: '10px'}">
            <Card>
              <p slot="title" class="card-title">
                <Icon type="android-checkbox-outline"></Icon>
                待办业务
              </p>
              <a type="text" slot="extra" @click.prevent="addNewToDoItem">
                <Icon type="plus-round"></Icon>
              </a>
              <Modal
                v-model="showAddNewTodo"
                title="添加新的待办事项"
                @on-ok="addNew"
                @on-cancel="cancelAdd">
                <Row type="flex" justify="center">
                  <Input v-model="newToDoItemValue" icon="compose" placeholder="请输入..." style="width: 300px" />
                </Row>
                <Row slot="footer">
                  <Button type="text" @click="cancelAdd">取消</Button>
                  <Button type="primary" @click="addNew">确定</Button>
                </Row>
              </Modal>
              <div class="to-do-list-con">
                <div v-for="(item, index) in toDoList" :key="index" class="to-do-item">
                  <to-do-list-item :content="item.title"></to-do-list-item>
                </div>
              </div>
            </Card>
          </Col>
        </Row>
      </Col>
      <Col :md="24" :lg="16">
        <Row :gutter="5">
          <Col :xs="24" :sm="12" :md="6" :style="{marginBottom: '10px'}">
            <infor-card
              id-name="user_created_count"
              :end-val="count.createUser"
              iconType="android-person"
              color="#2d8cf0"
              intro-text="注册客户(总量)"
            ></infor-card>
          </Col>
          <Col :xs="24" :sm="12" :md="6" :style="{marginBottom: '10px'}">
            <infor-card
              id-name="visit_count"
              :end-val="count.visit"
              iconType="android-person-add"
              color="#64d572"
              intro-text="新增注册客户(当天)"
            ></infor-card>
          </Col>
          <Col :xs="24" :sm="12" :md="6" :style="{marginBottom: '10px'}">
            <infor-card
              id-name="collection_count"
              :end-val="count.collection"
              iconType="checkmark-round"
              color="#ffd572"
              intro-text="准入客户(总量)"
            ></infor-card>
          </Col>
          <Col :xs="24" :sm="12" :md="6" :style="{marginBottom: '10px'}">
            <infor-card
              id-name="transfer_count"
              :end-val="count.transfer"
              iconType="android-hand"
              color="#f25e43"
              intro-text="拒绝客户(总量)"
            ></infor-card>
          </Col>
        </Row>
        <Row>
          <Card :padding="0">
            <p slot="title" class="card-title">
              <Icon type="map"></Icon>
              今日服务调用地理分布
            </p>
            <div class="map-con">
              <Col span="10">
                <map-data-table :cityData="cityData" height="281" :style-obj="{margin: '12px 0 0 11px'}"></map-data-table>
              </Col>
              <Col span="14" class="map-incon">
                <Row type="flex" justify="center" align="middle">
                  <home-map :city-data="cityData"></home-map>
                </Row>
              </Col>
            </div>
          </Card>
        </Row>
      </Col>
    </Row>
    <Row :gutter="10" class="margin-top-10">
      <Col :md="24" :lg="8" :style="{marginBottom: '10px'}">
        <Card>
          <p slot="title" class="card-title">
            <Icon type="android-map"></Icon>
            上周每日来访量统计
          </p>
          <div class="data-source-row">
            <visite-volume></visite-volume>
          </div>
        </Card>
      </Col>
      <Col :md="24" :lg="8" :style="{marginBottom: '10px'}">
        <Card>
          <p slot="title" class="card-title">
            <Icon type="ios-pulse-strong"></Icon>
            数据来源统计
          </p>
          <div class="data-source-row">
            <data-source-pie></data-source-pie>
          </div>
        </Card>
      </Col>
      <Col :md="24" :lg="8">
        <Card>
          <p slot="title" class="card-title">
            <Icon type="android-wifi"></Icon>
            各类用户服务调用变化统计
          </p>
          <div class="data-source-row">
            <user-flow></user-flow>
          </div>
        </Card>
      </Col>
    </Row>
    <Row class="margin-top-10" :style="{display: 'none'}">
      <Card>
        <p slot="title" class="card-title">
          <Icon type="ios-shuffle-strong"></Icon>
          上周每日服务调用量(万)
        </p>
        <div class="line-chart-con">
          <service-requests></service-requests>
        </div>
      </Card>
    </Row>
  </div>
</template>

<script>
  import cityData from './map-data/get-city-value.js';
  import homeMap from './components/map.vue';
  import dataSourcePie from './components/dataSourcePie.vue';
  import visiteVolume from './components/visiteVolume.vue';
  import serviceRequests from './components/serviceRequests.vue';
  import userFlow from './components/userFlow.vue';
  import countUp from './components/countUp.vue';
  import inforCard from './components/inforCard.vue';
  import mapDataTable from './components/mapDataTable.vue';
  import toDoListItem from './components/toDoListItem.vue';

  export default {
    name: 'home',
    components: {
      homeMap,
      dataSourcePie,
      visiteVolume,
      serviceRequests,
      userFlow,
      countUp,
      inforCard,
      mapDataTable,
      toDoListItem
    },
    data () {
      return {
        toDoList: [
          {
            title: '待添加功能。。。'
          },
          {
            title: '待添加功能。。。'
          },
          {
            title: '待添加功能。。。'
          },
          {
            title: '待添加功能。。。'
          },
          {
            title: '待添加功能。。。'
          }
        ],
        count: {
          createUser: 966,
          visit: 324,
          collection: 152,
          transfer: 234
        },
        cityData: cityData,
        showAddNewTodo: false,
        newToDoItemValue: ''
      };
    },
    computed: {
      avatorPath () {
        return localStorage.avatorImgPath;
      }
    },
    methods: {
      addNewToDoItem () {
        this.showAddNewTodo = true;
      },
      addNew () {
        if (this.newToDoItemValue.length !== 0) {
          this.toDoList.unshift({
            title: this.newToDoItemValue
          });
          setTimeout(() => {
            this.newToDoItemValue = '';
          }, 200);
          this.showAddNewTodo = false;
        } else {
          this.$Message.error('请输入待办事项内容');
        }
      },
      cancelAdd () {
        this.showAddNewTodo = false;
        this.newToDoItemValue = '';
      }
    }
  };
</script>
<style lang="less">
  @import "../../styles/home.less";
  @import "../../styles/common/common.less";
</style>
