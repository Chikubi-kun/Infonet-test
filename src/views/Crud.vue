<template>
    <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#custForm">
        Add Customer
    </button>
    <div class="modal fade" id="custForm" tabindex="-1" role="dialog" aria-labelledby="custFormTitle" aria-hidden="true">
        <div class="modal-dialog modal-dialog-scrollable" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="custFormTitle">Add Customer</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div>
                    <form>
                        <div class="modal-body">
                             <div class="form-group">
                                <input
                                    type="text"
                                    class="form-control"
                                    placeholder="Nama"
                                    v-show="!updateSubmit"
                                    v-model="form.nama">
                                <input
                                    type="text"
                                    class="form-control"
                                    placeholder="Nama"
                                    v-show="updateSubmit"
                                    v-model="form.nama"
                                    readonly>
                            </div>
                             <div class="form-group">
                                <input
                                    type="text"
                                    class="form-control"
                                    placeholder="Jabatan"
                                    v-model="form.jabatan">
                            </div>
                             <div class="form-group">
                                <select class="form-control" v-model="form.gender">
                                    <option value="" disabled selected>Gender</option>
                                    <option value="M">Male</option>
                                    <option value="F">Female</option>
                                </select>
                            </div>
                        </div>
                        <div class="modal-footer">
                            <button
                                type="button"
                                class="btn btn-secondary"
                                @click="clear()"
                                data-dismiss="modal">Close</button>
                            <button
                                type="button"
                                class="btn btn-primary"
                                v-show="updateSubmit"
                                @click="update(form)"
                                data-dismiss="modal">Update</button>
                            <button
                                type="button"
                                class="btn btn-primary"
                                v-show="!updateSubmit"
                                @click="add()"
                                data-dismiss="modal">Add</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>

    <table class="table">
        <thead>
            <tr>
                <th scope="col">Nama</th>
                <th scope="col">Jabatan</th>
                <th scope="col">Gender</th>
                <th scope="col">Actions</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="cust in customers" :key="cust.name">
                <td>{{ cust.nama }}</td>
                <td>{{ cust.jabatan }}</td>
                <td>{{ cust.gender }}</td>
                <td>
                    <button type="button" class="btn btn-warning" data-toggle="modal" data-target="#custForm" @click="edit(cust)"> Edit</button> | 
                    <button type="button" class="btn btn-danger" @click="del(cust)"> Delete</button>
                </td>
            </tr>
        </tbody>
    </table>
</template>

<script>
/* eslint-disable */
import axios from 'axios';

export default {
    data() {
        return {
            customers: [],
            form: {
                nama: '',
                jabatan: '',
                gender: ''
            },
            updateSubmit: false,
        }
    },
    mounted() {
        this.load()
    },
    methods: {
        load() {
            //require run: json-server --id nama .\mock-data\custuomer.json
            //karena item pada json tidak memiliki id maka nama digunakan sebagai key
            axios.get('http://localhost:3000/customers')
                .then(response => {
                    this.customers = response.data
                })
                .catch(err => {
                    console.log(err)
                })
        },
        clear() {
            this.form.nama = ''
            this.form.jabatan = ''
            this.form.gender = ''
            this.updateSubmit = false

        },
        add() {
            axios.post('http://localhost:3000/customers', this.form)
                .then(response => {
                        this.load()
                        this.clear()
                })
        },
        edit(cust) {
            this.updateSubmit = true
            this.form.nama = cust.nama
            this.form.jabatan = cust.jabatan
            this.form.gender = cust.gender
        },
        update(form) {
            return axios.put('http://localhost:3000/customers/' + this.form.nama, {
                nama: this.form.nama,
                jabatan: this.form.jabatan,
                gender: this.form.gender
            })
                .then(response => {
                    this.load()
                    this.clear()
                })
                .catch(err => {
                    console.log(err)
                })
        },
        del(cust) {
            axios.delete('http://localhost:3000/customers/' + cust.nama)
                .then(response => {
                    this.load()
                })
        }
    }
}
</script>