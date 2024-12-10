<template>
  <el-row type="flex" :gutter="20">
    <el-col :md="10" :lg="8">
      <el-card class="admin-info">
        <el-row :gutter="20">
          <el-col :span="10">
            <img class="avatar" :src="profile.avatar"/>
          </el-col>
          <el-col :span="14">
            <p class="admin-info-name">{{user.username}}</p>
            <p>{{user.admin_type}}</p>
          </el-col>
        </el-row>
        <hr/>
        <div class="last-info">
          <p class="last-info-title">{{$t('m.Last_Login')}}</p>
          <el-form label-width="80px" class="last-info-body">
            <el-form-item label="Time:">
              <span>{{session.last_activity | localtime}}</span>
            </el-form-item>
            <el-form-item label="IP:">
              <span>{{session.ip}}</span>
            </el-form-item>
            <el-form-item label="OS">
              <span>{{os}}</span>
            </el-form-item>
            <el-form-item label="Browser:">
              <span>{{browser}}</span>
            </el-form-item>
          </el-form>
        </div>
      </el-card>
    </el-col>

    <el-col :md="14" :lg="16" v-if="isSuperAdmin">
      <div class="info-container">
        <info-card color="#909399" icon="el-icon-fa-users" message="Total Users" iconSize="30px" class="info-item"
                   :value="infoData.user_count"></info-card>
        <info-card color="#67C23A" icon="el-icon-fa-list" message="Today Submissions" class="info-item"
                   :value="infoData.today_submission_count"></info-card>
        <info-card color="#409EFF" icon="el-icon-fa-trophy" message="Recent Contests" class="info-item"
                   :value="infoData.recent_contest_count"></info-card>
      </div>

  <panel style="margin-top: 5px">
    <span slot="title" v-loading="loadingReleases">制作小组基础信息
      <el-popover placement="right" trigger="hover">
        <p>Please upgrade to the latest version to enjoy the new features.</p>
      </el-popover>
    </span>

    <el-collapse v-model="activeNames">
      <div v-for="(item, index) in data" :key="index" style="display: flex; align-items: center; border-bottom: 1px solid #ccc; padding: 10px;">
        <img :src="item.image" alt="Image" style="width: 100px; height: 100px; margin-right: 20px;">
        <div>
          <p><strong>姓名 ：</strong> {{ item.name }}</p>
          <p><strong>ID   ：</strong> {{ item.studentId }}</p>
          <p><strong>身份 ：</strong> {{ item.gender }}</p>
        </div>
      </div>
    </el-collapse>
  </panel>

     
    </el-col>
  </el-row>
</template>


<script>
  import { mapGetters } from 'vuex'
  import browserDetector from 'browser-detect'
  import InfoCard from '@admin/components/infoCard.vue'
  import api from '@admin/api'

  export default {
    name: 'dashboard',
    components: {
      InfoCard
    },
 data() {
  return {
    infoData: {
      user_count: 0,
      recent_contest_count: 0,
      today_submission_count: 0,
      judge_server_count: 0,
      env: {}
    },
    activeNames: [1],
    session: {},
    loadingReleases: true,
    releases: [],
    data: [
      { name: '郎若谷', studentId: '2351871', gender: '见习魔法师', image: require('./lrg.jpg') },
      { name: '左知行', studentId: '2352469', gender: '牛马小鼠鼠', image: require('./zzx.png') },
      { name: '祁浩哲', studentId: '2353941', gender: '庄子养乌龟', image: require('./qhz.png') },
      { name: '黄保翔', studentId: '2351753', gender: '野猪骑士王', image: require('./hbx.png') },
      { name: '黄唯轩', studentId: '2353123', gender: '个人练习生', image: require('./hwx.jpg') }
    ]
  };
},
    mounted () {
      api.getDashboardInfo().then(resp => {
        this.infoData = resp.data.data
      }, () => {
      })
      api.getSessions().then(resp => {
        this.parseSession(resp.data.data)
      }, () => {
      })
      api.getReleaseNotes().then(resp => {
        this.loadingReleases = false
        let data = resp.data.data
        if (!data) {
          return
        }
        let currentVersion = data.local_version
        data.update.forEach(release => {
          if (release.version > currentVersion) {
            release.new_version = true
          }
        })
        this.releases = data.update
      }, () => {
        this.loadingReleases = false
      })
    },
    methods: {
      parseSession (sessions) {
        let session = sessions[0]
        if (sessions.length > 1) {
          session = sessions.filter(s => !s.current_session).sort((a, b) => {
            return a.last_activity < b.last_activity
          })[0]
        }
        this.session = session
      }
    },
    computed: {
      ...mapGetters(['profile', 'user', 'isSuperAdmin']),
      cdn () {
        return this.infoData.env.STATIC_CDN_HOST
      },
      https () {
        return document.URL.slice(0, 5) === 'https'
      },
      forceHttps () {
        return this.infoData.env.FORCE_HTTPS
      },
      browser () {
        let b = browserDetector(this.session.user_agent)
        if (b.name && b.version) {
          return b.name + ' ' + b.version
        } else {
          return 'Unknown'
        }
      },
      os () {
        let b = browserDetector(this.session.user_agent)
        return b.os ? b.os : 'Unknown'
      }
    }
  }
</script>

<style lang="less">
  .admin-info {
    margin-bottom: 20px;
    &-name {
      font-size: 24px;
      font-weight: 700;
      margin-bottom: 10px;
      color: #409EFF;
    }
    .avatar {
      max-width: 100%;
    }
    .last-info {
      &-title {
        font-size: 16px;
      }
      &-body {
        .el-form-item {
          margin-bottom: 5px;
        }
      }
    }
  }

  .info-container {
    display: flex;
    justify-content: flex-start;
    flex-wrap: wrap;
    .info-item {
      flex: 1 0 auto;
      min-width: 200px;
      margin-bottom: 10px;
    }
  }

</style>
