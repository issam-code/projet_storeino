<template>
<div class="provinces">
    <add_city :msg="msg" type_search="name" name="City" @add_city="add_city($event)"/>
    <cities_list :msg="msg" @update_city="update_city($event)" @delete_city="delete_city($event)"  />
</div>
</template>
<script>

import $ from 'jquery';
import cities_list from "../components/cities_list";
import add_city from "../components/add";
export default {
    name : "cities",
    components :{
        cities_list,
        add_city
    },
    data(){
        return{
            msg : "",
        }
    },
   
    async created () {
        this.$store.commit('refCity');
    },
    methods : {
         add_city :   function(city){
            this.$store.dispatch('add_city',{name : city, code : this.$route.params.code , name_pr : this.$route.params.name_pr} )
            .then(async (reponse) => {
                $('#add')
                    .modal('hide')
                    .find("input")
                    .val('')
                    .end();
                alert(reponse.data);
                this.$store.commit('refCity');
            })
            .catch((errors) => {
                this.msg = errors.response.data;
            });
            this.msg="";
            
            },
            delete_city  :  function(id){
                this.$store.dispatch('delete_city',{id : id, code : this.$route.params.code , name_pr : this.$route.params.name_pr} )
                .then( async (reponse) => {
                    alert(reponse.data);
                    this.$store.commit('refCity');
                })
                .catch((errors) => {
                    alert("try again, city not deleted ! !")
                    console.log(errors)
                });
            },
            update_city :  function(city){
                this.$store.dispatch('update_city',{id : city.id, name : city.name, code : this.$route.params.code, name_pr : this.$route.params.name_pr })
                .then(async (response) => {
                    alert(response.data.name + " city updated !");
                    $('#update_city')
                        .modal('hide')
                        // .find("input")
                        // .val('');
                    this.$store.commit('refCity');
                })
                .catch(async (errors) => {
                    this.msg=errors.response.data;
                    console.log(errors);
                    this.$store.commit('refCity');
                });
                this.msg = "";
            },
  }
}
</script>