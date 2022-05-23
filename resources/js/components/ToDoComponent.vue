<template>
    <div class="container">
        <div class="row justify-content-center mt-5">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">To Do list</div>

                    <div class="card-body">
                        <div class="input-group">
                            <div class="input-group m-2">
                                <input type="text" v-model="todoName" placeholder="Todo.." class="form-control" aria-label="todo" aria-describedby="todo">
                            </div>
                            <div class="input-group m-2">
                                <textarea type="text" v-model="todoDesc" placeholder="Description.." class="form-control" aria-label="description" aria-describedby="description"></textarea>
                            </div>
                            <div class="input-group-append m-2">
                                <button v-if="!isEdit" type="button" class="btn btn-info" v-on:click="saveToDo()">Add</button>
                                <button v-else type="button" class="btn btn-info" v-on:click="updateToDo()">Update</button>
                                <button type="button" class="btn btn-danger float-right" v-on:click="resetToDo()">Reset</button>
                            </div>
                        </div>

                        <table class="table table-bordered mt-2">
                            <thead>
                                <tr>
                                <th scope="col">#</th>
                                <th scope="col">Name</th>
                                <th scope="col">Description</th>
                                <th scope="col">Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(todo,index) in todos" :key="index" >
                                    <th scope="row">{{++index}}</th>
                                    <td>{{ todo.name }}</td>
                                    <td>{{ todo.description }}</td>
                                    <td>
                                    <button type="button" class="btn btn-danger" v-on:click="deleteToDo(--index)">Delete</button>
                                    <button type="button" class="btn btn-success" v-on:click="editToDo(--index)">Edit</button>
                                    </td>
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
                todos: [],
                api: 'http://127.0.0.1:8000/api/todos',
                todoName: '',
                todoDesc: '',
                isEdit: false,
                editTodoId: '',
                editTodoIndex: '',
            }
        },
        mounted() {
            // getList() {
                axios.get(this.api).then((response) => {
                    this.todos = response.data
                })
            // },
        },
        methods: {
            saveToDo() {
                let data = {
                    'name': this.todoName,
                    'description': this.todoDesc
                }
                axios.post(this.api, data).then((response) => {
                    this.todos.push(response.data);
                    this.todoName = '';
                    this.todoDesc = '';
                })
            },
            deleteToDo(index) {
                axios.delete(this.api + '/'+this.todos[index].id).then((response) => {
                    this.todos.splice(index, 1);
                })
            },
            editToDo(index) {
                this.isEdit = true;
                this.todoName = this.todos[index].name;
                this.todoDesc = this.todos[index].description;
                this.editTodoId = this.todos[index].id;
                this.editTodoIndex = index;
            },
            updateToDo() {
                let data = {
                    'name': this.todoName,
                    'description': this.todoDesc
                }
                axios.put(this.api +'/'+this.editTodoId, data).then((response) => {
                    this.todos[this.editTodoIndex].name = response.data.name;
                    this.todos[this.editTodoIndex].description = response.data.description;
                    this.todoName = '';
                    this.todoDesc = '';
                })
            },
            resetToDo() {
                this.todoName = '';
                this.todoDesc = '';
                this.isEdit = false;
                this.editTodoId = '';
                this.editTodoIndex = '';
            }

        },
    }
</script>
