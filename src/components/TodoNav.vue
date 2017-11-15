<template>
  <div>
      <el-row>
          <el-col span="24">
              <h1>{{title}}</h1>
          </el-col>
      </el-row>
  </div>
</template>

<script>
export default {
  name: 'TodoNav',
  data () {
      return {
        title: '待办清单'
      } 
  }
}
</script>

<style scoped>
h1{
    text-align: center;
    color: white;
}
.el-col{
    background-color:#2D2F33;
}
</style>
