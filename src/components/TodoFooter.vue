<template>
  <div>
      <el-row>
          <el-col span="24">
              <p>{{todofooter}}</p>
          </el-col>
      </el-row>
  </div>
</template>

<script>
export default {
  name: 'TodoFooter',
  data () {
      return {
        todofooter: '2017 Design and code by Krad Zhang'
      } 
  }
}
</script>

<style scoped>
p{
    text-align: center;
    color: white;
}
.el-col{
    background-color:#2D2F33;
}
</style>
