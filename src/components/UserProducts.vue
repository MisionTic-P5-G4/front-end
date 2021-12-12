<template>

    <div class="container-grid">
        <h2>Mis Productos</h2>
       
        <div class="container" >
            <table class="table table-hover" >
            <thead>
                <tr>
                    <th scope="col">Id Producto</th>
                    <th scope="col">Cantidad</th>
                    <!-- <th scope="col">imgSource</th> -->
                    <th scope="col">Acciones</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="userProduct in userProducts" v-bind:key="userProduct.id">
                    <td>{{userProduct.productId}}</td>
                    <td>{{userProduct.quantity}}</td>
                    <!-- <td>{{product.imgSrc.substring(0,3)}}</td> -->
                    <td>
                        <button v-on:click="deleteUserProduct(userProduct.productId)" type="bustton" class="btn btn-danger" data-bs-toggle="modal" data-bs-target="#Delete" data-bs-whatever="@mdo">Borrar</button>
                    </td>
                </tr>
            </tbody>
            </table>
        </div>
    </div>

<!-- <div class ="cuerpo">
    <br>
    <div class ="catalogue">
        <ul class="card-wrapper">
            
            <li class="card" v-for="userProduct in userProducts" v-bind:key="userProduct.id">
                <img :src="product.imgSrc" alt=''>
                <div class="row">
                    <div class="col col align-self-center">
                        <h3>{{product.name}}</h3>
                        <p>${{product.price}}</p>
                    </div>
                    <div class="col col-lg-4 align-self-end">
                        <button v-on:click="addToCart(product)" class="btn btn-primary">Añadir</button>
                    </div>
                </div>
            </li>
            
        </ul>
    </div> 
</div> -->



</template>


<script>   

import gql from "graphql-tag";
export default {
    name: "UserProducts",

    data: function(){
        return {
            userProducts: [],
            userProduct: {
                id: "",
                userId: "",
                productId: "",
                quantity: "",
                dateModified:""
            },
            idUserProductDelete: "",
            userLogged: ""
        }
    },

    methods: {

     getUserData: async function(){
         await this.$apollo
         .query({
         query: gql`
             query UserDetailById {
                 userDetailById {
                     id
                     username
                     name
                     email
                     phone
                     admin
                 }
             }
             `,
                
         })
         .then((result) => {
            this.userLogged = result.data.userDetailById.id
         })
         .catch((error) => {
             console.log(error)
           alert("ERROR: Fallo geUserData");
         });
     },

        getUserProducts: async function(){
            await this.$apollo
            .query({
            query: gql`
                query GetUserProductsByUserId($getUserProductsByUserIdId: Int!) {
                    getUserProductsByUserId(id: $getUserProductsByUserIdId) {
                        id
                        userId
                        productId
                        quantity
                        dateModified
                    }
                }
            `,
            variables: {
                getUserProductsByUserIdId: this.userLogged
            },
            })
            .then((result) => {
                this.userProducts = result.data.getUserProductsByUserId
            })
            .catch((error) => {
                console.log(error)
            alert("ERROR: Fallo getUserData");
            });
        },

        deleteUserProduct: async function(id){
            await this.$apollo
            .mutate({
            mutation: gql`
            mutation DeleteUserProduct($productId: Int!) {
                deleteUserProduct(productId: $productId)
            }
            `,
            variables: {
                productId: id
            }
            })
            .then((result) => {
                alert("result.data.deleteUserProduct");
                this.getUserProducts();
            })
            .catch((error) => {
                console.log(error)
            alert("ERROR: Fallo elimnando producto de usuario");
            });
            //this.getUserProducts();
        },

    },
    created: async function(){
        await this.getUserData();
        await this.getUserProducts();
    }
}


</script>


<style>


</style>