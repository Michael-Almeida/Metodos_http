<template>
  <div id="app">
    <h1>controle de usuáriso</h1>
    <div>
      <h4>Listagem</h4>
      <ul>
        <li v-for="user in usuarios" :key="user._id">
          {{ user.firstName }} {{ user.lastName }}
        </li>
      </ul>
    </div>
    <div>
      <hr />
      <h4>Cadastro</h4>
      <div>
        <label>Primeiro nome</label>
        <input type="text" v-model="firstName" />
      </div>
      <div>
        <label>Sobrenome</label>
        <input type="text" v-model="lastName" />
      </div>
      <div>
        <label>Idade</label>
        <input type="text" v-model="age" />
      </div>
      <div>
        <label>username</label>
        <input type="text" v-model="username" />
      </div>
      <div>
        <label>password</label>
        <input type="password" v-model="password" />
      </div>
      <button @click="addUser">Enviar</button>
      <p>{{ message }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: function() {
    return {
      usuarios: [],
      firstName: "",
      lastName: "",
      age: "",
      username: "",
      password: "",
      message: "",
    };
  },

  methods: {
    getUser: async function() {
      const result = await fetch("http://localhost:3000/")
        .then((res) => {
          return res.json();
        })
        .catch((error) => {
          return {
            error: true,
            message: error,
          };
        });
      if (!result.error) {
        this.usuarios = result;
      }
    },
    addUser: async function() {
      const newUser = {
        firstName: this.firstName,
        lastName: this.lastName,
        age: this.age,
        username: this.username,
        password: this.password,
      };

      if (newUser.firstName === "") {
        this.message = "Primeiro nome é obrigatório";
        return;
      }

      if (newUser.lastName === "") {
        this.message = "Sobrenome  é obrigatório";
        return;
      }

      if (newUser.age === "") {
        this.message = "Idade é obrigatória";
        return;
      }

      if (newUser.username === "") {
        this.message = "Username é obrigatório";
        return;
      }

      if (newUser.password === "") {
        this.message = "Senha  é obrigatório";
        return;
      }

      const result = await fetch('http://localhost:3000/', {
        headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          method: "POST",
          body: JSON.stringify(newUser)
        })
        .then(res => res.json())
        .then(res => res)
        .catch(error => {
          return {
            error : true,
            message : error
          }
        });

       if(result.insertedId){ 
        this.message = "Usuário cadastrado com sucesso"; 
      } 
    },
  },
  created: function() {
    this.getUser();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  padding: 30px;
}

label {
  display: block;
}

input {
  height: 30px;
  width: 300px;
  margin-bottom: 30px;
}
</style>
