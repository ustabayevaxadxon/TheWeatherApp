<template>
    <teleport to="body">
        <div class="wrapper">
            <transition name="modal-outer">
                <div v-show="showModal" class="absolute w-full bg-black bg-opacity-30 h-screen top-0 left-0 flex justify-center px-8">
                    <transition name="modal-inner">
                        <div v-if="showModal" class="p-4 bg-white self-start mt-32 max-w-screen-md rounded">
                            <slot></slot>
                            <button @click="$emit('close-modal')"  class="rounded text-white mt-8 bg-weather-primary py-2 px-6">
                                Close
                            </button>
                        </div>
                    </transition>
                </div>
            </transition>
        </div>
    </teleport>
</template>

<script setup>
defineEmits(['close-modal'])
defineProps({
    showModal: {
        type: Boolean,
        default: false
    }
})
</script>

<style scoped lang="scss">
.wrapper{
  .modal-outer-enter-active,
  .modal-outer-leave-active {
    transition: opacity 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
  }

  .modal-outer-enter-from,
  .modal-outer-leave-from {
    opacity: 0;
  }

  .modal-inner-enter-active {
    transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02) 0.15s;
  }
  .modal-inner-leave-active {
    transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
  }
  .modal-inner-enter-from {
    opacity: 0;
    transform: scale(0.8);
  }
  .modal-inner-leave-to {
      transform: scale(0.1);
  }
}
</style>