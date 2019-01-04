<template>
<div class="container mt-5">
  <div class="search-bar d-flex justify-content-center">
    <div class="px-2"><i class="fa fa-search"></i></div>
    <div class="px-2"><input class="search-input" v-model="fullRepoName" /></div>
    <div><button class="search-button" v-on:click="findRepo()">Find</button></div>
  </div>
  <div class="breadcrumb-container" v-if="ifShow">
    <span v-for="path in pathArray">/<a href="javascript:void(0)" v-on:click="navigatePath($event)">{{path}}</a></span>
  </div>
  <div class="table-container" v-if="ifShow">
    <table class="table table-bordered my-4">
      <thead class="thead-light">
        <tr class="text-center">
          <th scope="col">#</th>
          <th scope="col">File Name</th>
          <th scope="col">Download file</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="file,index in files">
          <!-- <tr> -->
          <th class="index" scope="row">{{index}}</th>
           <!-- <th scope="row">1</th> -->
          <td class="px-5"><i :class="file.type === 'dir'?'px-2 fa fa-folder':'px-2 fa fa-file'"></i><a href="javascript:void(0)" :class="file.type === 'dir'?'folder-link':'file-link'" v-on:click="getFiles($event)">{{file.name}}</a></td>
          <!-- <td><a v-on:click="findRepo($event)">asddsd</a></td> -->
          <td><a :href="file.download_url" download class="text-center" v-if="file.type==='file'"><i class="fa fa-download px-2"></i>Download</a></td>
        </tr>
      </tbody>
    </table>
  </div>
</div>
</template>

<script>
import axios from 'axios';

export default {
  name: "GithubExplorer",
  data(){
    return {
  userName: "",
  repoName: "",
  fullRepoName: "",
  files:[],
  url: "",
  path: "contents",
  ifShow: false,
  pathArray: ["contents"]
    }
  },
  methods: {
    findRepo(){
      this.ifShow = true;
      let repo = this.fullRepoName.split('/');
      this.userName = repo[0];
      this.repoName = repo[1];
      axios.get(`https://api.github.com/repos/${this.userName}/${this.repoName}/${this.path}`)
      .then(response => {
        this.files = response.data;
        this.files.sort((a,b) => {
          if (a.type === 'dir' && b.type !== a.type) return -1
          else if (a.type === 'file' && b.type !== a.type) return 1;
          else return 0;
      });
      console.log(this.files);
      })
      .catch(error => {
        this.ifShow = false;
        console.log(error);
      })
    },
   getFiles(e){
      if(e){
      console.log(e.target.innerHTML);
      this.path = this.path + '/' + e.target.innerText;
      this.findRepo();
        }
   },
   navigatePath(e){
     if(e){
       console.log(e.target.innerHTML);
       let repoStr = '';
       for(let path in this.pathArray){
       repoStr = repoStr + this.pathArray[path];
         if(e.target.innerHTML === this.pathArray[path]){
           break;
         }
       }
       this.path = repoStr;
       this.findRepo();
     }
   }
  },
  watch: {
   path(){
    this.pathArray = this.path.split('/'); 
   }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
.container {
  width: 100%;
}
.search-input {
  width: 400px;
  border-radius: 5px;
}
.search-button {
  background: #222831;
  color: #eeeeee;
  width: 75px;
}
.index {
  text-align: center;
}
.folder-link {
  color: darkblue;
  font-weight: bold;
}
.file-link {
  color: black;
}
a {
  color: #007bff;
}
</style>
