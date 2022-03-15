<template>
  <div class="container">
    <div class="row">
      <div class="col-md-6 col-md-offset-3">
        <h3>Vue-Resource</h3>
        <div class="form-group">
        <input type="text" class="form-control" v-model="userName">
        </div>
                <button class="btn btn-primary" @click="saveUsers"> Add User </button>
                <button class="btn btn-primary" @click="getUsers"> Get User</button>

<hr>
<ul class="list-group">
<li class="list-group-item" v-for="user in userList"> 
<span>{{user.data.userName}}</span>
                <button class="btn btn-xs btn-danger" @click="deleteUser(user.key)"> Delete User</button>

</li>

</ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      userName: null,
      userList:[],
      resource: {}

    }
  },
  methods: {
    saveUsers(){
     // this.$http.post("users.json", {userName : this.userName})
     // .then((response) => {
     // console.log(response);
      // })
            // this.$resource("users.json").save({},{ userName : this.userName});

            this.resource.specialSave({} , {userName : this.userName});

    },
        getUsers(){
      this.$http.get("users.json")
      .then((response) => {
        let data = response.data;
        this.userList = [];

        for(let key in data){
            this.userList.push(
            { key: key,
             data :  data[key]});
        }
      }) ;
        },
        deleteUser(userKey){
          this.$http.delete("users/"+ userKey+".json").
          then(response => {
            console.log(response);
         })


        }
      
  },
  created(){
    const customActions={
      specialSave : { method : "POST" , url : "users.json"}
    };
    this.resource = this.$resource("users.json", {} , customActions);
  }
  
}
</script>

<style>
</style>
