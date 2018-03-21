<template>
  <div class="hello">
    <button @click="$store.commit('add',10)">+</button>
    <button @click="$store.commit('reduce')">-</button>
    <h3>{{$store.state.count}}</h3>
    <el-button @click="ctrlShow">Click Me</el-button>
    <div style="display: flex; margin-top: 20px; height: 100px;">
      <transition name="el-fade-in-linear">
        <div v-show="show" class="transition-box">.el-fade-in-linear</div>
      </transition>
      <transition name="el-fade-in">
        <div v-show="show" class="transition-box">.el-fade-in</div>
      </transition>
    </div>
    <div>
      <i class="el-icon-edit"></i>
      <i class="el-icon-share"></i>
      <i class="el-icon-delete"></i>
      <el-button type="primary" icon="el-icon-search">Search</el-button>
      <el-button type="primary" :loading="loading" @click="serviceLoading">Loading</el-button>
    </div>
    <h2>table</h2>
    <div>
      <el-table
        ref="singleTable"
        v-loading="loading"
        element-loading-text="拼命加载中"
        element-loading-spinner="el-icon-loading"
        element-loading-background="rgba(0, 0, 0, 0.8)"
        :data="tableData"
        highlight-current-row
        @current-change="handleCurrentChange"
        style="width: 100%">
        <el-table-column
          type="index"
          width="50">
        </el-table-column>
        <el-table-column
          property="date"
          label="日期"
          width="120">
        </el-table-column>
        <el-table-column
          property="name"
          label="姓名"
          width="120">
        </el-table-column>
        <el-table-column
          property="address"
          label="地址">
        </el-table-column>
      </el-table>
      <div style="margin-top: 20px">
        <el-button @click="setCurrent(tableData[1])">选中第二行</el-button>
        <el-button @click="setCurrent()">取消选择</el-button>
      </div>
    </div>
    <h2>message</h2>
    <div>
      <el-button :plain="true" @click="openMessage">消息提示</el-button>
      <el-button :plain="false" @click="openVnMessage">VNode</el-button>
      <el-button :plain="false" @click="showAlert">alert</el-button>
      <el-button :plain="false" @click="showConfirm">确定框</el-button>
      <el-button :plain="false" @click="showPrompt">输入框</el-button>
    </div>
  </div>
</template>

<script>
import { Loading } from 'element-ui'

import store from '@/vuex/store'

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      show: true,
      loading: false,
      tableData: [{
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-04',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1517 弄'
      }, {
        date: '2016-05-01',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1519 弄'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1516 弄'
      }],
      currentRow: null
    }
  },
  store,
  methods: {
    setCurrent (row) {
      this.$refs.singleTable.setCurrentRow(row)
    },
    handleCurrentChange (val) {
      this.currentRow = val
    },
    ctrlShow () {
      this.show = !this.show
      store.commit('increment')
      console.log(store.state.count)
    },
    loadData () {
      this.loading = !this.loading
    },
    serviceLoading () {
      let options = {
        fullscreen: true,
        lock: true
      }
      let loadingInstance = Loading.service(options)
      this.$nextTick(() => { // 以服务的方式调用的 Loading 需要异步关闭
        alert('更新完毕，即将关闭')
        window.setTimeout(() => {
          loadingInstance.close()
        }, 2000)
      })
    },

    openMessage () {
      this.$message('这是一条消息')
    },

    openVnMessage () {
      const h = this.$createElement
      this.$message({
        showClose: true,
        duration: 5000,
        type: 'warning',
        message: h('p', null, [
          h('span', null, '内容可以是 '),
          h('i', {style: 'color: red; font-size: 40px'}, '自定义内容')
        ])
      })
    },

    showAlert () {
      this.$alert('this is message', 'Title', {
        confirmButtonText: '知道啦',
        callback: action => {
          this.$message({
            type: 'info',
            message: 'you click ok'
          })
        }
      })
    },

    showConfirm () {
      this.$confirm('are you sure to delete this record?', 'Warning', {
        confirmButtonText: 'sure',
        cancelButtonText: 'give up',
        type: 'warming'
      }).then(() => {
        this.$message({
          type: 'success',
          message: 'delete success'
        })
      }).catch(() => {
        this.$message({
          type: 'warn',
          message: 'cancel'
        })
      })
    },

    showPrompt () {
      this.$prompt('请输入你的邮箱', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '关闭',
        inputPattern: /[\w!#$%&'*+/=?^_`{|}~-]+(?:\.[\w!#$%&'*+/=?^_`{|}~-]+)*@(?:[\w](?:[\w-]*[\w])?\.)+[\w](?:[\w-]*[\w])?/,
        inputErrorMessage: '地址错误'
      }).then(res => {
        this.$message({
          type: 'success',
          message: '输入成功' + res.value
        })
      }).catch(() => {
        this.$message({
          type: 'error',
          message: '取消'
        })
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.transition-box {
  margin-bottom: 10px;
  width: 200px;
  height: 100px;
  border-radius: 4px;
  background-color: #409EFF;
  text-align: center;
  color: #fff;
  padding: 40px 20px;
  box-sizing: border-box;
  margin-right: 20px;
}
</style>
