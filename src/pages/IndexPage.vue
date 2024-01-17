<template>
  <q-page class="flex flex-center">
    <UserCardVue />
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'
import UserCard from 'src/components/UserCard.vue';
import UserCardVue from 'src/components/UserCard.vue';


export default defineComponent({
  name: 'IndexPage',
  components: {
    UserCardVue,
  },
  setup() {
    return {

    }
  }
})
</script>
