<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12">
                <div class="card">
                    <div class="card-header">
                        <div class="row">
                            <div class="col">
                                <h4>Customer Component</h4>
                            </div>
                            <div class="col">
                                <div class="float-right">

                                    <button type="button" class="btn btn-primary">Add New Customer
                                        <i class="fas fa-plus"></i>
                                    </button>
                                    <button @click="reload" type="button" class="btn btn-primary">Reload
                                        <i class="fas fa-sync"></i>
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="m-3">
                        <div class="row">
                            <div class="col-md-2">
                                <strong>Search By: </strong>
                            </div>
                            <div class="col-md-3">
                                <div class="form-group">
                                  <select v-model="queryField" class="form-control">
                                    <option value="name">Name</option>
                                    <option value="email">EMAIL</option>
                                    <option value="phone">PHONE</option>
                                    <option value="address">ADDRESS</option>
                                    <option value="total">TOTAL</option>
                                  </select>
                                </div>
                            </div>
                            <div class="col-md-7">
                                <input type="text" v-model="query" class="form-control" placeholder="Search">
                            </div>
                        </div>
                    </div>
                    <div class="card-body">
                       
                       <table class="table table-hover table-borderd table-striped">
                           <thead>
                               <tr>
                                   <th>ID</th>
                                   <th>NAME</th>
                                   <th>EMAIL</th>
                                   <th>PHONE</th>
                                   <th>ADDRESS</th>
                                   <th>TOTAL</th>
                                   <th>ACTION</th>
                               </tr>
                           </thead>
                           <tbody>
                               <tr v-for="(customer, index) in customers" :key="customer.id">
                                   <td>{{index+1}}</td>
                                   <td>{{customer.name}}</td>
                                   <td>{{customer.email}}</td>
                                   <td>{{customer.phone}}</td>
                                   <td>{{customer.address}}</td>
                                   <td>{{customer.total}}</td>
                                   <td>
                                       <button type="button" class="btn btn-primary">Edit</button>
                                       <button type="button" class="btn btn-danger">Delete</button>
                                   </td>
                               </tr>
                           </tbody>
                       </table>
                       <pagination v-if="pagination.last_page > 1"
                            :pagination="pagination"
                            :offset="5"
                            @paginate="query === '' ? getData() : searchData()"
                            ></pagination>
                    </div>
                </div>
            </div>
        </div>
         <vue-progress-bar></vue-progress-bar>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                query: "",
                queryField: "name",
                customers: [],
                pagination: {
                        current_page:1,
                }
            }
        },
        watch: {
            query: function(newQ, old){
                if (newQ === "") {
                    this.getData();
                } else {
                    this.searchData();
                }
            }
        },
        mounted() {
            console.log('Component mounted.')
            this.getData();
        },
        methods: {
            getData() {
                this.$Progress.start()
                axios.get('/api/customer?page='+this.pagination.current_page)
                .then(response => {
                    this.customers = response.data.data;
                    this.pagination = response.data.meta;
                    console.log(response)
                     this.$Progress.finish()
                })
                .catch(e => {
                    console.log(e);
                     this.$Progress.fail()
                })
            },
            searchData() {
                this.$Progress.start()
                axios.get('/api/search/customer' + '/' + this.queryField + '/' + this.query + '?page=' + this.pagination.current_page)
                .then(response => {
                    this.customers = response.data.data;
                    this.pagination = response.data.meta;
                    console.log(response)
                     this.$Progress.finish()
                })
                .catch(e => {
                    console.log(e);
                     this.$Progress.fail()
                })
            },
            reload() {
                this.$Progress.start()
                this.getData();
                this.query = "";
                this.queryField = "name";
            }

        },
    }
</script>
