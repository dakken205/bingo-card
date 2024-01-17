<template>
  <q-layout view="lHh Lpr lFf" style="font-family: 'MochiyPopOne-Regular';">
    <q-header elevated>
      <q-toolbar>

        <q-toolbar-title class="flex flex-center">
          新年会　DA研　ビンゴ大会
        </q-toolbar-title>

      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'MainLayout',
})
</script>
