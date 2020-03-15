<template>
  <!-- .prevent とすることで、通常の submit で処理されるページリロードを行わないようにしている。 -->
  <form @submit.prevent="createEmployee">
    <div v-if="errors.length != 0">
      <ul v-for="e in errors" :key="e">
        <li><font color="red">{{ e }}</font></li>
      </ul>
    </div>
    <div>
      <label>Name</label>
      <input v-model="employee.name" type="text">
    </div>
    <div>
      <label>Department</label>
      <input v-model="employee.department" type="text">
    </div>
    <div>
      <label>Gender</label>
      <select v-model="employee.gender">
        <option>other</option>
        <option>male</option>
        <option>female</option>
      </select>
    </div>
    <div>
      <label>Birth</label>
      <input v-model="employee.birth" type="date">
    </div>
    <div>
      <label>Joined Date</label>
      <input v-model="employee.joined_date" type="date">
    </div>
    <div>
      <label>Payment</label>
      <input v-model="employee.payment" type="number" min="0">
    </div>
    <div>
      <label>Note</label>
      <input v-model="employee.note" type="text">
    </div>
    <button type="submit">Commit</button>
  </form>
</template>

<script>
  import axios from 'axios';

  export default {
    data: function () {
      return {
        employee: {
          name: '',
          department: '',
          gender: '',
          birth: '',
          joined_date: '',
          payment: '',
          note: ''
        },
        errors: ''
      }
    },
    methods: {
      createEmployee: function() {
        axios
          .post('/api/v1/employees', this.employee)
          // モデル作成用の PIにデータを送信し、失敗したら this.errorsにエラー内容を格納するようにしている。
          // モデル作成が成功したら詳細画面に遷移する。
          .then(response => {
            let e = response.data;
            this.$router.push({ name: 'EmployeeDetailPage', params: { id: e.id } });
          })
          .catch(error => {
            console.error(error);
            if (error.response.data && error.response.data.errors) {
              this.errors = error.response.data.errors;
            }
          });
      }
    }
  }
</script>

<style scoped>
</style>
