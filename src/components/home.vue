<template>
  <div class="landing-page">
    <header class="header">
        <!-- Logo Section -->
        <h1 v-show="!isSticky" class="bank-title">IE Bank</h1>
      <div class="logo" :class="{ sticky: isSticky }" ref="logo">
        <img src="@/assets/Logo1.png" alt="IE Bank Logo" class="logo-img" />
      </div>
      <p class="slogan">"Your Future, Our Priority"</p>
      <p>
        At IE Bank, we provide innovative banking solutions tailored to meet your needs.
        Secure your financial future with us today!
      </p>
    </header>

    <section class="features">
      <div
        v-for="(feature, index) in features"
        :key="index"
        :class="['feature-item', index % 2 === 0 ? 'left' : 'right', { animate: isVisible[index] }]"
        ref="featureItems"
        :data-index="index"
      >
        <div class="feature-image">
          <img
            :data-src="feature.image"
            :alt="feature.title"
            class="lazyload"
          />
        </div>
        <div class="feature-text">
          <h2>{{ feature.title }}</h2>
          <p>{{ feature.description }}</p>
        </div>
      </div>
    </section>

    <section class="content">
        <h2>
            Secure your future now!
        </h2>
      <button @click="goToAccounts" class="cta-button">Manage Your Account</button>
    </section>

    
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      isSticky: false,
      logoHeight: 0, // To store the height of the logo
      features: [
        {
          title: "Secure Online Banking",
          description:
            "Experience the safest online banking with advanced security protocols protecting your assets.",
          image: require("@/assets/Logo1.png"),
        },
        {
          title: "24/7 Customer Support",
          description:
            "Our dedicated support team is available around the clock to assist you with your needs.",
          image: require("@/assets/support.webp"),
        },
        {
          title: "Flexible Loan Options",
          description:
            "We offer a variety of loan products with competitive rates to help you achieve your dreams.",
          image: require("@/assets/loans.jpg"),
        },
        {
          title: "Investment Opportunities",
          description:
            "Grow your wealth with our tailored investment solutions and expert advice.",
          image: require("@/assets/investment.jpg"),
        },
      ],
      isVisible: [],
      observer: null,
    };
  },
  methods: {
    goToAccounts() {
      this.$router.push("/accounts");
    },
    handleScroll() {
      this.isSticky = window.scrollY > 0;
    },
    initObserver() {
      const options = {
        root: null,
        rootMargin: "0px",
        threshold: 0.1,
      };

      this.observer = new IntersectionObserver(this.onIntersection, options);

      this.$refs.featureItems.forEach((el) => {
        if (el) {
          this.observer.observe(el);
        }
      });
    },
    onIntersection(entries) {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          const index = parseInt(entry.target.dataset.index, 10);
          this.$set(this.isVisible, index, true);
          this.observer.unobserve(entry.target);
        }
      });
    },
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll);
    // Initialize visibility array
    this.isVisible = new Array(this.features.length).fill(false);
    // Initialize Intersection Observer
    this.initObserver();
  },
  beforeDestroy() {
    window.removeEventListener("scroll", this.handleScroll);
    if (this.observer) {
      this.observer.disconnect();
    }
  },
};
</script>

<style scoped>
.landing-page {
  text-align: center;
  background: linear-gradient(to bottom right, #e0eafc, #cfdef3);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-bottom: 50px;
}

.logo-img {
  width: 150px; /* Adjust the size as needed */
  height: auto;
  transition: all 0.3s ease;
}

.header.sticky .logo-img {
  width: 100px; /* Reduce size when sticky */
}

.header {
  position: relative;
  padding: 50px;
  width: 100%;
  background: transparent;
}

.logo {
  transition: all 0.3s ease;
}

.logo.sticky {
  position: fixed;
  top: 0;
  left: 50%;
  transform: translateX(-50%) translateY(0); /* Slide in from top */
  opacity: 1;
  background-color: rgba(224, 234, 252, 0.9);
  padding: 10px 20px;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  z-index: 1000;
}

.logo h1 {
  font-size: 2em; /* Adjusted for sticky state */
  color: #2c3e50;
}

.header h1 {
  font-size: 4em;
  color: #2c3e50;
  margin-bottom: 0;
}

.slogan {
  font-size: 1.8em;
  color: #34495e;
  margin-top: 10px;
  font-style: italic;
}

.content {
  margin-top: 100px;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
  padding: 0 20px;
}

.content p {
  font-size: 1.2em;
  color: #2c3e50;
  line-height: 1.5em;
}

.cta-button {
  margin-top: 30px;
  padding: 15px 30px;
  font-size: 1.2em;
  background-color: #27ae60;
  color: white;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.cta-button:hover {
  background-color: #2ecc71;
  transform: translateY(-3px);
}

.cta-button:active {
  transform: translateY(1px);
}

/* Features Section */
.features {
  width: 100%;
  margin-top: 60px;
}

.feature-item {
  display: flex;
  align-items: center;
  padding: 50px;
  max-width: 1200px;
  margin: 0 auto;
  opacity: 0;
  transform: translateY(50px);
  transition: all 0.6s ease-out;
}

.feature-item.animate {
  opacity: 1;
  transform: translateY(0);
}

.feature-item:not(:last-child) {
  margin-bottom: 50px;
}

.feature-item.left .feature-image {
  order: 1;
}

.feature-item.left .feature-text {
  order: 2;
  text-align: left;
}

.feature-item.right {
  flex-direction: row-reverse;
}

.feature-item.right .feature-text {
  text-align: left;
}

.feature-image img {
  width: 500px;
  height: auto;
  border-radius: 10px;
  object-fit: cover;
  display: block;
  margin: 0 auto;
}

.feature-text {
  flex: 1;
  padding: 0 30px;
}

.feature-text h2 {
  font-size: 2em;
  color: #2c3e50;
}

.feature-text p {
  font-size: 1.2em;
  color: #34495e;
  margin-top: 20px;
  line-height: 1.6em;
}

/* Lazy Load Placeholder */
.lazyload {
  opacity: 0;
  transition: opacity 0.5s ease-in;
}

.lazyload-loaded {
  opacity: 1;
}

/* Responsive Styles */
@media (max-width: 768px) {
  .feature-item {
    flex-direction: column;
    padding: 20px;
  }

  .feature-item.right {
    flex-direction: column;
  }

  .feature-image img {
    width: 100%;
    margin-bottom: 20px;
  }

  .feature-text {
    padding: 0;
    text-align: center;
  }
}
</style>
