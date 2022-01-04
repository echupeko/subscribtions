<template>
  <div>
    <form @submit.prevent="updateSubscription">
      <input v-model="item.subscribeName" @change="isEdited = true" type="text">
      <input v-model.number="item.amount" @change="isEdited = true" type="number">
      <input v-model="item.dateStart" @change="isEdited = true" type="date">
      <input v-model="item.dateEnd" @change="isEdited = true" type="date">
      <input v-if="isEdited" type="submit" value="Сохранить">
      <input @click="deleteSubscription" type="button" value="Удалить">
    </form>
  </div>
</template>

<script>
export default {
  name: 'SubscriptionItem',
  props: ['item', 'index'],
  data() {
    return {
      isEdited: false
    }
  },
  methods: {
    updateSubscription() {
      this.$emit('updateSubscription', this.item, this.index)
      this.isEdited = !this.isEdited;
    },

    deleteSubscription() {
      this.$emit('deleteSubscription', this.index)
    }
  }
}
</script>

<style scoped></style>