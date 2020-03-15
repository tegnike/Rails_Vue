<template>
  <!-- @submit="createEmployee"はsubmitイベントを受け取って、createEmployeeメソッドを実行することを指している。 -->
  <!-- :errors="errors" と :employee="employee"は自身のデータ errors と employee を同名の props として子コンポーネントに渡している。 -->
  <employee-form-pane :errors="errors" :employee="employee" @submit="createEmployee"></employee-form-pane>
</template>

<script>
  import axios from 'axios';

  import EmployeeFormPane from 'EmployeeFormPane.vue';

  export default {
    components: {
      EmployeeFormPane
    },
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
