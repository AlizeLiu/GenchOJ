<template>
  <el-card>
    <div slot="header">
      <b> {{school}}欢迎你</b>
      <h3>欢迎访问内测版GenchOJ</h3>
    </div>
    <b>Version：0.0.1</b>
    <br>
  </el-card>
</template>

<script>
export default {
  name: "welcomemessage",
  data() {
    return {
      school:"GenchOJ",
    };
  },
  created() {


    var sb = this.$store.state.sb
    if( sb ==undefined){
      this.$axios
      .get("/settingboard/")
      .then(res => {
        if (res.data.length > 0) this.school = res.data[0].ojname;
        else this.school = "GenchOJ";
        this.$store.state.sb = res.data
      })
      .catch(error => {
        this.$message.error(
          "使用服务必须登录！"
        );
      });
    }
    else{
      if (sb.length > 0) this.school = sb[0].ojname;
        else this.school = "GenchOJ";
    }


  },
};
</script>

<style  scoped>
</style>