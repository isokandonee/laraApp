<template>
    <div class="container">
        <div class="row mt-5">
            <div class="col-sm-12">
                <!-- .card -->
                <div class="card">
              <div class="card-header">
                <h3 class="card-title">Users Table</h3>

                <div class="card-tools">
                  <button class="btn btn-success" @click="newModal">
                      Add New
                      <i class="fas fa-user-plus fa-fw"></i>
                  </button>
                </div>
              </div>
              <!-- /.card-header -->
              <div class="card-body table-responsive p-0">
                <table class="table table-hover">
                  <thead>
                    <tr>
                      <th>ID</th>
                      <th>Name</th>
                      <th>Email</th>
                      <th>Type</th>
                      <th>Registered Date</th>
                      <th>Modify</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="user in users" :key="user.id">
                      <td>{{ user.id }}</td>
                      <td>{{ user.name | upText }}</td>
                      <td>{{ user.email }}</td>
                      <td>{{ user.type | upText }}</td>
                      <td>{{ user.created_at }}</td>
                      <td>
                          <a href="#" @click="editModal(user)">
                              <i class="fa fa-edit"></i>
                          </a>
                          ||
                          <a href="#" @click="deleteUser(user.id)">
                              <i class="fa fa-trash red"></i>
                          </a>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
              <!-- /.card-body -->
            </div>
                <!-- .card -->
            </div>
        </div>

        <!--Create User & Update Modal -->
        <div class="modal fade" id="addNew" tabindex="-1" role="dialog" aria-labelledby="addNewLabel" aria-hidden="true">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 v-show="!editmode" class="modal-title" id="addNewLabel">Add New User</h5>
                        <h5 v-show="editmode" class="modal-title" id="addNewLabel">Update User's Info</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <form @submit.prevent="editmode ? updateUser() : createUser()">
                      <div class="modal-body">
                          
                          <div class="form-group">
                            <input v-model="form.name" placeholder="Full Name" type="text" name="name"
                              class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
                            <has-error :form="form" field="name"></has-error>
                          </div>

                          <div class="form-group">
                            <input v-model="form.email" placeholder="Email Address" type="email" name="email"
                              class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
                            <has-error :form="form" field="email"></has-error>
                          </div>

                          <div class="form-group">
                            <textarea v-model="form.bio" placeholder="Short bio for user (Optional)" type="text" name="bio"
                              class="form-control" :class="{ 'is-invalid': form.errors.has('bio') }">
                            </textarea>
                            <has-error :form="form" field="bio"></has-error>
                          </div>
                          
                          <div class="form-group">
                            <select v-model="form.type" type="type" name="type"
                              class="form-control" :class="{ 'is-invalid': form.errors.has('type') }">
                            <option value="">Type...</option>
                            <option value="admin">Admin</option>
                            <option value="user">Standard User</option>
                            <option value="author">Author</option>
                            </select>
                            <has-error :form="form" field="name"></has-error>
                          </div>

                          <div class="form-group">
                            <input v-model="form.password" placeholder="Password" type="password" name="password"
                              class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
                            <has-error :form="form" field="password"></has-error>
                          </div>
                      </div>
                      <div class="modal-footer">
                          <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
                          <button v-show="!editmode" type="submit" class="btn btn-primary">Create</button>
                          <button v-show="editmode" type="submit" class="btn btn-success">Update</button>
                      </div>
                    </form>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
    export default {
      data() {
        return {
          editmode: false,
          users : {},
          form: new Form({
            id : '',
            name : '',
            email : '',
            password : '',
            type : '',
            bio : '',
            photo : '',
          })
        }
      },
      methods: {
        newModal() {
          this.editmode = false;
          this.form.reset();
            $('#addNew').modal('show');
        },
        editModal(user) {
          this.editmode = true;
          this.form.reset();
            $('#addNew').modal('show');
            this.form.fill(user);
        },

        updateUser() {
          
          this.$Progress.start();
          this.form.put('api/user/'+this.form.id)
          .then(()=>{
            this.$Progress.finish();

            AddEvent.$emit('AfterUpdate');

            $('#addNew').modal('hide');            
            
            Swal.fire(            
              'Updated!',
              'Account Was Updated Successful',
              'success',
            );

          })
          .catch(()=> {
            this.$Progress.fail();
            $('#addNew').modal('hide');
            Swal.fire(            
              'Not Updated!',
              'Account Update Was Not Successful',
              'error',
            );
          })
        },

        deleteUser(id) {
          Swal.fire({
            title: 'Are you sure?',
            text: "You won't be able to revert this",
            icon: 'warning',
            showCancelButton: true,
            confirmButtonColor: '#3085d6',
            cancelButtonColor: '#d33',
            confirmButtonText: 'Yes, delete it?',
          })
          .then((result) => {
            // Send request to the server
            if (result.value) {
                this.form.delete('api/user/'+id)
                .then(() => {

                    Swal.fire(
                      'Deleted!',
                      'Your file has been deleted',
                      'success',
                    );

                    AddEvent.$emit('AfterDelete');

                })
                .catch(() => {
                  Swal.fire(
                      'Failed!',
                      'Please try again.',
                      'warning',
                    )
                });
            }    

            
          })
        },

        loadUsers() {
          axios.get('api/user').then(({ data }) => (this.users = data.data));
        },

        createUser() {
          this.$Progress.start();
          this.form.post('api/user')
          .then(()=>{
            this.$Progress.finish();

            AddEvent.$emit('AfterCreate');

            $('#addNew').modal('hide');
            
            Toast.fire({
              icon: 'success',
              title: 'User Created Successfully'
            });

          })
          .catch(()=> {
            this.$Progress.fail();
          })

        },

      },

      created() {
        this.loadUsers();
        AddEvent.$on('AfterCreate', () => {
          this.loadUsers();
        });
        
        AddEvent.$on('AfterDelete', () => {
          this.loadUsers();
        });
        
        AddEvent.$on('AfterUpdate', () => {
          this.loadUsers();
        });
        // setInterval( () => this.loadUsers(),10000);
      },

        mounted() {
            console.log('Component mounted.');
            this.$Progress.start();
            this.$Progress.finish();
        }
    }
</script>
