<script setup lang="ts">
import { ref } from 'vue'
import StepOneComponent from './StepOneComponent.vue'
import StepTwoComponent from './StepTwoComponent.vue'
import StepThreeComponent from './StepThreeComponent.vue'
import StepFourComponent from './StepFourComponent.vue'
import ConfirmComponent from './ConfirmComponent.vue'

const activeStep = ref(1)

const steps = [1, 2, 3, 4]

const nextStep = () => {
  activeStep.value += 1
}

const goBack = () => {
  activeStep.value -= 1
}

const changeStep = (step: number) => {
  activeStep.value = step
}
</script>

<template>
  <div class="w-full relative">
    <img
      src="/images/bg-sidebar-mobile.svg"
      alt="sidebar mobile"
      class="w-full h-auto relative z-30"
    />
    <div
      class="flex items-center justify-center gap-4 mt-[-134px] relative z-30 mb-3"
    >
      <div
        v-for="step in steps"
        :key="step"
        :class="`w-8 h-8 rounded-full border border-brand-white ${
          activeStep === step && 'bg-brand-light-blue'
        } overflow-hidden`"
      >
        <div
          :class="`w-full h-full flex justify-center items-center text-sm ${
            activeStep === step
              ? 'text-brand-marine-blue brand-bold'
              : 'text-brand-alabaster brand-medium'
          }`"
        >
          {{ step }}
        </div>
      </div>
    </div>
    <div class="w-full px-4 relative bg-brand-magnolia pt-4 pb-20">
      <div class="w-full rounded-xl bg-brand-white py-8 px-6 relative z-40">
        <StepOneComponent v-show="activeStep === 1" @change-step="changeStep" />
        <StepTwoComponent v-show="activeStep === 2" @change-step="changeStep" />
        <StepThreeComponent
          v-show="activeStep === 3"
          @change-step="changeStep"
        />
        <StepFourComponent
          v-show="activeStep === 4"
          @change-step="changeStep"
        />
        <ConfirmComponent v-show="activeStep === 0" />
      </div>
    </div>
    <!-- <div class="flex justify-between items-center w-full p-4 pb-3">
      <button
        v-if="activeStep > 1"
        class="text-sm capitalize brand-medium text-brand-cool-gray"
        @click="goBack"
      >
        go back
      </button>
      <div></div>
      <button
        v-if="activeStep > 1 && activeStep < 4"
        class="h-10 px-4 capitalize brand-regular text-sm text-brand-alabaster bg-brand-marine-blue rounded"
        @click="nextStep"
      >
        next step
      </button>
      <button
        v-if="activeStep === 4"
        class="h-10 px-6 capitalize brand-regular text-sm text-brand-alabaster bg-brand-purplish-blue rounded"
        @click="() => (activeStep = 0)"
      >
        confirm
      </button>
    </div> -->
  </div>
</template>
