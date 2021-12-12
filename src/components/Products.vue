<template>
    <div class ="cuerpo">
        <br>
        <div class ="catalogue">
            <ul class="card-wrapper">
                
                    <li class="card" v-for="product in products" v-bind:key="product.id">
                    <img :src="product.imgSrc" alt=''>
                    <h3><a href="/producto">{{product.name}}</a></h3>
                    <p>${{product.price}}</p>
                </li>
                
                
            </ul>
        </div> 
    </div>    
</template> 

<script>
import gql from "graphql-tag";

    export default {
    name: 'Products',
    

    data: function(){
        return {
            product: [],
            products: [],
            createProduct: {
                name: "",
                price: 0,
                service: false,
                imgSrc: ""
            },
            modifyProduct: {
                name: "",
                price: "",
                service: false,
                imgSrc: ""
            },
            idProductDelete: "",
            idProductModi: ""
        }
    },

    methods: {
       getProductsList: async function(){
            await this.$apollo
            .query({
            query: gql`
                query Products {
                    products {
                        id
                        name
                        price
                        service
                        imgSrc
                    }
                }
                `,
                    
            })
            .then((result) => {
                this.products = result.data.products;
            })
            .catch((error) => {
            alert("ERROR: Fallo geUserData");
            });
        },

    },

    created: async function(){
        this.getProductsList();
    },
  }
</script>
<style>

    .list-group{
        background-color: rgba(255, 255, 255, 0.534);
        border-radius: 0.5rem;
        box-shadow: 0.05rem 0.1rem 0.3rem -0.03rem rgba(0, 0, 0, 0.45);
        padding-bottom: 1rem;
        height: 100%;


    }
    .list-group ul{
        display: flex;
        flex-direction: column;
        width: 130px;
        margin: 0;
        padding: 0;
        height: 100%;
    }
    .list-group ul button{
        width: 90%;
        margin: 0 auto;
        margin-top: 10px;
        border-radius: 5px;
        height: 30px;
    }
    .list-group ul button:hover{
        background-color: rgb(187, 187, 187);
        cursor: pointer;
    }

    .list-group h2 span{
        font-size: 1.5rem;
    }

    body{
        margin: 0 0 0 0;
    }

    .card {
    /*
    --card-gradient: rgba(0, 0, 0, 0.8);
    --card-gradient: #949494b7, #00000088;
    --card-blend-mode: overlay;
     */
    
    background-color: rgba(255, 255, 255, 0.534);
    border-radius: 0.5rem;
    box-shadow: 0.05rem 0.1rem 0.3rem -0.03rem rgba(0, 0, 0, 0.45);
    padding-bottom: 1rem;
    background-image: linear-gradient(
        var(--card-gradient),
        white max(9.5rem, 27vh)
    );
    overflow: hidden;
    }

    img {
    border-radius: 0.5rem 0.5rem 0 0;
    width: 100%;
    object-fit: cover;
    
    max-height: max(20rem, 30vh);
    aspect-ratio: 4/3;
    mix-blend-mode: var(--card-blend-mode);
    
    }
    :last-child {
    margin-bottom: 0;
    }
    :hover,
    :focus-within {
    --card-gradient: #00000075 max(8.5rem, 20vh);
    }
    
    /* Additional demo display styles */
    * {
    box-sizing: border-box;
    }

    .body {
    display: grid;
    place-content: center;
    justify-items: center;
    min-height: 100vh;
    margin: 0;
    padding: 1rem;
    line-height: 1.5;
    font-family: -apple-system, BlinkMacSystemFont, avenir next, avenir,
        helvetica neue, helvetica, Ubuntu, roboto, noto, segoe ui, arial, sans-serif;
    color: #444;
    background-color: #ffffff;
    }
    .card h3 {
    margin-top: 1rem;
    font-size: 1.25rem;
    }
    .card a {
    color: inherit;
    }
    .card-wrapper {
    list-style: none;
    padding: 0;
    margin: 0;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(30ch, 1fr));
    grid-gap: 1.5rem;
    max-width: 100vw;
    width: 120ch;
    padding-left: 1rem;
    padding-right: 1rem;
    }
</style>