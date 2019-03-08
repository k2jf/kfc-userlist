# kfc-userlist
获取用户列表

# git 相关
```
# 初次clone
git remote add -f kfc-userlist git@github.com:k2jf/kfc-userlist.git
git subtree add -P src/components/kfc-userlist kfc-userlist master --squash

# pull
git subtree pull -P src/components/kfc-userlist kfc-userlist master --squash

# push
git subtree push -P src/components/kfc-userlist kfc-userlist master

```
# 属性
| 属性                    | 说明                           | 类型                 | 默认值        |
| ----------------------- | ------------------------------ | -------------------- | ------------- |
| @on-click-user | 当点击用户名称时触发的事件 | Func(userInfo) | -             |
# 使用方法
```
<template>
    <UserList @on-click-user="clickUser"/>
</template>

<script>
import UserList from '@/components/kfc-userlist'

export default {
  name: 'Another',
  components: { 
    UserList
  },
  data(){
    return {
      userInfo:''
    }
  },
  methods:{
    clickUser(item){
      this.userInfo = item
      alert(this.userInfo.username)
    }
  }

}
</script>

```