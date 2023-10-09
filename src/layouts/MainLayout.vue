<template>
  <q-layout view="lHh lpR fFf" class="ing-layout">
    <q-header bordered class="ing-layout__header">
      <q-toolbar>
        <q-btn dense flat round icon="la la-bars" @click="toggleLeftDrawer"/>
        <q-toolbar-title>
        </q-toolbar-title>
      </q-toolbar>
    </q-header>


    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      side="left" bordered
      class="ing-layout__left-drawer"
    >
      <q-toolbar class="ing-layout__left-drawer__toolbar">
        <q-item class="q-mt-lg q-px-lg">
          <q-item-section avatar>
            <img
              alt="Logo"
              src="logo.png"
              style="width: 32px; height: 32px"
            >
          </q-item-section>
          <q-item-section class="text-subtitle2 text-weight-bolder">Admin Dashboard</q-item-section>
        </q-item>
      </q-toolbar>

      <q-list padding class="q-mt-lg q-px-lg ing-layout__left-drawer__list">
        <q-item clickable>
          <q-item-section avatar>
            <q-icon name="la la-qrcode"/>
          </q-item-section>
          <q-item-section>
            Registrations
          </q-item-section>
        </q-item>

        <q-item active clickable>
          <q-item-section avatar>
            <q-icon name="la la-gift"/>
          </q-item-section>

          <q-item-section>
            Raffles
          </q-item-section>
        </q-item>

        <q-item clickable>
          <q-item-section avatar>
            <q-icon name="la la-users-cog"/>
          </q-item-section>

          <q-item-section>
            Users
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>


    <q-page-container>
      <router-view/>
    </q-page-container>
  </q-layout>
</template>

<script>
import { ref } from 'vue'

export default {
  setup () {
    const leftDrawerOpen = ref(false)

    return {
      leftDrawerOpen,
      toggleLeftDrawer () {
        leftDrawerOpen.value = !leftDrawerOpen.value
      }
    }
  }
}
</script>
