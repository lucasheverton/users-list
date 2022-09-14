<template>
  <table>
    <tr>
      <th>Nome</th>
      <th>Email</th>
      <th>Cidade</th>
    </tr>

    <tr v-if="optionSelected && filterUser && userFiltered.length < 1">
      <td colspan="3"><strong>Nehuma informação foi encontrada</strong></td>
    </tr>

    <tr v-else v-for="(user, index) in !filterUser || !optionSelected ? usersEnum : userFiltered" :key="index">
      <td v-text="user.nome"></td>
      <td v-text="user.email"></td>
      <td v-text="user.cidade"></td>
    </tr>


  </table>
</template>

<script>
export default {
  name: 'Users',
  props: {
    filterUser: {
      type: String,
      required: false
    },
    optionSelected: {
      type: String,
      required: false
    },
    usersEnum: {
      type: Array,
      required: false
    },
    userFiltered: {
      type: Array,
      required: false
    }
  }
}
</script>

<style lang="scss" scoped>
  /* Variables */
$primary-white: #fff;
$primary-blue: #000cd9;

table {
  margin: 3em auto 0 auto;
  width: 830px;
  box-shadow: 0 2px 20px 0 rgb(34 31 31 / 10%);
  border-radius: 0 0 10px 10px;

  @media screen and (max-width: 844px) {
    width: 100%;
  }

  tr:first-child {
    background-color: $primary-blue;
    color: $primary-white;

    th {
      padding: 1em;
      font-size: 1.05em;
      font-weight: 600;
      border-radius: 10px 10px 0 0;

      @media screen and (max-width: 768px) {
        font-size: .975em;
        padding: 0.675em;
      }
    }
  }

  tr {
    background-color: $primary-white;
    padding: 0.75em;
    height: 40px;

    &:hover {
      cursor: pointer;
      box-shadow: 0 2px 20px 0 rgb(34 31 31 / 10%);
    }
  }

  td {
    height: 40px;
    font-size: 1em;
    font-weight: 400;

    @media screen and (max-width: 768px) {
      font-size: .875em;
      word-break: break-word;
      padding: 0 5px;
    }
  }
}
</style>
