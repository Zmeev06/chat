<script setup lang="ts">
import { RouteMeta, RouterView, useRouter } from "vue-router";
import { ref } from "vue";
import ProgressSpin from "../components/ProgressSpin.vue";

const router = useRouter();
const isShowLoader = ref(false);

const getTransitionName = (transitionMeta: RouteMeta) => {
  if (
    transitionMeta &&
    typeof transitionMeta === "object" &&
    transitionMeta.transition &&
    typeof transitionMeta.transition === "string"
  ) {
    return transitionMeta.transition;
  }
  return "fade";
};

router.beforeEach(() => {
  isShowLoader.value = true;
});
router.afterEach(() => {
  isShowLoader.value = false;
});
</script>

<template>
  <RouterView v-slot="{ Component, route }">
    <template v-if="Component">
      <Transition :name="getTransitionName(route.meta)">
        <Suspense>
          <template #default>
            <component :is="Component" />
          </template>
          <template #fallback> <ProgressSpin/> </template>
        </Suspense>
      </Transition>
    </template>
    <template v-if="isShowLoader">
      <MainLoader />
    </template>
  </RouterView>
</template>
<style></style>
