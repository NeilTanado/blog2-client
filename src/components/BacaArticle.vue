<template>
  <div>
    <center>
      <h3> Read your article here </h3>
      <div class="card-panel teal">
        <h3 class="white-text">
          {{title}}
        </h3>
        <h5>
          {{text}}
        </h5>
        <a class="waves-effect waves-light btn btn-cancel" @click="cancel()">cancel</a>
      </div>
    </center>
  </div>
</template>

<script>
import axios from 'axios'
import swal from 'sweetalert';
import Vuex from "vuex";

export default {
  name:'BacaArticle',
  data(){
    return{
      title: '',
      text: '',
      id: '',
      author: '',
    }
  },
  methods:{
    editArticle: function(){
      let articleID = this.id
      console.log(articleID);
      let theArticle = {
        title: this.title,
        text: this.textarea
      }
      axios.put('https://blogs2.neil.guru/articles/updatearticle/'+articleID,theArticle)
      .then((value) => {
        swal({
          title: "Grats!",
          text: "You edit an article!",
          icon: "success",
        });
        this.$router.push('/')
      })
      .catch((err) => {
        console.log(err);
      })
    },
    cancel(){
      this.$router.push('/')
    }
  },
  created(){
    this.title = this.$route.query.title,
    this.text = this.$route.query.text,
    this.id = this.$route.query.id
    this.author = this.$route.query.authorId
  }
}
</script>

<style>
  .margintopsbots{
    margin: 20px 20px 15px 20px;
  }
  .btn-cancel{
    background-color:red;
  }
  .btn-cancel:hover {
    background-color: #ef9a9a;
  }

</style>
