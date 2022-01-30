<template>
  <div>
    <input v-model="item.subscribeName" :class="{ 'error-block': !item.subscribeName}" @change="modified" type="text">
    <input v-model.number="item.amount" :class="{ 'error-block': !item.amount}" @change="modified" type="number">
    <input v-model="item.dateStart" :class="{ 'error-block': !item.dateStart}" @change="modified" type="date">
    <input v-model="item.dateEnd" :class="{ 'error-block': !item.dateEnd}" @change="modified" type="date">
    <input v-model="item.accountName" :class="{ 'error-block': !item.accountName}" @change="modified" placeholder="Введите аккаунт, через который оформлена подписка" type="text">
    <button @click="updateSubscription(item)" v-if="isEdited">Сохранить</button>
    <button @click="$emit('deleteSubscription')">Удалить</button>
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
    modified() {
      this.isEdited = true;
    },

    updateSubscription() {
      for(let key in this.item) {
        if(!this.item[key]) {
          alert('Поле ' + key + ' пустое')
          return;
        }
      }
      this.$emit('updateSubscription');
      this.isEdited = !this.isEdited;
    }
  }
}
</script>

<style scoped>
  .error-block {
    border-width: 2px;
    border-color: red;
  }
</style>