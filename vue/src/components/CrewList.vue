<template>
    <div>
        <h1>Crew List</h1>
        <DataTable
            class="table table-hover table-striped"
            id="datatable"
            :columns="columns"
            ajax="http://localhost:8000/api/crews"
            ref="table"
  
        >
        </DataTable>
        <RouterView/>
    </div>
  </template>
  <script>
  import { ref, onMounted } from 'vue';
  import DataTable from 'datatables.net-vue3';
  import DataTablesLib from 'datatables.net';
  import axios from 'axios';
  import 'datatables.net-select';
  
  DataTable.use(DataTablesLib);

  

  
  
  export default {
    name: "DataTableComponent",
    components: {DataTable},
    data(){
        return {
          columns: [
                {"data": "first_name","title" : "First Name"},
                {"data": "middle_name","title" : "Middle Name"},
                {"data": "last_name","title" : "Last Name"},
                {"data": "email","title" : "email"},
                {"data": "address","title" : "Address"},
                {"data": "birthdate","title" : "Birth Date"},
                {"data": "age","title" : "Age"},
                {"data": "rank","title" : "Rank"},
                {"data": "height","title" : "Height"},
                {"data": "weight","title" : "Weight"},
                {"data": null,"title" : "Actions", "defaultContent": '<router-link class-active="active" to="/delete" exact>TIME</router-link>'}  
                
        ],
            dt: null,
  
            data:{
                     id: '',
                     code: '',
                     name: ''
    
    
          }
        }
    },  mounted() {
    
      },
    methods: {
          
             save()
             {
              if(this.data.id == '')
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
              axios.post("http://localhost:8000/api/crews", this.data)
              .then(
                ({data})=>{
                  alert("saved");
                  this.data.code = '';
                  this.data.name = '',
                  this.id = ''
                  $('#datatable').DataTable().draw;
                }
              )
    
             },
             edit(id)
             {
              this.data = data;
             },
             updateData()
             {
                var editrecords = 'http://localhost:8000/api/crews/'+ this.data.id+'/edit';
                axios.put(editrecords, this.data)
                .then(
                  ({data})=>{
                    this.data.name = '';
                    this.data.address = '',
                    this.data.phone = ''
                    this.id = ''
                    alert("Updated!!!");
                    
                  }
                );
    
             },
    
             remove(){
             
     
                var url = `http://localhost:8000/api/crews/${data.id}/delete`;
                axios.delete(url);
                
                alert("Deleted");
               
              }
        }
  }
  
  </script>
  
  <style>
  @import 'datatables.net-dt';
  </style>