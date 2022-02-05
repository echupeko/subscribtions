<template>
  <div>
    <input v-model="item.subscribeName" :class="{ 'error-block': !checkEmpty(item.subscribeName) }" @change="modified" type="text">
    <input v-model.number="item.amount" :class="{ 'error-block': !checkEmpty(item.amount) }" @change="modified" type="number">
    <input v-model="item.dateStart" :class="{ 'error-block': !checkEmpty(item.dateStart) }" @change="modified" type="date">
    <input v-model="item.dateEnd" :class="{ 'error-block': !checkEmpty(item.dateEnd) }" @change="modified" type="date">
    <input v-model="item.accountName" :class="{ 'error-block': !checkEmpty(item.accountName) }" @change="modified" placeholder="Введите аккаунт, через который оформлена подписка" type="text">
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
    checkEmpty(value) {
      return Boolean(''+value);
    },

    modified() {
      this.isEdited = true;
    },

    updateSubscription() {
      for(let key in this.item) {
        let isNotEmpty = Boolean(''+this.item[key]);
        if(!isNotEmpty) {
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