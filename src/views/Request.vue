<template>
  <app-loader v-if="loading"></app-loader>
  <app-page back title="Заявка" v-else-if="request">
    <p><strong>Имя владельца</strong>: {{request.fio}}</p>
    <p><strong>Телефон</strong>: {{request.phone}}</p>
    <p><strong>Сумма</strong>: {{currency(request.sum)}}</p>
    <p><strong>Статус</strong>: <AppStatus :type="request.status" /></p>

    <div class="form-control">
      <label for="status">Статус</label>
      <select id="status" v-model="status">
        <option value="done">Завершен</option>
        <option value="cancelled">Отменен</option>
        <option value="active">Активен</option>
        <option value="pending">Выполняется</option>
      </select>
    </div>

    <button class="btn danger" @click="remove">Удалить</button>
    <button class="btn" @click="update" v-if="hasChanged">Обновить</button>
  </app-page>
  <h3 v-else class="text-center text-white">
    Заявки с ID = {{route.params.id}} нет.
  </h3>
</template>

<script>
import {ref, onMounted, computed} from 'vue';
import {useRoute, useRouter} from 'vue-router';
import {useStore} from 'vuex';
import AppPage from '@/components/ui/AppPage';
import AppLoader from '@/components/ui/AppLoader';
import AppStatus from '@/components/ui/AppStatus';
import {currency} from '@/utils/currency-formatter';

export default {
  setup() {
    const route = useRoute()
    const router = useRouter()
    const store = useStore()
    const status = ref()
    const loading = ref(true)
    const request = ref({})

    onMounted(async () => {
      loading.value = true
      request.value = await store.dispatch('request/loadById', route.params.id)
      status.value = request.value?.status
      loading.value = false
    })

    const hasChanged = computed(() => request.value.status !== status.value)

    const remove = async () => {
      await store.dispatch('request/remove', route.params.id)
      router.push('/')
    }

    const update = async () => {
      const data = {...request.value, status: status.value, id: route.params.id}
      await store.dispatch('request/update', data)
      request.value.status = status.value
    }

    return {
      loading,
      remove,
      update,
      request,
      currency,
      status,
      hasChanged
    }
  },
  components: {AppPage, AppLoader, AppStatus}
}
</script>