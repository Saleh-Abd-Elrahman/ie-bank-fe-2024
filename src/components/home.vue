<template>
  <div class="landing-page">
    <header class="header">
      <h1>Welcome to IE Bank</h1>
      <p class="slogan">"Your Future, Our Priority"</p>
    </header>

    <section class="content">
      <p>
        At IE Bank, we provide innovative banking solutions tailored to meet your needs. 
        Secure your financial future with us today!
      </p>
      <button @click="goToAccounts" class="cta-button">Manage Your Account</button>
    </section>
  </div>
</template>

<script>
export default {
  name: "Home",
  methods: {
    goToAccounts() {
      this.$router.push("/accounts");
    }
  }
};
</script>

<style scoped>
.landing-page {
  text-align: center;
  padding: 50px;
  background-color: #f5f7fa;
}

.header h1 {
  font-size: 3em;
  color: #2c3e50;
}

.slogan {
  font-size: 1.5em;
  color: #34495e;
  margin-top: 10px;
}

.content {
  margin-top: 30px;
}

.cta-button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 1.2em;
  background-color: #3498db;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.cta-button:hover {
  background-color: #2980b9;
}
</style>
