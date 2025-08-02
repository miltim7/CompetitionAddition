<template>
  <div>
    <button @click="openModal">Открыть модалку</button>
    <Modal ref="modal" />
  </div>
</template>

<script>
import Modal from '@/components/Modal.vue'

export default {
  name: 'HomeView',
  components: {
    Modal
  },
  methods: {
    openModal() {
      this.$refs.modal.showModal()
    }
  }
}
</script>