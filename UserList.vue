<template>
  <div class="tree-wrapper" style="width:200px;text-align:left">
    <i-input
      placeholder="搜索用户"
      style="width: 90%"
      icon="ios-search"
      v-model="searchUserName"
      @on-click="onSearch" />
    <div style="width: 100%;padding-top: 8px" class="demo-spin-article">
      <div
        :value="item.username"
        style="margin-top: 6px;"
        v-for="item in userList"
        :key="item.username">
        <i-button type="text" @click.native="clickUser(item.username)">
          {{ item.username }}
        </i-button>
        <div style="float: right;margin-top: 10px">
          <i-tooltip content="点击查看用户信息" placement="left-start">
            <i-icon type="md-menu" @click.native="showUserInfo(item)" />
          </i-tooltip>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { Button, Input, Icon, Tooltip, Modal } from 'iview'

export default {
  name: 'UserList',
  components: {
    'i-button': Button,
    'i-input': Input,
    'i-icon': Icon,
    'i-tooltip': Tooltip
  },
  data () {
    return {
      userList: '',
      searchUserName: ''
    }
  },
  mounted () {
    this.getUserList({})
  },
  methods: {
    getUserList (param) {
      let token = 'eyJjdHkiOiJKV1QiLCJlbmMiOiJBMTkyQ0JDLUhTMzg0IiwiYWxnIjoiZGlyIn0..-FbAnP9QA9uRzv-9Qptn8g.NhMKnXW6BAsXUxOlbEzcmdalxct5XUy1A-YE97HgoYwAfD8bt9CUweTaW9kW2KI92XfQAiFsLiF1x_zGhe7NQ2-L1Aq1ZSdDzA3iN9yt3ztBIbkVw0lKmB2TOAgFx2pFulTnfss4kLH9mAT6t_h9FOjwMbVtD1tBLJJ9lB24tR8JtmtN2-p1ZckX75zlc5hmGkvB6KwuHa47ofLAn_ScvrB91rPpljgBgRjV3KnrSqs.MJKY-AI0TAd28SetQVfaIjwtjX4ZmfJB'

      var xhr = new XMLHttpRequest()
      var url = 'http://10.12.20.36:28091/auth-service/v1/users?page=1&size=10000'

      xhr.open('GET', url, true)
      xhr.setRequestHeader('K2_KEY', token)
      xhr.setRequestHeader('Content-Type', 'application/json;charset=UTF-8')
      xhr.onreadystatechange = () => {
        if (xhr.readyState === 4) {
          if (xhr.status === 200) {
            this.userList = JSON.parse(xhr.responseText).result
          }
        }
      }
      xhr.send(param)
    },
    onSearch () {
      let params = { userName: this.searchUserName }
      this.getUserList(params)
    },
    clickUser (username) {
      // this.userName=username;
      console.log(username)
    },
    showUserInfo (item) {
      var userCreatedDate = this.formatDate(new Date(item.createTime))
      var username = item.username
      var tenantId = item.tenantId
      var email = item.email
      var content = `<table class="detail-modal-table">
              <tr>
                <td>用户名</td>
                <td>&nbsp;:&nbsp;</td>
                <td>${username}</td>
              </tr>
              <tr>
                <td>租户ID</td>
                <td>&nbsp;:&nbsp;</td>
                <td>${tenantId}</td>
              </tr>
              <tr>
                <td>邮箱</td>
                <td>&nbsp;:&nbsp;</td>
                <td>${email}</td>
              </tr>
              <tr>
                <td>创建时间</td>
                <td>&nbsp;:&nbsp;</td>
               <td>${userCreatedDate}</td>
              </tr>
            </table>`
      Modal.info({ title: '用户信息查看', content: content })
    },
    formatDate (dateObj) {
      if (dateObj && typeof dateObj === 'object') {
        // note this returns UTC time (at zone 0), which is not what we want
        // return dateObj.toISOString().replace(/([-0-9]{10})T([:0-9]{8}).*/, '$1 $2');

        const yearStr = dateObj.getFullYear()
        const monthStr = ('00' + (dateObj.getMonth() + 1)).slice(-2)
        const dateStr = ('00' + dateObj.getDate()).slice(-2)

        const hourStr = ('00' + dateObj.getHours()).slice(-2)
        const minuteStr = ('00' + dateObj.getMinutes()).slice(-2)
        const secondStr = ('00' + dateObj.getSeconds()).slice(-2)

        return `${yearStr}-${monthStr}-${dateStr} ${hourStr}:${minuteStr}:${secondStr}`
      } else {
        return ''
      }
    }
  }
}
</script>
<style>
  .tree-wrapper {
    min-height: 100%;
    padding: 8px;
  }
</style>
