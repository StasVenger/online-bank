<template>
  <nav class="navbar">
    <h3>Online bank</h3>
    <ul class="navbar-menu">
      <li>
        <router-link to="/">Заявки</router-link>
      </li>
      <li>
        <router-link to="/help">Помощь</router-link>
      </li>
      <li>
        <a href="#" @click.prevent="open">Сообщения</a>
      </li>
      <li>
        <a href="#" @click.prevent="logout">Выход</a>
      </li>
    </ul>
  </nav>
</template>

<script>
import {useRouter} from 'vue-router';
import {useStore} from 'vuex';
export default {
  setup() {
    const store = useStore();
    const router = useRouter();

    return {
      logout: () => {
        store.commit('auth/logout')
        router.push('/auth')
      },
      open: () => store.commit('openSidebar')
    }
  }
}
</script>