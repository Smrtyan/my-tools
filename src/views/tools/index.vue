<template>
  <div>
    <h2>Generate csb url</h2>
    <h3>env</h3>
    <div>
      <el-radio v-for="(value, name) in env_list" :key="name" v-model="env" :label="name">{{ value }}</el-radio>
    </div>
    <h3>db type</h3>
    <div>
      <el-radio v-for="x in db_type_list" :key="x" v-model="dbType" :label="x">{{ x }}</el-radio>
    </div>

    <h3>enterprise</h3>
    <div>
      <el-radio v-for="(value, name) in enterprise_list" :key="name" v-model="enterprise" :label="name">{{ value }}</el-radio>
    </div>
    <h3>account</h3>
    <div>
      <el-radio v-for="(value, name) in account_list" :key="name" v-model="account" :label="name">{{ value }}</el-radio>
    </div>

    <h3>region</h3>
    <div>
      <el-radio v-for="(value, name) in region_list" :key="name" v-model="region" :label="name">{{ value }}</el-radio>
    </div>

    <h3>csb url</h3>
    <div>{{ csb_url }}</div>
    <el-button
      v-clipboard:copy="csb_url"
      v-clipboard:success="firstCopySuccess"
      v-clipboard:error="firstCopyError"
      type="success"
      size="mini"
      circle
    >Copy</el-button>
  </div>

</template>

<script>

import service from '@/utils/my-request'
export default {

  data() {
    return {
      db_type_list: ['MySQL', 'PostgreSQL', 'MongoDB', 'openGauss'],
      env_list: [],
      enterprise_list: [],
      account_list: [],
      region_list: [],
      dbType: 'MySQL',
      env: '',
      enterprise: '',
      account: '',
      region: '',
      csb_url: ''
    }
  },
  watch: {
    env(val) {
      console.log('env change' + val)
      this.enterprise = ''
      this.account = ''
      this.region = ''
      this.csb_url = ''
      // get enterprise
      service.get('http://192.168.0.114:8000/api/enterprise?env=' + this.env).then((data) => {
        console.log(data.data)
        this.enterprise_list = data.data
        console.log(Object.keys(this.enterprise_list)[0])
        this.enterprise = Object.keys(this.enterprise_list)[0]
      })
    },
    enterprise(val) {
      console.log('enterprise change' + val)
      this.account = ''
      this.region = ''
      this.csb_url = ''

      // get account
      service.get('http://192.168.0.114:8000/api/account?env=' + this.env + '&enterprise=' + this.enterprise).then((data) => {
        console.log(data.data)
        this.account_list = data.data
        console.log(Object.keys(this.account_list)[0])
        this.account = Object.keys(this.account_list)[0]
      })
    },
    account(val) {
      console.log('account change' + val)

      this.region = ''
      this.csb_url = ''
      // get region
      service.get('http://192.168.0.114:8000/api/region?env=' + this.env + '&enterprise=' + this.enterprise + '&account=' + this.account).then((data) => {
        console.log(data.data)
        this.region_list = data.data
        console.log(Object.keys(this.region_list)[0])
        this.region = Object.keys(this.region_list)[0]
      })
    },
    region(val) {
      console.log('region change' + val)
      // generate csb url
      service.get('http://192.168.0.114:8000/api/url?env=' + this.env + '&enterprise=' + this.enterprise + '&account=' + this.account + '&region=' + this.region + '&dbType=' + this.dbType).then((data) => {
        console.log(data.data)
        this.csb_url = data.data
      })
    },
    dbType(val) {
      console.log('region change' + val)
      // generate csb url
      service.get('http://192.168.0.114:8000/api/url?env=' + this.env + '&enterprise=' + this.enterprise + '&account=' + this.account + '&region=' + this.region + '&dbType=' + this.dbType).then((data) => {
        console.log(data.data)
        this.csb_url = data.data
      })
    }
  },
  mounted() {
    service.get('http://192.168.0.114:8000/api/env/').then((data) => {
      console.log(data.data)
      this.env_list = data.data
      console.log(Object.keys(this.env_list)[0])
      this.env = Object.keys(this.env_list)[0]
    })
  },
  methods: {
    go() {
      service.get('http://192.168.0.114:8000/api/hello/').then((data) => {
        console.log(data)
      })
    }, firstCopySuccess(e) {
      console.log('copy arguments e:', e)
      this.$message('复制成功')
    },
    firstCopyError(e) {
      console.log('copy arguments e:', e)
      this.$message('复制失败')
    }

  }
}
</script>
