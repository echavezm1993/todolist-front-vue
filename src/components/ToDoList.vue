<template>
    <div class="pt-3">
        <v-row class="d-flex justify-center">
            <v-col cols="6">
                <v-text-field label="New ToDo" outlined v-model="newItem" @keyup.enter="addItem"></v-text-field>
            </v-col>
            <v-col cols="1">
                <v-btn large color="primary" @click="addItem">Add</v-btn>
            </v-col>
        </v-row>
        <transition-group name="fade">
            <v-card class="mx-auto pa-3 ma-2 text-center" max-width="600" v-for="todo in ToDos" :key="todo.id">
                <v-row class="d-flex justify-space-around">
                    <v-checkbox cols="2" color="success" v-model=todo.status @click="updateItem(todo)"></v-checkbox>
                    <v-col class="text-left" cols="8">
                        <v-list-item-content> {{todo.name}}</v-list-item-content>
                    </v-col>
                    <v-col class="text-right" cols="2">
                        <v-btn icon color="red" @click="deleteToDo(todo.id)">
                            <v-icon>mdi-delete</v-icon>
                        </v-btn>
                    </v-col>
                </v-row>
            </v-card>
        </transition-group>
    </div>
</template>

<script>
import axios from "axios";
export default {
    data() {
        return {
            ToDos: [],
            newItem: "",
            url: "http://localhost:8085/api/tasks"
        }
    },
    created() {
        this.getToDoList();
    },
    methods: {
        async getToDoList() {
            axios.get(this.url).then(response => {
                this.ToDos = response.data;
                console.log(this.ToDos);
            }).catch(error => {
                console.log(error);
            });
        },
        async addItem() {
            console.log("addItem functions")
            if (this.newItem != "") {
                console.log("addItem");
                axios.post(this.url, {name: this.newItem, status: false}).then(response => {
                    console.log(response);
                    this.getToDoList();
                })
                this.newItem = "";
            }
        },
        async updateItem(todo) {
            console.log("call updateItem function", todo)
            axios.put(this.url + "/" + todo.id, todo).then(response => {
                console.log(response);
            });
        },
        async deleteToDo(id) {
            console.log("Delete todo ", id);
            axios.delete(this.url + "/" + id).then(response => {
                console.log(response);
                this.getToDoList();
            })
        }
    }
}
</script>