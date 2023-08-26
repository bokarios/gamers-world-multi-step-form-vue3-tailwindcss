<script setup lang="ts">
import { useSubscriptionStore } from '@/stores/subscription'

defineEmits(['change-step'])

const plans = ['arcade', 'advanced', 'pro']
const plansPrice = [9, 12, 15]

const calculateTotal = () => {
  let total = 0

  if (useSubscriptionStore().billing === 'monthly') {
    total += plansPrice[useSubscriptionStore().plan - 1]
    useSubscriptionStore().addOns.forEach((addOn) => (total += addOn.price))
    return total
  }

  total += plansPrice[useSubscriptionStore().plan - 1] * 10
  useSubscriptionStore().addOns.forEach((addOn) => (total += addOn.price * 10))
  return total
}
</script>

<template>
  <div class="flex flex-col justify-between w-full h-full">
    <div class="w-full">
      <div class="text-2xl md:text-4xl brand-bold text-brand-marine-blue mb-3">
        Finishing up
      </div>
      <div class="text-body brand-regular text-brand-cool-gray mb-6 md:mb-9">
        Double-check everything looks OK before confirming.
      </div>
      <div
        class="w-full rounded-xl bg-brand-alabaster px-4 md:px-6 py-3 md:py-5 mb-6"
      >
        <div class="w-full flex justify-between items-center">
          <div>
            <div
              class="text-sm md:text-body brand-bold text-brand-marine-blue capitalize"
            >
              {{ plans[useSubscriptionStore().plan - 1] }} ({{
                useSubscriptionStore().billing
              }})
            </div>
            <button
              class="brand-medium text-brand-cool-gray text-sm p-0 relative"
              @click="$emit('changeStep', 2)"
            >
              Change
              <div
                class="absolute bottom-0 w-full h-0.5 bg-brand-cool-gray"
              ></div>
            </button>
          </div>
          <div class="text-sm md:text-body brand-bold text-brand-marine-blue">
            ${{
              useSubscriptionStore().billing === 'monthly'
                ? plansPrice[useSubscriptionStore().plan - 1]
                : plansPrice[useSubscriptionStore().plan - 1] * 10
            }}/{{ useSubscriptionStore().billing === 'monthly' ? 'mo' : 'yr' }}
          </div>
        </div>
        <hr
          v-if="useSubscriptionStore().addOns.length > 0"
          class="mt-3 md:mt-6 mb-4 md:mb-5"
        />
        <div class="flex flex-col gap-3 md:gap-4 mb-2">
          <div
            v-for="addOn in useSubscriptionStore().addOns"
            :key="addOn.id"
            class="w-full flex justify-between items-center"
          >
            <div class="brand-regular text-sm text-brand-cool-gray capitalize">
              {{ addOn.name }}
            </div>
            <div class="brand-medium text-sm text-brand-marine-blue">
              +${{
                useSubscriptionStore().billing === 'monthly'
                  ? addOn.price
                  : addOn.price * 10
              }}/{{
                useSubscriptionStore().billing === 'monthly' ? 'mo' : 'yr'
              }}
            </div>
          </div>
        </div>
      </div>
      <div class="w-full flex justify-between items-center px-6">
        <div class="brand-regular text-sm text-brand-cool-gray">
          Total (per
          {{ useSubscriptionStore().billing === 'monthly' ? 'month' : 'year' }})
        </div>
        <div class="brand-bold text-lg md:text-xl text-brand-purplish-blue">
          +${{ calculateTotal() }}/{{
            useSubscriptionStore().billing === 'monthly' ? 'mo' : 'yr'
          }}
        </div>
      </div>
    </div>
    <div
      class="flex justify-between items-center w-full absolute bottom-[-136px] right-0 md:static"
    >
      <button
        class="text-sm md:text-body capitalize brand-medium text-brand-cool-gray"
        @click="$emit('change-step', 3)"
      >
        go back
      </button>
      <button
        class="h-10 md:h-12 px-6 md:px-8 capitalize brand-regular text-sm md:text-body text-brand-alabaster bg-brand-purplish-blue rounded md:rounded-lg"
        @click="$emit('change-step', 0)"
      >
        confirm
      </button>
    </div>
  </div>
</template>
