<template lang="html">
  <Menu mode="horizontal" theme="primary" :active-name="activeName" @on-select="onSelect">
    <span class="header-userName">{{ userAccount }}</span>
    <Submenu name="3">
        <template slot="title">
            <Icon type="ios-person"></Icon>
            个人中心
        </template>
        <MenuGroup title="使用">
            <MenuItem name="3-1">我的项目</MenuItem>
            <MenuItem name="3-2">帮助文档</MenuItem>
        </MenuGroup>
        <MenuGroup title="操作">
            <MenuItem name="3-3">检查更新</MenuItem>          
            <MenuItem name="3-4">登出</MenuItem>
        </MenuGroup>
    </Submenu>
    <MenuItem name="2">
        <Icon type="stats-bars"></Icon>
        地图库
    </MenuItem>
    <MenuItem name="1">
        <Icon type="plus"></Icon>
        新地图
    </MenuItem>
  </Menu>
</template>


<script>
import storage from 'store'
import { mapState } from 'vuex'
export default {
  name: "Header",
  props: {
    activeName: {
      type: String,
      default: '1'
    }
  },
  data() {
    return {
    }
  },
  computed: mapState({
    userAccount: state => state.userInfo.userInfo.email || storage.get('userInfo').email,
    countAlias: 'userAccount'
  }),
  methods: {
    onSelect(value) {
      if(value === '1')
        this.$router.push({ name: 'Home' })
      if(value === '2')
        this.$router.push({ name: 'Map' })
      if(value === '3-1')
        this.$router.push({ name: 'MyProject'})
      if(value === '3-2')
        this.$router.push({ name: 'Help' })
      if(value === '3-3')
        this.$router.push({ name: 'Update' })
      if(value === '3-4') {
        this.$Message.success('登出成功,再见👋')
        storage.remove('userInfo')
        storage.remove('userId')
        storage.remove('accessToken')
        this.$router.push({
          name: 'Login'
        })
      }
    }
  }
}
</script>


<style lang="less" scoped>
@import '../../assets/my-theme/custom.less';
  .header-userName {
    padding: 0 20px;
    font-weight: 600;
    cursor: default;
  }
</style>
