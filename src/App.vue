<template>
  <div id="app">
    <form name="subForm" @submit.prevent="addSubscription">
      <input v-model="subscribeName" placeholder="Введите название подписки" type="text">
      <input v-model.number="amount" type="number">
      <input v-model="dateStart" type="date">
      <input v-model="dateEnd" type="date">
      <input type="submit" value="Добавить">
    </form>
    <SubscriptionItem
      v-for="(item, index) in subscriptions"
      :key="item.index"
      :index="index"
      :item="item"
      @updateSubscription="updateSubscription"
      @deleteSubscription="deleteSubscription"
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
      dateEnd: new Date()
    }
  },
  mounted() {
    this.subscriptions = JSON.parse(sessionStorage.getItem('subscriptions'))
  },
  methods: {
    addSubscription() {
      let subscription = {
        id: this.subscriptions.length,
        subscribeName: this.subscribeName,
        amount: this.amount,
        dateStart: this.dateStart,
        dateEnd: this.dateEnd
      };

      this.subscriptions.push(subscription);
      this.updateSubscription();
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
      sessionStorage.setItem('subscriptions', JSON.stringify(this.subscriptions));
    },

    cleanForm() {
      this.subscribeName = '';
      this.amount = null;
      this.dateStart = null;
      this.dateEnd = null;
    }
  }
}
</script>

<style></style>
