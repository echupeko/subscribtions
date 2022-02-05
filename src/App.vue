<template>
  <div id="app">
    <header>
      <h1>Мои подписки</h1>
    </header>
    <div>
      <button v-if="!hiddenForm" @click="toggleForm">Добавить подписку</button>
    </div>

    <form v-if="hiddenForm" name="subForm" @submit.prevent="addSubscription">
      <input v-model="subscribeName" name="subscribeName" placeholder="Введите название подписки" type="text" required>
      <input v-model.number="amount" name="amount" type="number" required>
      <input v-model="dateEnd" name="dateEnd" type="date" required>
      <input v-model="dateStart" name="dateStart" type="date" required>
      <input v-model="accountName" name="accountName" placeholder="Введите аккаунт, через который оформлена подписка" type="text" required>
      <input type="submit" value="Добавить">
      <button @click="toggleForm">Х</button>
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
      hiddenForm: false,
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
    toggleForm() {
      this.hiddenForm = !this.hiddenForm;
    },

    addSubscription() {
      let formData = new FormData(document.forms[0]);
      this.subscriptions.push(Object.fromEntries(formData));
      this.updateSubscriptions();
      this.cleanForm();
      this.toggleForm();
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
@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap');

* {
  margin: 0;
  padding: 0;
  font-family: 'Roboto', sans-serif;

}

input {
  border: 1px solid gray;
}

h1 {
  font-size: 107px;
  font-weight: lighter;
  text-transform: uppercase;
}
</style>
