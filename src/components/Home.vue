<template>
  <header class="header">
    <a href="https://www.penso.com.br/" target="_blank">
      <img class="img-fluid" src="../img/logo-penso.svg" :alt="companyName" :title="companyName">
    </a>
  </header>

  <div class="container-search">
    <select id="filter-options" @change="chooseOption" :class="{'--invalid': filterUser.length > 0 && !optionSelected}">
      <option value="" selected>---</option>
      <option value="name">Nome</option>
      <option value="city">Cidade</option>
    </select>
    <input class="container-search-input" type="text" name="search" placeholder="Search ..."
      @input="filterUser = $event.target.value">
  </div>

  <main>
    <Users :filterUser="filterUser" :optionSelected="optionSelected" :usersEnum="usersEnum"
      :userFiltered="userFiltered"></Users>
  </main>
</template>

<script>
const axios = require('axios');
import Users from './Users.vue';

export default {
  name: 'Home',
  components: {
    Users
  },
  data() {
    return {
      companyName: 'Penso Tecnologia',
      listAllUsers: [],
      usersEnum: [],
      filterUser: '',
      optionSelected: ''
    }
  },
  computed: {
    userFiltered() {
      if (!this.optionSelected) { return; }

      const users = [...this.listAllUsers];

      return users.filter(item => {
        if (this.optionSelected === 'name') {
          return !!item.nome.match(new RegExp(this.filterUser, 'i'))
        } else {
          return !!item.cidade.match(new RegExp(this.filterUser, 'i'))
        }
      })
    }
  },
  methods: {
    chooseOption() {
      const selectOption = document.getElementById('filter-options');
      const valueSelect = selectOption.options[selectOption.selectedIndex].value;

      this.optionSelected = valueSelect;
    }
  },
  created() {
    axios.get('https://jsonplaceholder.typicode.com/users')
      .then(res => {
        if (res.status === 200) {
          const users = res.data;
          const listAllUsers = users.map(item => {
            return {
              nome: item.name,
              email: item.email,
              cidade: item.address.city
            }
          })

          this.listAllUsers = listAllUsers;

          const usersEnum = listAllUsers.filter((item, index) => {
            return !!item && index < 5;
          })

          usersEnum.sort((a, b) => {
            return a.nome < b.nome ? -1 : a.nome > b.nome ? 1 : 0;
          })

          this.usersEnum = usersEnum;

        } else { return; }
      })
      .catch(err => {
        console.error(err);
        throw new Error("Erro na chamada da api.");
      })
  }
}
</script>

<style lang="scss" scoped>
  /* Variables */
$primary-black: #000;
$primary-border-color: #9E9E9E;
$danger-color: #dc3545;

.header {
  max-width: 134px;
  height: auto;
  display: block;
  margin: 0 auto;

  a {
    outline: none;
    text-decoration: none;

    .img-fluid {
      width: 100%;
      height: auto;
    }
  }
}

.container-search {
  max-width: 1280px;
  display: flex;
  justify-content: center;
  margin: 2em auto 0 auto;

  @media screen and (max-width: 768px) {
    height: 43px;
  }

  select {
    width: 200px;
    height: auto;
    padding: 1em;
    text-align: center;
    font-size: 1em;
    background-color: #eceeef;
    color: $primary-black;
    border: none;
    border-radius: 10px;
    font-weight: 500;
    outline: none;
    transition: 0.3s;
    box-sizing: border-box;
    -webkit-appearance: none;
    -moz-appearance: none;
    background-image: linear-gradient(45deg, transparent 50%, $primary-black 0), linear-gradient(135deg, $primary-black 50%, transparent 0);
    background-position: calc(100% - 26px) calc(1em + 4px), calc(100% - 21px) calc(1em + 4px), calc(100% - 2.5em) 0.5em;
    background-size: 10px 6px, 6px 10px, 1px 1.5em;
    background-repeat: no-repeat;

    @media screen and (max-width: 768px) {
      padding: .675em;
      font-size: .975em;
    }
    
    &.--invalid {
      border: 1px solid $danger-color;
      animation: flashing 1.5s infinite;
    }

    @keyframes flashing { 60% { border-color:#fff ; }  }

      option {
        text-align: center;
      }
    }

  .container-search-input {
    border: none;
    padding: 1em 0 1em 16px;
    width: 600px;
    margin-left: 10px;
    background-color: #eceeef;
    border-bottom: 1px solid $primary-border-color;
    border-radius: 10px;
    font-size: 1em;
    font-weight: 400;
    text-transform: capitalize;
    top: 0;
    position: sticky;

    @media screen and (max-width: 768px) {
      width: 70%;
      font-size: .975em;
      padding: .675em;
    }

    &:focus {
      border: none;
      outline: none;
      background: inherit;
      border-bottom: 1px solid $primary-border-color;
      background-color: #EDEDED
    }
  }
}

main {
  height: 400px;
}
</style>
