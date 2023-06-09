<script setup>
import { storeToRefs } from 'pinia';

import { useAlertStore, useAuthStore, useUsersStore } from '../stores';
import { formValues } from '../helpers';
import { autoFocus as vAutoFocus } from '../directives';

const authStore = useAuthStore();
const usersStore = useUsersStore();
const alertStore = useAlertStore();
const { user } = storeToRefs(authStore);

async function onSubmit(e) {
  const cardData = formValues(e);

  await usersStore.update(user.value.id, { cardData });
  console.log(formValues(e, 4));
  formValues(e, 4, true);
  try {
    alertStore.success('Credit card data has been stored');
  } catch (error) {
    alertStore.error(error);
  }
}
</script>

<template>
  <div class="layout-container" v-if="user">
    <div class="deposit-container">
      <h1>Hi {{ user.firstName }}!</h1>
      <div class="form-container">
        <form @submit.prevent="onSubmit">
          <div class="input-container">
            <div class="input-element">
              <label for="name">Name</label>
              <input
                type="text"
                name="name"
                id="name"
                placeholder="NAME"
                :value="user?.cardData?.name"
                v-auto-focus
                required
              />
            </div>
            <div class="input-element">
              <label for="mm">Expiration month</label>
              <input
                type="number"
                min="1"
                max="12"
                step="1"
                name="mm"
                id="mm"
                placeholder="10"
                :value="user?.cardData?.mm"
                required
              />
            </div>
            <div class="input-element">
              <label for="yy">Expiration year</label>
              <input
                type="number"
                min="2022"
                max="2099"
                step="1"
                name="yy"
                id="yy"
                placeholder="2027"
                :value="user?.cardData?.yy"
                required
              />
            </div>
            <div class="input-element">
              <label for="cardNumber">Card number</label>
              <input
                type="text"
                pattern="([0-9]{4} [0-9]{4} [0-9]{4} [0-9]{4})"
                placeholder='1234 5678 1234 5678'
                name="cardNumber"
                id="cardNumber"
                :value="user?.cardData?.cardNumber"
                required
              />
            </div>
            <div>
              <input type="password" name="cvc" id="cvc" placeholder="CVC" required />
            </div>
          </div>
          <button type="submit" value="Save">Send</button>
        </form>
      </div>
    </div>
  </div>
</template>

<style scoped>
form {
  display: flex;
  flex-wrap: wrap;
}

.input-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

input {
  width: 250px;
  margin: 10px 10px 10px 0;
}

.input-element {
  display: flex;
  width: 250px;
  flex-wrap: wrap;
}

input[name='name'],
input[name='cardNumber'],
.input-element:nth-child(1),
.input-element:nth-child(4) {
  width: 100%;
}

input[type='password'] {
  width: 100px;
  margin-right: auto;
}
</style>
