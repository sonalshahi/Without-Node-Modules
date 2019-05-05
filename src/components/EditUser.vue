<template>
  <div id="EditUser" >
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="email" disabled placeholder="Email">
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="name" placeholder="Name">
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input type="text"  v-model="hobby"  placeholder="Hobby">
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="age"  placeholder="Age">
        </div>
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="contact"  placeholder="Contact Number">
        </div>
      </div>  
      </div>

  <button type="button" v-on:click="saveEmployee">
  Save
  </button>
  </div>
</template>

<script>
import router from '../router';
import firebase from 'firebase';

    import db from './firebaseInit';
    export default {
      data () {
        return {
            email: '',
            name: '',
            age: '',
            hobby: '',
            contact: '',
            updateid: ''
        }
      },
      created(){
      var w = firebase.auth().currentUser.email;

        db
      .collection('user1')
      .orderBy('Name')
      .get()
      .then(querySnapshot => {
        this.loading = false;
        querySnapshot.forEach(doc => {
          const data = {
            Name : doc.data().Name,
            Email : doc.data().Email,
            Hobby : doc.data().Hobby,
            contact : doc.data().contact,
            Age : doc.data().Age
          };
          if(w == data.Email){
              this.name = data.Name;
              this.hobby = data.Hobby;
              this.contact = data.contact;
              this.age = data.Age;
              this.email = data.Email;
              this.updateid = doc.id;
          }
        });
      });
    },
        methods: {
        saveEmployee () {
            db.collection("user1").doc(this.updateid).update({
              "Name" : this.name,
              "Age" : this.age,
              "contact" : this.contact,
              "Hobby" : this.hobby
        }).then(function() {
    console.log("Document successfully updated!");
           // this.$router.push('/');
           router.replace('/');
    }).catch(function(error) {
    // The document probably doesn't exist.
    console.error("Error updating document: ", error);
});      
}
        }
    };
</script>
