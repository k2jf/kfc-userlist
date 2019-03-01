<template>
  <div class="tree-wrapper">
    <i-input
      placeholder="搜索用户"
      style="width: 90%"
      icon="ios-search"
      v-model="searchUserName"
      @on-click="onSearch" />
    <div style="width: 100%;padding-top: 8px" class="demo-spin-article">
      <i-spin
        size="large"
        fix
        v-if="spinShow"></i-spin>
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
            <i-icon type="md-menu" @click.native="clickUserInfo(item)" />
          </i-tooltip>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { Button, Input, Spin, Icon, Tooltip } from 'iview'

export default {
  name: 'UserList',
  components: {
    'i-button': Button,
    'i-input': Input,
    'i-spin': Spin,
    'i-icon': Icon,
    'i-tooltip': Tooltip
  },
  mounted () {
    this.getUserList({})
  },
  methods: {
    getUserList (param) {
      let token = 'eyJjdHkiOiJKV1QiLCJlbmMiOiJBMTkyQ0JDLUhTMzg0IiwiYWxnIjoiZGlyIn0..DKInRzSv705fETH7equI1Q.Je2FVCpN_G2umkeLSexnmpTQZz2TWfOhLgG39iYspF1vyQfq3xtvXBxRCPdrQhN4MLPLEXRTlPx1cbDsuu0o9DA7DbwiU82x2ojTgFCVy5HvRi_uKtJW_2nAetFUopoi-EmYPhI44xrITztR_aE0x83Em95ijchagmCu012uBqZdv9nKht3DFsUIMvF4CSLFJMa0tFE274D1yQI_456_5ixhCb7-NggOrJ5Z_0w2BwPl4hyncLpH_i33KdQKoiGvD-E1EtbAsKq007Gk0N6pkQ.APvHmLOA3uFkJAL3wnZba8o5vJTds0zL'

      var xhr = new XMLHttpRequest()
      var url = 'http://10.12.20.36:28091/auth-service/v1/users?page=1&size=10000'

      xhr.open('GET', url, true)
      xhr.setRequestHeader('K2_KEY', token)
      xhr.setRequestHeader('Content-Type', 'application/json;charset=UTF-8')
      xhr.onreadystatechange = () => {
        if (xhr.readyState === 4) {
          if (xhr.status === 200) {
            this.userList = JSON.parse(xhr.responseText).result
            return JSON.parse(xhr.responseText).result
          }
        }
      }
      xhr.send(param)
    },
    getUserInfo () {
      let params = {}
      this.getUserList(params).then(data => {
        this.userList = data
        this.spinShow = !this.spinShow
      })
    },
    onSearch () {
      this.spinShow = !this.spinShow
      let params = { userName: this.searchUserName }
      this.getUserList(params).then(data => {
        this.userList = data
        this.spinShow = !this.spinShow
      })
    },
    clickUser (username) {
      // this.userName=username;
      console.log(username)
    },
    clickUserInfo (item) {
      let info = [
        { label: '用户名', value: item.username },
        { label: '用户ID', value: item.id },
        { label: '创建时间', value: this.formatDate(new Date(item.createTime)) },
        { label: '租户ID', value: item.tenantId },
        { label: '邮箱', value: item.email }
      ]
      this.userInfo = item
      this.$detail('用户信息查看', info)
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
