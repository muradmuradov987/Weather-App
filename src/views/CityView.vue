<template>
  <div>
   <Suspense>
     <AsyncCityView />
     <template #fallback>
      <p>Loading...</p>
     </template>
   </Suspense>
  </div>
</template>

<script>
import AsyncCityView from "@/components/AsyncCityView.vue";
export default {
  components: {
    AsyncCityView
  },
};
</script>
