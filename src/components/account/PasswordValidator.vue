<template>
  <div class="password-changer box-shadow">
    <form @submit.prevent>
      <label for="current">Current password</label>
      <input :type="passwordTypeold" v-model="passwordModelold" name="current">
      <div class="show-box">
        <button @click="showPasswordold">
          <i class="far fa-eye"></i> show
        </button>
      </div>
      <label for="new">new password</label>
      <input :type="passwordType" v-model="password" name="new">
      <div class="show-box">
        <button @click="showPassword">
          <i class="far fa-eye"></i> show
        </button>
      </div>
      <label for="strength">Strength
        <span v-if="!isInitial">:</span>
        <span
          v-bind:class="{initial: isInitial, short : isShort, weak: isWeak, fair : isFair, excellent : isExcellent }"
        >{{passwordStregnth}}</span>
      </label>
      <div class="rg-bar">
        <div
          v-bind:class="{ highlight: true, initial: isInitial,  bgShort : isShort, bgWeak: isWeak, bgFair : isFair, bgExcellent : isExcellent }"
        ></div>
      </div>
      <p class="support-text">Your password must:</p>
      <ul class="support-text">
        <li v-bind:class="{checked: isAtLeast8}">
          <i class="fas fa-check"></i> Be at least 8 characters long
        </li>
        <li v-bind:class="{checked: isSpecial}">
          <i class="fas fa-check"></i> Contain a special character
        </li>
        <li v-bind:class="{checked: isCapital}">
          <i class="fas fa-check"></i> Contain a capital letter
        </li>
      </ul>
      <button v-bind:class="{disabled: !isValid, btnSubmit: true}" :disabled="isValid" >Set new password</button>
    </form>
  </div>
</template>
<script>
export default {
  name: "PasswordValidator",
  data() {
    return {
      password: "",
      passwordType: "password",
      passwordModelold: "",
      passwordTypeold: "password"
    };
  },
  computed: {
    isInitial() {
      return this.password.length === 0;
    },
    isShort() {
      return this.password.length > 3 && !this.isAtLeast8;
    },
    isWeak() {
      return this.isAtLeast8 && (!this.isSpecial || !this.isCapital);
    },
    isFair() {
      return this.isAtLeast8 && this.isSpecial && this.isCapital;
    },
    isExcellent() {
      return this.password.length >= 10 && this.isSpecial && this.isCapital;
    },
    isValid() {
      return this.isFair || this.isExcellent;
    },
    isCapital() {
      const regex = /[A-Z]/g;
      return this.password.match(regex);
    },
    isAtLeast8() {
      return this.password.length >= 8;
    },
    isSpecial() {
      const regex = /[^A-Za-z0-9]/g;
      return this.password.match(regex);
    },
    passwordStregnth() {
      let msg = "";
      msg = this.isShort ? "Too short" : msg;
      msg = this.isWeak ? "Weak" : msg;
      msg = this.isFair ? "Fair" : msg;
      msg = this.isExcellent ? "Strong" : msg;
      return msg;
    }
  },
  methods: {
    showPassword() {
      this.passwordType =
        this.passwordType === "password" ? "text" : "password";
    },
    showPasswordold() {
      this.passwordTypeold =
        this.passwordTypeold === "password" ? "text" : "password";
    }
  }
};
</script>
