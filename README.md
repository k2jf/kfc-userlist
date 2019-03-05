# kfc-userlist
获取用户列表

# git 相关
```
# pull
git remote add -f kfc-userlist git@github.com:k2jf/kfc-userlist.git
git subtree add -P src/components/UserList kfc-userlist master --squash

# push
git subtree push -P src/components/UserList kfc-userlist master

```

# 使用方法
```
<template>
<div>
    <UserList />
</div>
</template>

<script>
import UserList from '@/components/UserList'

components: {'UserList'}

</script>

```