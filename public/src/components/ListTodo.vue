<template>
    <div>

        <div class="col-md-12" v-show="todos.length>0">

            <h3>Todo Items</h3>

            <div class="row mrb-10" v-for="todo in todos" v-bind:key="todo._id">

                <div class="input-group">

                    <div class="input-group-prepend">
                        <span class="input-group-text">
                        <input type="checkbox" v-model="todo.done"
                               :checked="todo.done" :value="todo.done"
                               v-on:change="updateTodo(todo)"
                               title="Mark as done?"/>
                        </span>
                    </div>

                    <input type="text" class="form-control" :class="todo.done?'todo__done':''" v-model="todo.name"
                           @keypress="todo.editing=true" @keyup.enter="updateTodo(todo)">

                    <div class="input-group-append">
                        <span class="input-group-text" title="Delete todo?"
                          v-on:click="deleteTodo(todo._id)">X</span>
                    </div>

                </div>

                <span class="help-block small" v-show="todo.editing">Hit enter to update</span>

            </div>

        </div>


        <div class="row alert alert-info text-center" v-show="todos.length==0">


            <p class="alert alert-info">
                <strong>All Caught Up</strong>
                <br/>
                You do not have any todo items</p>

        </div>


    </div>
</template>

<script>
    export default {

        data() {

            return {

                todos: []

            }

        },

        created: function () {

            this.syncTodo();

        },

        methods: {
            syncTodo() {
                firebase.firestore().collection("todos").onSnapshot((snap) => {
                    let tmp, todos = [];
                    snap.forEach((docSnapshot) => {
                        tmp = docSnapshot.data();
                        tmp._id = docSnapshot.id;
                        todos.push(tmp);
                    });
                    this.todos = todos;
                });
            },

            updateTodo(todo) {

                let id = todo._id;
                todo.editing = false;

                firebase.firestore().collection("todos").doc(id).set(todo)
                .then(() => {
                    console.log("Document successfully written!");
                })
                .catch((error) => {
                    console.error("Error writing document: ", error);
                });

            },


            deleteTodo(id) {

                firebase.firestore().collection("todos").doc(id).delete()
                .then(() => {
                    console.log("Document successfully deleted: " + id);
                }).catch(function(error) {
                    console.error("Error removing document: ", error);
                });

            }

        }

    }
</script>

<style scoped>
    .delete__icon {
    }

    .todo__done {

        text-decoration: line-through !important

    }

    .no_border_left_right {

        border-left: 0px;

        border-right: 0px;

    }

    .flat_form {

        border-radius: 0px;

    }

    .mrb-10 {

        margin-bottom: 10px;

    }

    .addon-left {

        background-color: none !important;

        border-left: 0px !important;

        cursor: pointer !important;

    }

    .addon-right {

        background-color: none !important;

        border-right: 0px !important;

    }
</style>
