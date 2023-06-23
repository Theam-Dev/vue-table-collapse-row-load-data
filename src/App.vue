<template>
  <nav class="navbar navbar-expand-sm bg-dark navbar-dark">
  <a class="navbar-brand" href="#">Vue table collapse load data</a>
  <ul class="navbar-nav">
  </ul>
</nav>
<div class="container-fluid">
  <table class="table">
    <thead>
      <tr>
        <th width="20">Id</th>
        <th width='200'>Name</th>
        <th>Email</th>
      </tr>
    </thead>
    <tbody id="tableData">
      <tr v-for="(val,index) in itemData" :key="index" v-on:click="collapse(val.id,index)" style="background-color: #c5c5c5; cursor: pointer;" :id="'testApp'+index">
        <td>{{ val.id }}</td>
        <td>{{ val.name }}</td>
        <td>{{ val.email }}</td>
      </tr>
      <tr v-for="(val,index) in itemData" :key="index" :class="'collapse demo_'+index">
      </tr>
    </tbody>
  </table>
</div>
</template>

<script>
import $ from 'jquery';
import axios from 'axios';
export default {
  name: 'App',
  data(){
    return {
      itemData : {}
    }
  },
  methods:{
    collapse(id,index){
      $("#tableData .demo_"+index).toggleClass("show");
      if($("#tableData .demo_"+index).hasClass("show")){
        $("#tableData .demo_"+index).remove();
        axios.get("https://jsonplaceholder.typicode.com/posts/"+id)
        .then(function(res){
            var item = res.data;
            $("#testApp"+index).after(
              "<tr class='demo_"+index+" collapse show'>" +
                  "<td width='20'></td>" +
                  "<td width='200'>"+item.title+"</td>" +
                  "<td>"+item.body+"</td>" +
              "</tr>"
            )
        })
        .catch(function(er){
          console.log(er);
        })
      }
    }
  },
  mounted(){
    var app = this;
    axios.get("https://jsonplaceholder.typicode.com/users")
    .then(function(res){
        app.itemData = res.data;
    })
    .catch(function(er){
      console.log(er);
    })
  }
}
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }
  .spinner-border {
      display: inline-block;
      width: 2rem;
      height: 2rem;
      vertical-align: text-bottom;
  }
</style>
