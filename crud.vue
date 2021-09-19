<template>
<div class="container" id="crudApp">
   <br />
   <h3 align="center">Delete or Remove Data From Mysql using Vue.js with PHP</h3>
   <br />
   <div class="panel panel-default">
    <div class="panel-heading">
     <div class="row">
      <div class="col-md-6">
       <h3 class="panel-title">Sample Data</h3>
      </div>
      <div class="col-md-6" align="right">
       <input type="button" class="btn btn-success btn-xs" @click="openModel" value="Add" />
      </div>
     </div>
    </div>
    <div class="panel-body">
     <div class="table-responsive">
      <table class="table table-bordered table-striped">
       <tr>
        <th>First Name</th>
        <th>Last Name</th>
        <th>Edit</th>
        <th>Delete</th>
       </tr>
       <tr v-for="row in allData">
        <td>{{ row.first_name }}</td>
        <td>{{ row.last_name }}</td>
        <td><button type="button" name="edit" class="btn btn-primary btn-xs edit" @click="fetchData(row.id)">Edit</button></td>
        <td><button type="button" name="delete" class="btn btn-danger btn-xs delete" @click="deleteData(row.id)">Delete</button></td>
       </tr>
      </table>
     </div>
    </div>
   </div>
   <div v-if="myModel">
    <transition name="model">
     <div class="modal-mask">
      <div class="modal-wrapper">
       <div class="modal-dialog">
        <div class="modal-content">
         <div class="modal-header">
          <button type="button" class="close" @click="myModel=false"><span aria-hidden="true">&times;</span></button>
          <h4 class="modal-title">{{ dynamicTitle }}</h4>
         </div>
         <div class="modal-body">
          <div class="form-group">
           <label>Enter First Name</label>
           <input type="text" class="form-control" v-model="first_name" />
          </div>
          <div class="form-group">
           <label>Enter Last Name</label>
           <input type="text" class="form-control" v-model="last_name" />
          </div>
          <br />
          <div align="center">
           <input type="hidden" v-model="hiddenId" />
           <input type="button" class="btn btn-success btn-xs" v-model="actionButton" @click="submitData" />
          </div>
         </div>
        </div>
       </div>
      </div>
     </div>
    </transition>
   </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
    name: 'crudApp',
data() {
    return {
      allData:'',
      Users:[],
    myModel:false,
  actionButton:'Insert',
  dynamicTitle:'Add Data',
    };
  },
  
  methods:{
       fetchAllData(){
   axios.post('http://localhost:8080/appvue/src/Api/read.php', {
    action:'fetchall'
   }).then((res)=>{
    app.allData = res.data;
   });
  },
    deleteUser(userid){
      axios.post('http://localhost:8080/appvue/src/Api/read.php', {
        action:'delete',
        id:userid
      })
      .then((res)=>{
        if(res.data.error){
          alert("Errorka");
        }else{
      app.fetchAllData();
          alert(res.data.message);
        }
      }).catch((err)=>{
        console.log(err);
      })
    },
    openModel(){
   this.first_name = '';
   this.last_name = '';
   this.actionButton = "Insert";
   this.dynamicTitle = "Add Data";
   this.myModel = true;
  },
 fetchData(id){
   axios.post('http://localhost:8080/appvue/src/Api/read.php', {
    action:'fetchSingle',
    id:id
   }).then((res)=>{
    this.first_name = res.data.first_name;
    this.last_name = res.data.last_name;
    this.hiddenId = res.data.id;
    this.myModel = true;
    this.actionButton = 'Update';
    this.dynamicTitle = 'Edit Data';
   });
  },

     created(){
  this.fetchAllData();
 },
}
}
</script>

<style>

</style>
