<template>
  <div class="customBackground">
    <v-container>
      <v-row align="center" justify="center">
        <v-col cols="12" sm="9">
          <v-card class="elevation-6 mt-15">
            <v-window v-model="step">
              <v-window-item :value="1">
                <LoginStep v-on:register="step++" />
              </v-window-item>

              <v-window-item :value="2">
                <RegisterStep v-on:login="step--" />
              </v-window-item>
            </v-window>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import { defineComponent } from 'vue';

import LoginStep from '../components/Home/LoginStep.vue';
import RegisterStep from '../components/Home/RegisterStep.vue';

export default defineComponent({
  name: 'HomeView',

  data: () => ({
    step: 1
  }),

  components: {
    LoginStep,
    RegisterStep
  }
});
</script>

<style scoped>
.customBackground {
  background: linear-gradient(-45deg, #a7a7a7, #c9aad6, #8ab7c7, #97e5d3);
  background-size: 400% 400%;
  animation: gradient 15s ease infinite;
  height: 100vh;
}

@keyframes gradient {
  0% {
    background-position: 0% 50%;
  }

  50% {
    background-position: 100% 50%;
  }

  100% {
    background-position: 0% 50%;
  }
}
</style>
