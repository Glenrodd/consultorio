<template>
    <div class="container">
        <div class="row mt-5">
          <div class="col-md-12">
            <div class="card">
              <div class="card-header">
                <h3 class="card-title">Tabla de Usuarios</h3>
                <div class="card-tools">
                  <button class="btn btn-success" data-toggle="modal" data-target="#addNew">Nuevo <i class="fas fa-user-plus"></i></button>
                </div>
              </div>
              <!-- /.card-header -->
              <div class="card-body table-responsive p-0">
                <table class="table table-hover">
                  <thead>
                    <tr>
                      <th>ID</th>
                      <th>Nombre</th>
                      <th>Email</th>
                      <th>Tipo</th>
                      <th>Fecha de Registro</th>
                      <th>Modificar</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="user in users" :key="user.id">
                      <td>{{user.id}}</td>
                      <td>{{user.name}}</td>
                      <td>{{user.email}}</td>
                      <td>{{user.type | upText}}</td>
                      <td>{{user.created_at | myDate}}</td>
                      <td>
                          <a href="#">
                            <i class="fa fa-edit blue"></i>
                          </a>/
                          <a href="#">
                              <i class="fa fa-trash red"></i>
                          </a>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
              <!-- /.card-body -->
            </div>
            <!-- /.card -->
          </div>
        </div>
        <!-- Modal -->
        <div class="modal fade" id="addNew" tabindex="-1" role="dialog" aria-labelledby="addNewLabel" aria-hidden="true">
          <div class="modal-dialog modal-dialog-centered" role="document">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title" id="addNewLabel">Registrar Nuervo Usuario</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                  <span aria-hidden="true">&times;</span>
                </button>
              </div>
              <form @submit.prevent="createUser">
                  <div class="modal-body">
                    <div class="form-group">                  
                      <input v-model="form.name" type="text" name="name"
                      placeholder="Nombre Usuario" 
                        class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
                      <has-error :form="form" field="name"></has-error>
                    </div>
                    <div class="form-group">                  
                      <input v-model="form.email" type="email" name="email"
                      placeholder="email@gmail.com" 
                        class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
                      <has-error :form="form" field="email"></has-error>
                    </div>
                    <div class="form-group">                  
                      <textarea v-model="form.bio" name="bio" id="bio"
                      placeholder="Pequeña Biografia del usuario (opcional)" 
                        class="form-control" :class="{ 'is-invalid': form.errors.has('bio') }"></textarea>
                      <has-error :form="form" field="bio"></has-error>
                    </div>
                    <div class="form-group">                  
                      <select v-model="form.type" type="type" name="type" id="type" 
                        class="form-control" :class="{ 'is-invalid': form.errors.has('type') }">
                        <option value="">Seleccione rol de usuario</option>
                        <option value="admin">Administrador</option>
                        <option value="user">Usuario</option>
                        <option value="doctor">Doctor</option>
                      </select>
                      <has-error :form="form" field="bio"></has-error>
                    </div>
                    <div class="form-group">                  
                      <input v-model="form.password" type="password" name="password" id="password" 
                      placeholder="Password" 
                        class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
                      <has-error :form="form" field="password"></has-error>
                    </div>
                  </div>
                  <div class="modal-footer">
                    <button type="button" class="btn btn-danger" data-dismiss="modal">Cerrar</button>
                    <button type="submit" class="btn btn-primary">Registrar</button>
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
                users : {},
                form: new Form({
                    name: '',
                    email: '',
                    password: '',
                    type: '',
                    bio: '',
                    photo: ''
                })
            }
        },
        methods:{
            loadUsers(){
                axios.get("api/user").then(({data})=>(this.users = data.data));
            },
            createUser(){
                this.$Progress.start();
                this.form.post('api/user');
                toast.fire({
                  icon: 'success',
                  title: 'Nuevo usuario registrado correctamente'
                })
                this.$Progress.finish();
            }
        },
        mounted() {
            console.log('Component mounted.')
        },
        created(){
            this.loadUsers();
        }

    }
</script>
