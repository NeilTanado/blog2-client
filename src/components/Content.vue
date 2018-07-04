<template>
  <div>
    <div class="valign-wrapper flex-container give-margin">
      <a class="waves-effect waves-light btn" @click="createArt()">Create New Article</a>
    </div>
    <div class="row" v-for="article in articles">
      <div class="col s12 m12">
        <div class="card blue-grey darken-1">
          <div class="card-content white-text">
            <div class="row">
              <div class="col">
                <span class="card-title" style="cursor: pointer;">
                    <router-link :to="{ path: '/read', query:{id:article._id,title:article.title,text:article.text,authorId:article.author._id}}">
                      <b>{{ article.title }}</b>
                    </router-link>
                </span>
              </div>
              <div class="col right">
                <div class="row">
                  <div class="col">
                    <a style="cursor:pointer;" title="Edit this article" @click="editData(article.author._id,article._id)">
                      <router-link :to="{ path: '/articleEdit', query:{id:article._id,title:article.title,text:article.text}}">
                      <i class="material-icons small " style="font-size:35px;">edit</i>
                    </router-link>
                    </a>
                  </div>
                  <div class="col">
                    <a style="cursor:pointer;" title="Delete this article" @click="deleteArticle(article.author._id,article._id)">
                      <i class="material-icons small" style="font-size:35px; color:#ef9a9a;">delete</i>
                    </a>
                  </div>
                </div>
              </div>
            </div>
            <pre style="color:#eeeeee; margin: -4% 0 1% -1%;"> author: {{ article.author.name }} </pre>
            <br>
            <article class="">
              <p class="cuttext">{{ article.text }}</p>
            </article>

          </div>
          <div class="card-action">
            <div class="" style="margin: 0 0 0 87%;" @click="readData(article._id)">
              <router-link :to="{ path: '/read', query:{id:article._id,title:article.title,text:article.text,authorId:article.author._id}}">
              <a class="btn">Read More</a>
            </router-link>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Vuex from "vuex";

export default {
  name:'context',
  data(){
    return{
      articles:[],
    }
  },
  methods:{
    createArt:function(){
      if(!localStorage.getItem('token')){
        alert('please login for create an article')
      }else{
        this.$router.push('/article')
      }
    },
    callData:function(){
      let self = this
      axios.get('https://blogs2.neil.guru/articles/dataarticle')
      .then((value) => {
        value.data.data.forEach(articlesData=>{
          console.log(articlesData);
          self.articles.unshift(articlesData)
        })
      })
      .catch((err) => {
        console.log(err);
      })
    },
    editData:function(articleUserId,articleId){
      if(localStorage.getItem('userId')){
        var userId = localStorage.getItem('userId')
        console.log(articleUserId,userId);
        if(userId === articleUserId){

        }else{
          alert('kamu tidak bisa mengedit article milik orang lain')
          this.$router.push('/')
        }
      }
      else{
        alert('please login first to edit')
        this.$router.push('/')
      }
    },
    deleteArticle:function(articleUserId,articleId){
      console.log(articleUserId);
      if(localStorage.getItem('userId')){
        var userId = localStorage.getItem('userId')
        console.log(articleUserId);
        if(userId === articleUserId){
          axios.delete('https://blogs2.neil.guru/articles/deletearticle/'+articleId)
          .then((value) => {
            for(var i = 0 ; i < this.articles.length ; i++){
              if(this.articles[i]._id === articleId){
                this.articles.splice(i,1)
              }
            }
          })
          .catch((err) => {
            console.log(err);
          })
        }else{
          alert('kamu tidak bisa menghapus article milik orang lain')
          this.$router.push('/')
        }
      }
    }
  },
  mounted(){
    this.callData()
  }
}
</script>

<style scoped>
  .flex-container{
    justify-content : center;
    display: flex;
  }
  .give-margin {
    margin-top: 1.5em;
    margin-bottom: 1.5em;
  }

  .card-content{
    border:1px solid black;
  }
  .floating{
    float:right;
  }
  .cuttext {
    white-space: nowrap;
    width: 1000px;
    overflow: hidden;
    text-overflow: "----";
  }
</style>
