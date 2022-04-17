<template>
  <q-layout view="lHh lpR lFf">
    <q-header class="mx-8 my-4 rounded drop-shadow-lg">
      <q-toolbar>
        <q-btn
          class="visible md:invisible"
          dense
          flat
          round
          icon="menu"
          @click="toggleLeftDrawer" />

        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg" />
          </q-avatar>
          Title
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-drawer
      show-if-above
      v-model="leftDrawerOpen"
      side="left"
      bordered
      :mini="miniState"
      @mouseover="miniState = false"
      @mouseout="miniState = true"
      mini-to-overlay>
      <!-- drawer content -->
      <q-list bordered padding>
        <template v-for="(item, index) in generatedRoutes" :key="index">
          <q-item
            clickable
            :active="item.name === item.path"
            :to="item.path"
            v-if="item.meta?.layout == null  || item.meta?.layout == 'default'"
            exact>
            <q-item-section avatar>
              <q-icon :name="item.meta?.icon ? item.meta?.icon : 'inbox'" />
            </q-item-section>
            <q-item-section class="cursor-pointer" style="text-transform: capitalize">
              {{ item.name == 'index' ? 'Home' :   item.name.replace('-', ' ')}}
            </q-item-section>
          </q-item>
        </template>
      </q-list>
    </q-drawer>

    <!-- Content -->
    <q-page-container>
      <router-view class="pt-4" v-slot="{ Component }">
        <transition name="slide-fade" mode="out-in">
          <component :is="Component" />
        </transition>
      </router-view>
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import { useRouter } from 'vue-router'

  import generatedRoutes from 'virtual:generated-pages'
  const router = useRouter()

  const leftDrawerOpen = ref<boolean>(false)
  const toggleLeftDrawer = () => {
    leftDrawerOpen.value = !leftDrawerOpen.value
  }

  const miniState = ref(true)
</script>
<style lang="scss">
  .slide-fade-enter {
    transform: translateX(10px);
    opacity: 0;
  }

  .slide-fade-enter-active,
  .slide-fade-leave-active {
    transition: all 0.2s ease;
  }

  .slide-fade-leave-to {
    transform: translateX(-10px);
    opacity: 0;
  }
</style>
