<template>
  <div class="container">
      <div class="row justify-content-center">
          <div class="col-md-10">
              <h3 class="text-center text-dark mt-2">Library (Document List)</h3>
          </div>
      </div>
      <div class="row">
          <div class="col-md-4">
              <div class="card-header">
                  Add Document
              </div>
              <div class="card-body">
                  <form @submit.prevent="save">
                   
                  <div class="form-group">
                      <label>Code</label>
                      <input type="text" v-model="document_data.code" class="form-control"  placeholder="Enter Code">
                   
                  </div>
                  <div class="form-group">
                      <label>Document Name</label>
                      <input type="text" v-model="document_data.name" class="form-control"  placeholder="Enter Document Name">
                   
                  </div>
                   
                  <button type="submit" class="btn btn-primary">Save</button>
                  </form>
              </div>
          </div>
          <div class="col-md-8">
              <h2>Document List</h2>
                  <table class="table table-dark">
                  <thead>
                      <tr>
                      <th scope="col">Code</th>
                      <th scope="col">Document Name</th>
                      <th scope="col">Actions</th>
                      </tr>
                  </thead>
                  <tbody>
                      <tr v-for="document_data in result" v-bind:key="document_data.id">
                           
                          <td>{{ document_data.code }}</td>
                          <td>{{ document_data.name }}</td>
                          <td>
                              <button type="button" class="btn btn-warning" @click="edit(document_data)">Edit</button>
                              <button type="button" class="btn btn-danger"  @click="remove(document_data.id)">Delete</button>
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
  name: 'Document',
  data () {
    return {
      result: {},
      document_data:{
                 id: '',
                 code: '',
                 name: ''


      }
    }
  },
  created() { 
      this.DocumentLoad();
  },
  mounted() {
        console.log("mounted() called.......");
  },

  methods: {
    DocumentLoad()
         {
               var page = "http://localhost:8000/api/documents";
               axios.get(page)
                .then(
                    ({data})=>{
                      console.log(data);
                      this.result = data.document_data;
                    }
                );
         },
          
         save()
         {
          if(this.document_id == '')
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
          
          axios.post("http://localhost:8000/api/documents", this.document_data)
          .then(
            ({data})=>{
              alert("saved");
              this.DocumentLoad();
              this.document_data.name = '';
              this.document_data.code = '',
              this.id = ''
            }
          )

         },
         edit(document_data)
         {
          
          this.document_data = document_data;
         },
         updateData()
         {
          
            var editrecords = 'http://localhost:8000/api/documents/'+ this.document_data.id+'/edit';
            axios.put(editrecords, this.document_data)
            .then(
              ({data})=>{
                this.document_data.name = '';
                this.document_data.code = '',
                this.id = ''
                alert("Updated!!!");
                this.DocumentLoad();
              }
            );

         },

         remove(doc_id){
       
            var url = 'http://127.0.0.1:8000/api/documents/'+ doc_id + '/delete';
            axios.delete(url);
            alert("Deleted");
            this.DocumentLoad();
          }
    }
}
</script>