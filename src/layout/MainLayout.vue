<template>
  <the-nav-bar />
  <the-sidebar />
  <div class="container with-nav">
    <app-message />
    <router-view />
  </div>
</template>

<script>
import TheNavBar from '@/components/TheNavBar';
import AppMessage from '@/components/ui/AppMessage';
import TheSidebar from '@/components/TheSidebar';
export default {
  components: {TheNavBar, AppMessage, TheSidebar}
}
</script>