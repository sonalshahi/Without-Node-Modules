<template>
  <div id="home">
    <h4>About You</h4>
    <table>
    <thead>

      <tr>
        <th v-for="key in columns">
          {{key}}
          </span>
        </th>
      </tr>

    </thead>

    <tbody>


      <tr>
        <td>
          {{user.Name}}
        </td>
        <td>
          {{user.Email}}
        </td>
        <td>
          {{user.contact}}
        </td>
        <td>
          {{user.Hobby}}
        </td>
        <td>
          {{user.Age}}
        </td>
      </tr>

    </tbody>
    </table>
   <div class="fixed-action-btn">
      <router-link to="/edituser" class="btn-floating btn-large red">
        <i class="fa fa-edit"></i>
      </router-link>
    </div>
  </div>
</template>

<script>
var w = '';
var column = ['Name','Email','Contact','Hobby','Age'];
import firebase from 'firebase';
import db from './firebaseInit';
export default {
  name: 'home',
  data() {
    return {
      user: {},
      loading: true,
      columns: []
    };
  },
  created() {
    this.columns = column;
    if (firebase.auth().currentUser) {
      w = firebase.auth().currentUser.email;
    //  this.isLoggedIn = true;
    //  this.currentUser = firebase.auth().currentUser.email;
    }
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
            Age: doc.data().Age
          };
          if(w == data.Email){
              this.user = data;
          }
        });
      });
  }
};
</script>


<style>

table {
  border: 2px solid #42b983;
  border-radius: 3px;
  background-color: #fff;
}

th {
  background-color: #42b983;
  color: rgba(255,255,255,0.66);
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

td {
  background-color: #f9f9f9;
}

th, td {
  min-width: 120px;
  padding: 10px 20px;
}


</style>