<template>
  <div class="hello">
     <p class="text-sm-center grey--text">Search User GitHub</p>
    <div>
        <v-container grid-list-md>
            <v-layout row wrap>
              <v-flex xs5 offset-sm4>
                <form @submit.prevent="search">
                    <v-text-field v-model="username" solo label="Insira o nome do usuário" ></v-text-field>
                </form>    
              </v-flex>
            </v-layout>
        </v-container>
    </div>
    <br>
    <div>
        <p v-if="data.name && data.location && data.avatar_url ">
            <v-layout >
                <v-flex xs3 offset-sm5>
                    <v-card elevation-10>
                        <v-card-media v-bind:src="data.avatar_url" height="200px"></v-card-media>
                        <v-card-title primary-title>                         
                            <div>
                                <h3 class="headline mb-0" >{{data.name}}</h3>
                                <p class="text-sm-left grey--text">{{data.login}}</p>
                                <p class="text-sm-left grey--text">{{data.bio}}</p>
                            </div>
                        </v-card-title>
                                             
                        <v-card-actions>
                            <v-btn flat color="orange" @click.native="show = !show">Repository</v-btn>
                            <v-btn flat color="purple">Explore</v-btn> <v-spacer></v-spacer>
                        </v-card-actions>
                        <v-slide-y-transition>
                          <v-card-text v-show="show">
                            <v-list>
                                <v-list-group v-for="item in repository" :key="item.name" onclick="">
                                <v-list-tile slot="activator">
                                  <v-list-tile-content>
                                    <v-list-tile-title>
                                        <v-icon dark>folder</v-icon>{{ item.name }}
                                    </v-list-tile-title>
                                  </v-list-tile-content>
                                </v-list-tile>
                                <v-list-tile>
                                  <v-list-tile-content>
                                    <v-list-tile-title>
                                        <a :href="item.html_url" v-bind:title="item.description ">{{ item.html_url }}</a>
                                    </v-list-tile-title>
                                  </v-list-tile-content>
                                </v-list-tile>
                              </v-list-group>
                            </v-list>
                          </v-card-text>
                        </v-slide-y-transition>
                  </v-card>
                </v-flex>
          </v-layout>
        </p>  
    </div>
    <p v-if="errorMsg.length"> {{errorMsg}}</p>
  </div>
</template>
<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
Vue.use (VueAxios, axios)

export default {
  name: 'GitHubApi',
    data () {
        return {
            username: [],
            data: [],
            errorMsg: [],
            show: false,
            repository:[],
            errorRepository:[],
            items: [
                  {
                    action: 'folder',
                    title: 'Attractions',
                    items: [
                      { title: 'List Item' }
                    ]
                  },
                  {
                    action: 'folder',
                    title: 'Dining',
                    active: true,
                    items: [
                      { title: 'Breakfast & brunch' },
                      { title: 'New American' },
                      { title: 'Sushi' }
                    ]
                  },
                  {
                    action: 'school',
                    title: 'Education',
                    items: [
                      { title: 'List Item' }
                    ]
                  },
                  {
                    action: 'directions_run',
                    title: 'Family',
                    items: [
                      { title: 'List Item' }
                    ]
                  },
                  {
                    action: 'healing',
                    title: 'Health',
                    items: [
                      { title: 'List Item' }
                    ]
                  },
                  {
                    action: 'content_cut',
                    title: 'Office',
                    items: [
                      { title: 'List Item' }
                    ]
                  },
                  {
                    action: 'local_offer',
                    title: 'Promotions',
                    items: [
                      { title: 'List Item' }
                    ]
                  }
            ],
        }
    },

    methods: {
        search (){
            const api = `https://api.github.com/users/${this.username}`
            Vue.axios.get(api).then(response => {
                this.data = response.data;
                this.searchRepository(this.data.repos_url);
            }).catch(error =>{
                this.errorMsg = 'Nenhum usuário encontrado'
                this.data = []
            })
        },
        searchRepository (url){
            const apiRepository = url
            Vue.axios.get(apiRepository).then(response => {
                this.repository = response.data
                // console.log(repository);
            }).catch(error =>{
                this.errorRepository = 'Nenhum Repositório encontrado'
                this.repository = []
            })
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
