<template>
    <div class="container">
        <div class="row justify-content-center mt-5">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">User list</div>

                    <div class="card-body">
                        <table class="table table-bordered mt-2">
                            <thead>
                                <tr>
                                <th scope="col">#</th>
                                <th scope="col">Name</th>
                                <th scope="col">Email</th>
                                <th scope="col">Account Visit</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(user,index) in users" :key="index" >
                                    <th scope="row">{{++index}}</th>
                                    <td>{{ user.name }}</td>
                                    <td>{{ user.email }}</td>
                                    <td><button type="button" class="btn btn-primary" v-on:click="viewProfile(--index)">View</button></td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                users: [],
                api: 'http://127.0.0.1:8000/api/users',
            }
        },
        mounted() {
            // getList() {
                axios.get(this.api).then((response) => {
                    this.users = response.data
                })
            // },
        },
        methods: {
            viewProfile(index){
                axios.get(this.api + '/' + this.users[index].id).then((response) => {
                    this.users = response.data
                })
            }
        }
    }
</script>
