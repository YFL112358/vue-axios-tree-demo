<template>
  <div class="demo">
    <button class="btn" type="button" name="button" @click="getData">点击</button>
    <el-tree :data="datas" :props="defaultProps" node-key="id" ref="tree" @node-click="handleNodeClick"></el-tree>
     <ul v-show="isShowNativeResult">
      <li v-for="(data,index) in realDatas">
        <span>
          {{data.objectValue}}
        </span>
       
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Demo',
  data () {
    return {
       datas: [],
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
        that.realDatas = response.data
        this.isShowNativeResult = true;
        this.$message.success(response.data)
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
  background-color: red;
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
