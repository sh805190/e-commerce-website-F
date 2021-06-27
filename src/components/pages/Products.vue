<template lang="">
    <div>
        <div class="text-start mt-4">
            <button class="btn btn-primary" @click="openModal(true)">Create New
                Product</button>
            <a href="#" @click.prevent="signout">Log out</a>

            <!-- data-bs-toggle="modal" data-bs-target="#productModal" -->
        </div>
        <table class="table mt-4">
            <thead>
                <tr>
                    <th>Category</th>
                    <th>Products' Name</th>
                    <th>Original Price</th>
                    <th>Sale Price</th>
                    <th>Enabled</th>
                    <th>Modification</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item) in products" :key="item.id">
                    <td>{{item.category}}</td>
                    <td>{{item.title}}</td>
                    <td class="text-start">{{item.origin_price}}</td>

                    <td class="text-start">{{item.price}}</td>

                    <td>
                        <span v-if="item.is_enabled" class="text-success">True</span>
                        <span v-else>False</span>
                    </td>

                    <td><button class="btn btn-outline-primary btn-sm" @click="openModal(false,item)">Modify</button>
                    </td>

                </tr>
            </tbody>

        </table>
        <div class="modal fade" id="productModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel"
            aria-hidden="true">
            <div class="modal-dialog modal-lg" role="document">
                <div class="modal-content border-0">
                    <div class="modal-header bg-dark text-white">
                        <h5 class="modal-title" id="exampleModalLabel">
                            <span>New Product</span>
                        </h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        <div class="row">
                            <div class="col-sm-4">
                                <div class="form-group">
                                    <label for="image">Image URL:</label>
                                    <input type="text" class="form-control" id="image" placeholder="Input URL address"
                                        v-model="tempProduct.imageUrl">
                                </div>
                                <div class="form-group">
                                    <label for="customFile">Or Upload Image:
                                        <i class="fas fa-spinner fa-spin"></i>
                                    </label>
                                    <input type="file" id="customFile" class="form-control" ref="files"
                                        @change="upload">
                                </div>
                                <img img="https://images.unsplash.com/photo-1483985988355-763728e1935b?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=828346ed697837ce808cae68d3ddc3cf&auto=format&fit=crop&w=1350&q=80"
                                    class="img-fluid" :src="tempProduct.imageUrl" alt="">
                            </div>
                            <div class="col-sm-8">
                                <div class="form-group">
                                    <label for="title">Title</label>
                                    <input type="text" class="form-control" id="title" v-model="tempProduct.title"
                                        placeholder="Input Product Title">
                                </div>

                                <div class="form-row">
                                    <div class="form-group col-md-6">
                                        <label for="category">Category</label>
                                        <input type="text" class="form-control" id="category"
                                            v-model="tempProduct.category" placeholder="Input Category">
                                    </div>
                                    <div class="form-group col-md-6">
                                        <label for="price">Unit</label>
                                        <input type="unit" class="form-control" id="unit" v-model="tempProduct.unit"
                                            placeholder="Input Unit">
                                    </div>
                                </div>

                                <div class="form-row">
                                    <div class="form-group col-md-6">
                                        <label for="origin_price">Original Price</label>
                                        <input type="number" class="form-control" id="origin_price"
                                            v-model="tempProduct.origin_price" placeholder="Input Original Price">
                                    </div>
                                    <div class="form-group col-md-6">
                                        <label for="price">Sale Price</label>
                                        <input type="number" class="form-control" id="price" v-model="tempProduct.price"
                                            placeholder="Input Sale Price">
                                    </div>
                                </div>
                                <hr>

                                <div class="form-group">
                                    <label for="description">Product Description</label>
                                    <textarea type="text" class="form-control" id="description"
                                        v-model="tempProduct.description"
                                        placeholder="Input Product Description"></textarea>
                                </div>
                                <div class="form-group">
                                    <label for="content">Products Details</label>
                                    <textarea type="text" class="form-control" id="content"
                                        v-model="tempProduct.content" placeholder="Input Products Details"></textarea>
                                </div>
                                <div class="form-group">
                                    <div class="form-check">
                                        <input class="form-check-input" type="checkbox" v-model="tempProduct.is_enabled"
                                            :true-value="1" :flase-value="0" id="is_enabled">
                                        <label class="form-check-label" for="is_enabled">
                                            Enable
                                        </label>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-outline-secondary" data-dismiss="modal">Cancel</button>
                        <button type="button" class="btn btn-primary" @click="updateProduct">Confirm</button>
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>
<script>
    import { Modal } from 'bootstrap';


    export default {
        data() {
            return {
                products: [],
                tempProduct: {},
                isNew: false,
            }
        },
        methods: {
            getProducts() {
                // get products can be seen by users
                const api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/products`;
                const vm = this;
                this.$http.get(api).then((response) => {
                    console.log(response.data);
                    vm.products = response.data.products;
                });
            },
            openModal(isNew, item) {
                // this.modal = new Modal(document.getElementById('productModal')) /*不需要bootstrap*/


                if (isNew) {
                    this.tempProduct = {}
                    this.isNew = true;

                }
                else {
                    this.tempProduct = Object.assign({}, item);
                    this.isNew = false
                }
                this.modal.show()

            },
            updateProduct() {
                let api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/admin/product`;
                let httpMethod = 'post'
                const vm = this;
                if (!vm.isNew) {
                    api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/admin/product/${vm.tempProduct.id}`
                    httpMethod = 'put'
                }
                this.$http[httpMethod](api, { data: vm.tempProduct }).then((response) => {
                    console.log(response.data);
                    // vm.products = response.data.products;
                    if (response.data.success) {
                        this.modal.hide()
                        vm.getProducts;
                        window.location.reload();

                    } else {
                        this.modal.hide()
                        vm.getProducts;
                        console.log("Cannot Update Product");
                        window.location.reload();

                    }
                });
            },
            signout() {
                const api = `${process.env.VUE_APP_APIPATH}/logout`;
                const vm = this;
                this.$http.post(api).then((response) => {
                    console.log(response.data);
                    if (response.data.success) {
                        vm.$router.push("/login");
                    }
                });

            },
            upload() {
                console.log(this);
                const uploadedFile = this.$refs.files.files[0];
                var formData = new FormData();
                formData.append('file-to-upload', uploadedFile)
                const vm = this;
                const url = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/admin/upload`;

                this.$http.post(url, formData, {
                    header: {
                        'Content-Type': 'multipart/form-data'
                    }
                }).then((response) => {
                    console.log(response.data);
                    if (response.data.success) {
                        // vm.tempProduct.imageUrl = response.data.imageUrl;
                        // console.log(vm.tempProduct)
                        vm.$set(vm.tempProduct, 'imageUrl', response.data.imageUrl)

                    }
                })
            }
        },
        mounted() {
            this.modal = new Modal(document.getElementById('productModal'))
        },
        created() {
            this.getProducts();
            const token = document.cookie.replace(/(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/, '$1');
            console.log(token);
            this.$http.defaults.headers.common.Authorization = token;
            const api = `${process.env.VUE_APP_APIPATH}/api/user/check`;
            this.$http.post(api, this.user).then((res) => {
                if (!res.data.success) {
                    this.$router.push('login');
                }
            });


        },

    }
</script>
<style lang="">

</style>
