<template>
  <div>
    <div class="menu-container main-column-limit">
      <nav class="desktop-menu">
        <div class="left">
          <NuxtLink to="/">Home</NuxtLink>
          <NuxtLink v-if="auth" to="/my-stories">My stories</NuxtLink>
          <NuxtLink to="/new-story">New story</NuxtLink>
        </div>
        <div class="right">
          <NuxtLink v-if="!auth" to="/sign-in">Sign in</NuxtLink>
          <a v-if="auth" href="#" @click.prevent="logout">Logout</a>
        </div>
      </nav>
      <button class="mobile-menu-button btn-transparent" @click="toggleMenu">
        <font-awesome-icon icon="bars" class="icon" />
      </button>
    </div>
    <div v-show="isExpanded" class="mobile-menu">
      <div class="overlay">
        <nav>
          <div class="main-column-limit">
            <button
              class="mobile-menu-button btn-transparent"
              @click="toggleMenu"
            >
              <font-awesome-icon icon="times" class="icon" />
            </button>

            <NuxtLink to="/">Home</NuxtLink>
            <NuxtLink v-if="auth" to="/my-stories">My stories</NuxtLink>
            <NuxtLink to="/new-story">New story</NuxtLink>
            <NuxtLink v-if="!auth" to="/sign-in" class="pill">Sign in</NuxtLink>
            <a v-if="auth" href="#" class="pill" @click.prevent="logout">
              Logout
            </a>
          </div>
        </nav>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isExpanded: false,
    }
  },
  computed: {
    auth() {
      return this.$store.state.auth
    },
  },
  watch: {
    $route() {
      this.isExpanded = false
    },
  },
  methods: {
    toggleMenu() {
      this.isExpanded = !this.isExpanded
    },
    logout() {
      this.$store.dispatch('cleanAuth')
    },
  },
}
</script>

<style lang="scss">
@use '@/assets/css/vars' as vars;

.menu-container {
  display: flex;
  flex-direction: row-reverse;
  margin-top: 20px;
}
.mobile-menu-button {
  font-size: 24px;

  .icon > * {
    color: vars.$black;
  }
}
.desktop-menu {
  display: none;

  a {
    font-weight: bold;
    padding-bottom: 5px;

    &.active-link {
      border-bottom: 3px solid vars.$white;
    }
  }
}
.mobile-menu {
  position: absolute;
  top: 0;
  width: 100%;
  z-index: 10;

  .overlay {
    background-color: rgba(0, 0, 0, 0.4);
    height: 100%;
    position: fixed;
    width: 100%;
    top: 0;
    bottom: 0;
  }

  nav {
    background-color: white;
    border-radius: 0 0 vars.$border-radius vars.$border-radius;
    padding: 20px 0;
    text-align: right;

    a {
      display: block;
      margin-bottom: 30px;
      color: vars.$dark-green;
      text-align: center;
    }

    a:last-of-type {
      margin-bottom: 0px;
    }

    .pill {
      background-color: vars.$dark-green;
      color: vars.$white;
      padding: 10px;
      border-radius: vars.$border-radius;
    }
  }
}

/*----------------------------------------------
  MEDIA QUERIES
-----------------------------------------------*/
@media screen and (min-width: 480px) {
  .desktop-menu {
    display: flex;
    justify-content: space-between;
    width: 100%;

    .right a:not(:last-of-type),
    .left a:not(:last-of-type) {
      margin-right: 20px;
    }
  }
  .menu-container .mobile-menu-button,
  .mobile-menu {
    display: none;
  }
}
</style>
