<template>
  <div id="app" class="w-full h-full flex flex-col justify-center bg-gray-300">
    <div
      v-if="showPasswordReset"
      class="w-full h-full flex flex-col justify-center items-center p-4"
    >
      <PasswordReset />
    </div>
    <div
      v-else-if="userSession === null"
      class="w-full h-full flex flex-col justify-center items-center p-4"
    >
      <Auth />
    </div>
    <div
      v-else
      class="
        w-full
        h-full
        flex flex-col
        justify-center
        items-center
        p-4
        max-w-sm
        m-auto
      "
    >
      <Suspense>
        <template #default>
          <div>
            <TodoList />
            <button class="btn-black w-full mt-12" @click="handleLogout">
              Logout
            </button>
          </div>
        </template>
        <template #fallback>
          <Loading />
        </template>
      </Suspense>
    </div>
    <Footer />
  </div>
</template>

<script lang="ts">
import Auth from '@/components/Auth.vue'
import PasswordReset from '@/components/PasswordReset.vue'
import TodoList from '@/components/TodoList.vue'
import Loading from '@/components/Loading.vue'
import Footer from '@/components/Footer.vue'
import { userSession, handleLogout } from '@/utils/useAuth'
import { getParameterByName } from '@/lib/helpers'

export default {
  components: {
    Auth,
    PasswordReset,
    TodoList,
    Loading,
    Footer,
  },
  // for no particular reason this setup fails with @typescript-eslint/explicit-module-boundary-types
  // https://github.com/typescript-eslint/typescript-eslint/blob/master/packages/eslint-plugin/docs/rules/explicit-module-boundary-types.md
  /* eslint-disable @typescript-eslint/explicit-module-boundary-types */
  setup() {
    return { userSession, handleLogout }
  },
  /* eslint-enable @typescript-eslint/explicit-module-boundary-types */
  computed: {
    showPasswordReset: function (): boolean {
      const requestType = getParameterByName('type', location.href)
      return requestType === 'recovery'
    },
  },
}
</script>

<style>
#app {
  @apply h-screen min-h-screen antialiased;
}
</style>
