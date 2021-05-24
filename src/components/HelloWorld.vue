<template>
  <div class="demo">
    <button class="btn" type="button" name="button" @click="getData">点击</button>
   
    <div class="left">
      <el-tree :data="datas" :props="defaultProps" node-key="id" ref="tree" @node-click="handleNodeClick"></el-tree>
    </div>
    <div class="from_box">
     <div class="form-sub">
    <form action="" method="post" id="form" class="form">
      <input  v-for="(em,index) in realDatas" :name="em.objectName" v-model="em.objectValue" :key="index"/>
    </form>
    </div>
    <span class="but" @click="onSubmit()">提交</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Demo',
  data () {
    return {
       datas: [],
       realDatas:[],
        defaultProps: {
          children: 'children',
          label: 'showName'
        },
        isShowNativeResult:false
    }
  },
 
  methods:{
    getData() {
      var that = this
      this.$axios.get('v1/getIceData')
        .then(response =>{
          console.log(response.data)
          that.datas = response.data
        })
        .catch(error =>{
          console.log(error)
        })
    },
    handleNodeClick(data) {
      var ridsb = this.$refs.tree
      console.log(ridsb);
      console.log(ridsb.getNodePath())
      if (data.fc == null) {
        console.log("dddd")
        return
      }
      var requestBody = [{
        objectName : data.label,
        fc : data.fc
      }]
      this.$axios.post('v1/getRealIceData', requestBody)
      .then(response =>{
        console.log(response.data)
        this.realDatas = response.data
        this.isShowNativeResult = true;
      })
      .catch(error =>{
        console.log(error)
      })
    },
    onSubmit() {
      /* json格式提交： */
      // let formData = JSON.stringify(this.formMess);
 
      /* formData格式提交： */
      this.$axios.post('v1/updateIceData', this.realDatas)
      .then(response =>{
        console.log(response.data)
      })
      .catch(error =>{
        console.log(error)
      })
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.btn {
  padding: 5px;
  background-color: rgb(226, 63, 63);
}
.left {
  float: left;
}
.right {
  float: right;
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
.listItem {
  display:flex;
  flex-direction: column;
  justify-content: center;
  align-items:center;
  border:1px solid #ddd;
  margin-top:5px
}
</style>
