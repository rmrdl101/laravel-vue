<script setup>
    import axios from 'axios';
    import { ref, onMounted, reactive } from 'vue';

    const users = ref([]);


    // this get updated once the form is filled
    const form = reactive({
        name: '',
        email: '',
        password: ''
    });
    
    const getUsers = () => {
        axios.get('/api/users')
        .then((response) => {
            users.value = response.data;
        })
    }

    const createUser = () => {
        axios.post('/api/users', form)
        .then((response) => {
            users.value.unshift(response.data); // this pushes the data to the top of the array
            form.name = '';
            form.email = '';
            form.password = '';
            
            $('#pageModal').modal('hide'); // this closses the pageModal
        })
    }

    onMounted(() => { 
        getUsers();
    });
</script>
<template>
    <div class="content-header">
        <div class="container-fluid">
            <div class="row mb-2">
                <div class="col-sm-6">
                    <h1 class="m-0">Users</h1>
                </div>
                <div class="col-sm-6">
                    <ol class="breadcrumb float-sm-right">
                        <li class="breadcrumb-item">
                            <router-link to="/admin/dashboard">Dashboard</router-link>
                        </li>
                        <li class="breadcrumb-item active">Users</li>
                    </ol>
                </div>
            </div>
        </div>
    </div>

    <div class="content">
        <div class="container-fluid">
            <button type="button" class="mb-2 btn btn-primary" data-toggle="modal" data-target="#pageModal">
                Add User
            </button>
            <div class="card">
                <div class="card-body">
                    <table class="table table-bordered">
                        <thead>
                            <tr>
                                <th style="width: 10px;">#</th>
                                <th>Name</th>
                                <th>Email</th>
                                <th>Registered Date</th>
                                <th>Role</th>
                                <th>Options</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(user, index) in users" :key="user.id">
                                <td>{{ index + 1 }}</td>
                                <td>{{ user.name }}</td>
                                <td>{{ user.email }}</td>
                                <td>-</td>
                                <td>-</td>
                                <td>-</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <div class="modal fade" id="pageModal" data-backup="static" tabindex="-1" role="dialog" aria-labelledby="staticBackdropLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-tittle" id="staticBackdropLabel">Add User</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden>&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    <form autocomplete="off">
                        <div class="form-group">
                            <label for="name">Name</label>
                            <input v-model="form.name" type="text" class="form-control" id="name" placeholder="Enter full name">
                        </div>
                        <div class="form-group">
                            <label for="email">Email</label>
                            <input v-model="form.email" type="email" class="form-control" id="email" placeholder="email">
                        </div>
                    </form>
                        <div class="form-group">
                            <label for="password">Password</label>
                            <input v-model="form.password" type="password" class="form-control" id="password" placeholder="password">
                        </div>
                    </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-dismiss="modal">Cancel</button>
                    <button @click="createUser" type="button" class="btn btn-primary">Save</button>
                </div>
            </div>
        </div>
    </div>
</template>
