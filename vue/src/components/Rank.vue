<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-10">
                <h3 class="text-center text-dark mt-2">Library (Rank List)</h3>
            </div>
        </div>
        <div class="row">
            <div class="col-md-4">
                <div class="card-header">
                    Add rank
                </div>
                <div class="card-body">
                    <form @submit.prevent="save">
                     
                    <div class="form-group">
                        <label>Code</label>
                        <input type="text" v-model="rank_data.code" class="form-control"  placeholder="Enter Code">
                     
                    </div>
                    <div class="form-group">
                        <label>Rank Name</label>
                        <input type="text" v-model="rank_data.rank_name" class="form-control"  placeholder="Enter Rank Name">
                     
                    </div>
                    <div class="form-group">
                        <label>Alias</label>
                        <input type="text" v-model="rank_data.alias" class="form-control"  placeholder="Enter Alias">
                     
                    </div>
                     
                    <button type="submit" class="btn btn-primary">Save</button>
                    </form>
                </div>
            </div>
            <div class="col-md-8">
                <h2>rank List</h2>
                    <table class="table table-dark">
                    <thead>
                        <tr>
                        <th scope="col">Code</th>
                        <th scope="col">Rank Name</th>
                        <th scope="col">Alias</th>
                        <th scope="col">Actions</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="rank_data in result" v-bind:key="rank_data.id">
                             
                            <td>{{ rank_data.code }}</td>
                            <td>{{ rank_data.rank_name }}</td>
                            <td>{{ rank_data.alias }}</td>
                            <td>
                                <button type="button" class="btn btn-warning" @click="edit(rank_data)">Edit</button>
                                <button type="button" class="btn btn-danger"  @click="remove(rank_data.id)">Delete</button>
                            </td>
                            </tr>
                         
                    </tbody>
                    </table>
            </div>
        </div>
    </div>    
  </template>
  <script>
  import axios from 'axios';
  
  export default {
    name: 'rank',
    data () {
      return {
        result: {},
        rank_data:{
                   id: '',
                   code: '',
                   rank_name: '',
                   alias: ''
  
  
        }
      }
    },
    created() { 
        this.rankLoad();
    },
    mounted() {
          console.log("mounted() called.......");
    },
  
    methods: {
      rankLoad()
           {
                 var page = "http://localhost:8000/api/ranks";
                 axios.get(page)
                  .then(
                      ({data})=>{
                        console.log(data);
                        this.result = data.rank_data;
                      }
                  );
           },
            
           save()
           {
            if(this.rank_data.id == '')
              {
                this.saveData();
              }
              else
              {
                this.updateData();
              }       
  
           },
           saveData()
           {
            
            axios.post("http://localhost:8000/api/ranks", this.rank_data)
            .then(
              ({data})=>{
                alert("saved");
                this.rankLoad();
                this.rank_data.rank_name = '';
                this.rank_data.code = '',
                this.rank_data.alias = '',
                this.id = ''
              }
            )
  
           },
           edit(rank_data)
           {
            
            this.rank_data = rank_data;
           },
           updateData()
           {
            
              var editrecords = 'http://localhost:8000/api/ranks/'+ this.rank_data.id+'/edit';
              axios.put(editrecords, this.rank_data)
              .then(
                ({data})=>{
                    this.rank_data.rank_name = '';
                this.rank_data.code = '',
                this.rank_data.alias = '',
                  this.id = ''
                  alert("Updated!!!");
                  this.rankLoad();
                }
              );
  
           },
  
           remove(rank_id){
         
              var url = 'http://127.0.0.1:8000/api/ranks/'+ rank_id + '/delete';
              axios.delete(url);
              alert("Deleted");
              this.rankLoad();
            }
      }
  }
  </script>