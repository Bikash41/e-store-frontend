<template>
     <div class="store-page">
      <div class="store-page-child">
          <h1 class="small-screen">Stores</h1>
          <hr/>
          <div class="store-class">
              <div class="store-details" v-for="(store,index) in stores" :key="index" @click="seeProduct(store.products,store._id,store.imageUrl)">
                  <!--  -->
                 <div class="store-img">
                     <img :src="store.imageUrl">
                 </div>
               <div class="store-information">
                    <div class="store-name">
                        <h4>{{store.name}}</h4>
                    </div>
                 </div>
              </div>
          </div>
      </div>
   </div>
</template>

<script>
import {store,getSingleStore} from '@/services/store'
export default{
    name: 'AppStore',
    data(){
        return{
            stores: []
        }
    },
    methods:{
        seeProduct(products,storeId, storeImage){
            getSingleStore(storeId).then(data=>{
                this.$store
        .commit('setAddress', data.store.address)
        this.$store
        .commit('setStoreEmail', data.store.storeEmail)
            this.$store
        .commit('setStoreId', storeId)
         this.$store
        .commit('setStoreImage', storeImage)
         this.$router.push({
                        name: 'AppProduct',
                        params: {
                            storeId: storeId
                        }
                    });
        })
        }
    },
    computed:{
      isAuthenticated(){
          return this.$store.getters.isAuthenticated;
      },
      role(){
          return this.$store.state.auth.role
      }
  },
    created(){
        
        store().then(data=>{
            this.stores = data.store;
            console.log("is auth", this.$store.getters.isAuthenticated);
        })
        .catch(error=>error)

    }
}
</script>

<style>
.store-page{
    width:90%;
    margin:50px auto;
}

.store-page-child{
    display: flex;
    /* justify-content: center; */
    flex-direction: column;
    /* align-items: center; */
}
.store-class{
  margin-top:10px;
  display:inline-flex;
  flex-wrap: wrap;
  justify-content: center;
  width: 100%;
  /* align-items: center; */
}
.store-details{
    /* flex-basis:10%; */
    /* display: inline-flex; */
    display: inline-flex;
    flex-direction: column;
    width: 100%;
    border-radius:6px;
    /* box-shadow: 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22); */
    border:none;
    outline:0;
    padding:0;
    cursor: pointer;
    border-radius: 10px;
    /* margin:0 30px 30px 0; */
    margin: 30px 30px;
    width:350px;
    height:350px;
    /* margin-bottom:50px;  */
}
.store-details:hover{
    box-shadow: 0 16px 30px rgba(0, 0, 0, 0.30), 0 12px 12px rgba(0, 0, 0, 0.27);
}
.store-img{
    border-radius: 10px 10px 0 0;
    width: 100%;
    height: 100%;
}
.store-img img{
    width:100%;
    height:100%;
    padding: 0;
    border-radius: 10px 10px 0 0;
}
.store-information{
    width:100%;
    border:1px solid grey;
    height:auto;
    display: flex;
    justify-content: space-between;
    border-radius: 0px 0px 10px 10px;
}
.store-name{
    margin:5px 0px;
    font-size: small;
}
@media screen and (max-width:1400px)
{
    .store-class{
        align-items: center;
        justify-content: center;
        flex-wrap: wrap;
    }
}
@media screen and (max-width:500px)
{
    .store-details{
        width:280px;
        height:300px;
    }
    .small-screen{
        font-size:25px;
    }
    .store-information{
        width:280px;
        flex-direction: column;
        text-align: center;
        align-items: center;
    }
    .store-name{
        margin-bottom:0px;
    }
}
</style>