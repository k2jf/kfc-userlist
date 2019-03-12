# kfc-userlist
获取用户列表
用户列表带搜索，不区分大小写，改变即搜索。搜索不会再次调用后台接口。

# git 相关
```
# 初次clone
git remote add -f kfc-userlist git@github.com:k2jf/kfc-userlist.git
git subtree add -P src/components/kfc-userlist kfc-userlist master --squash
or
git subtree add -P src/components/kfc-userlist git@github.com:k2jf/kfc-userlist.git master --squash

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
  methods:{
    clickUser(item){
      alert(item.username)
    }
  }

}
</script>

```