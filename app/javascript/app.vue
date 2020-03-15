<template>
  <div>
    <router-view></router-view>
  </div>
</template>

<script>
  import Vue from 'vue'
  import VueRouter from 'vue-router'

  import EmployeeIndexPage from 'EmployeeIndexPage.vue'
  import EmployeeDetailPage from 'EmployeeDetailPage.vue'
  import EmployeeNewPage from 'EmployeeNewPage.vue'

  // 指定のURLにリクエストが来たらまずここを経由する。
  // 次にそのpathに基づくcomponentのファイルを探す。
  // 見つけたらその内容を表示する。
  // そのファイルに書かれている処理（API->controller->...）を実行する。
  const router = new VueRouter({
    routes: [
      { path: '/',
        component: EmployeeIndexPage },
      { path: '/employees/:id(\\d+)',  // :id は数値のみに制限する
        name: 'EmployeeDetailPage',
        // ↑ ルートに名前を付けている
        // rootからリンクを貼るのに必要 ref. https://router.vuejs.org/ja/guide/essentials/named-routes.html
        component: EmployeeDetailPage  },
      { path: '/employees/new',
        name: 'EmployeeNewPage',
        component: EmployeeNewPage    }
    ]
  })

  Vue.use(VueRouter)

  export default {
    router
  }
</script>

<style scoped>
</style>
