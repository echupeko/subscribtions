<template>
  <div id="app">
    <form name="subForm" @submit.prevent="addSubscription">
      <input v-model="subscribeName" name="subscribeName" placeholder="Введите название подписки" type="text" required>
      <input v-model.number="amount" name="amount" type="number" required>
      <input v-model="dateEnd" name="dateEnd" type="date" required>
      <input v-model="dateStart" name="dateStart" type="date" required>
      <input v-model="accountName" name="accountName" placeholder="Введите аккаунт, через который оформлена подписка" type="text" required>
      <input type="submit" value="Добавить">
    </form>
    <SubscriptionItem
      v-for="(item, index) in subscriptions"
      :key="item.index"
      :index="index"
      :item="item"
      @updateSubscription="updateSubscription(item, index)"
      @deleteSubscription="deleteSubscription(index)"
    />
  </div>
</template>

<script>
import SubscriptionItem from './components/SubscriptionItem.vue'

export default {
  name: 'App',
  components: {
    SubscriptionItem
  },

  data() {
    return {
      subscriptions: [],
        subscribeName: '',
        amount: 0,
        dateStart: new Date(),
        dateEnd: new Date(),
        accountName: ''
    }
  },

  mounted() {
    this.subscriptions = JSON.parse(localStorage.subscriptions) || [];
  },

  methods: {
    addSubscription() {
      let formData = new FormData(document.forms[0]);
      this.subscriptions.push(Object.fromEntries(formData));
      this.updateSubscriptions();
      this.cleanForm();
    },

    deleteSubscription(index) {
      this.subscriptions.splice(index, 1);
      this.updateSubscriptions();
    },

    updateSubscription(item, index) {
      this.subscriptions[index] = item;
      this.updateSubscriptions();
    },

    updateSubscriptions() {
      localStorage.subscriptions = JSON.stringify(this.subscriptions);
    },

    cleanForm() {
      this.subscribeName = '';
      this.amount = null;
      this.dateStart = null;
      this.dateEnd = null;
      this.accountName = '';
    }
  }
}
</script>

<style>
input {
  border: 1px solid gray;
}
</style>
