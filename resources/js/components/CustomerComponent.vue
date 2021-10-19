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

                                    <button type="button" class="btn btn-primary" @click="create">Add New Customer
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
                                       <button @click="edit(customer)" type="button" class="btn btn-primary">Edit</button>
                                       <button @click="destroy(customer)" type="button" class="btn btn-danger">Delete</button>
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
         <vue-snotify></vue-snotify>
         <!-- Button trigger modal -->
        <!-- Modal -->
        <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                   <form @submit.prevent="store()" method="post">
                       <div class="form-group">
                         <label for="name">Name</label>
                         <input type="text"
                           class="form-control" v-model="form.name" name="name" id="name" placeholder="Enter Your Name">
                       </div>
                       <div class="form-group">
                         <label for="email">Email</label>
                         <input type="text"
                           class="form-control" v-model="form.email" name="email" id="email" placeholder="Enter Your email">
                       </div>
                       <div class="form-group">
                         <label for="phone">Phone</label>
                         <input type="number"
                           class="form-control" v-model="form.phone" name="phone" id="phone" placeholder="Enter Your phone">
                       </div>
                       <div class="form-group">
                         <label for="address">Address</label>
                         <textarea class="form-control" v-model="form.address" name="address" id="address" rows="3"></textarea>
                       </div>
                       <div class="form-group">
                         <label for="total">Total</label>
                         <input type="number"
                           class="form-control" v-model="form.total" name="total" id="total" placeholder="Enter Your Amount">
                       </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                            <button type="submit" class="btn btn-primary">Save changes</button>
                        </div>
                   </form>
                </div>
                </div>
            </div>
        </div>
        <!-- Edit Modal -->
        <div class="modal fade" id="EditModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                   <form @submit.prevent="update()" method="post">
                       <div class="form-group">
                         <label for="name">Name</label>
                         <input type="text"
                           class="form-control" v-model="form.name" name="name" id="name" placeholder="Enter Your Name">
                       </div>
                       <div class="form-group">
                         <label for="email">Email</label>
                         <input type="text"
                           class="form-control" v-model="form.email" name="email" id="email" placeholder="Enter Your email">
                       </div>
                       <div class="form-group">
                         <label for="phone">Phone</label>
                         <input type="number"
                           class="form-control" v-model="form.phone" name="phone" id="phone" placeholder="Enter Your phone">
                       </div>
                       <div class="form-group">
                         <label for="address">Address</label>
                         <textarea class="form-control" v-model="form.address" name="address" id="address" rows="3"></textarea>
                       </div>
                       <div class="form-group">
                         <label for="total">Total</label>
                         <input type="number"
                           class="form-control" v-model="form.total" name="total" id="total" placeholder="Enter Your Amount">
                       </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                            <button type="submit" class="btn btn-primary">Save changes</button>
                        </div>
                   </form>
                </div>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
import Form from 'vform'
    export default {
        data() {
            return {
                query: "",
                queryField: "name",
                customers: [],
                pagination: {
                        current_page:1,
                },
                form: new Form({
                    id: "",
                    name: "",
                    email: "",
                    phone: "",
                    address: "",
                    total: "",
                }),
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
                this.$snotify.success("Data Successfully Refresh", "Success");
            },
            create() {
                this.form.reset();
                this.form.clear();
                $("#exampleModal").modal("show");
            },
            store() {
                this.$Progress.start();
                this.form.busy = true;
                this.form
                .post('/api/customer')
                .then(response => {
                    this.getData();
                    $("#exampleModal").modal("hide");
                    if (this.form.successful) {
                     this.$Progress.finish();
                     this.$snotify.success("Customer Successfully Saved");
                    } else {
                     this.$Progress.fail();
                     this.$snotify.error(
                         "Something went wrong try again later.",
                         "Error"
                     );
                    }
                    console.log(response);
                })
                .catch(function (error) {
                    console.log(error);
                });
            },
            edit(customer) {
                this.form.reset();
                this.form.clear();
                this.form.fill(customer);
                $("#EditModal").modal("show");
            },
            update() {
                this.$Progress.start();
                this.form.busy = true;
                this.form
                .put('/api/customer/'+this.form.id)
                .then(response => {
                    this.getData();
                    $("#EditModal").modal("hide");
                    if (this.form.successful) {
                     this.$Progress.finish();
                     this.$snotify.success("Customer Update Successfully");
                    } else {
                     this.$Progress.fail();
                     this.$snotify.error(
                         "Something went wrong try again later.",
                         "Error"
                     );
                    }
                    console.log(response);
                })
                .catch(function (error) {
                    console.log(error);
                });
            },
            destroy(customer) {
                this.$snotify.clear();
                this.$snotify.confirm(
                    'You can not recover this data again!', 
                    'Are You Sure?', 
                    {
                        showProgressBar: false,
                        closeOnClick: false,
                        pauseOnHover: true,
                        buttons: [
                            {
                                text: 'Yes', 
                                action: (toast) => {
                                    this.$snotify.remove(toast.id);
                                    this.$Progress.start();
                                    this.form
                                    .delete('/api/customer/'+ customer.id)
                                    .then(response => {
                                        this.getData();
                                        this.$Progress.finish();
                                        this.$snotify.success("Customer Deleted Successfully");
                                        console.log(response);
                                    })
                                    .catch(function (error) {
                                        console.log(error);
                                    });
                                }, 
                                bold: true 
                            },
                            {text: 'No', action: () => console.log('Clicked: No')},
                            {text: 'Later', action: (toast) => {console.log('Clicked: Later'); this.$snotify.remove(toast.id); } },
                            {text: 'Close', action: (toast) => {console.log('Clicked: No'); this.$snotify.remove(toast.id); }, bold: true},
                        ]
                });
            }

        },
    }
</script>
