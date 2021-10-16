<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12">
                <div class="card">
                    <div class="card-header">Customer Component</div>

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
                            @paginate="getData()"
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
                customers: [],
                pagination: {
                        current_page:1,
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
            }
        },
    }
</script>
