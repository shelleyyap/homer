<template>
  <div v-cloak v-if="links" class="container-fluid">
    <nav class="navbar" role="navigation" aria-label="main navigation">
      <div class="container">
        <div class="navbar-brand">
          <a
            role="button"
            aria-label="menu"
            aria-expanded="false"
            class="navbar-burger"
            :class="{ 'is-active': showMenu }"
            v-on:click="$emit('navbar-toggle')"
          >
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
          </a>
        </div>
        <div class="navbar-menu" :class="{ 'is-active': showMenu }">
          <div class="navbar-start">
            <a
              :class='{"navbar-selected": (key == selectedIndex), "navbar-item": true}'
              rel="noreferrer"
              v-for="(link, key) in links"
              @click="selectTab(key)"
              :key="key"
              :href="link.url"
              :target="link.target"
            >
              <i
                v-if="link.icon"
                :class="['fa-fw', link.icon, { 'mr-2': link.name }]"
              ></i>
              {{ link.name }}
            </a>
          </div>
          <div class="navbar-end">
            <slot></slot>
          </div>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  name: "Navbar",
  props: {
    open: {
      type: Boolean,
      default: false,
    },
    links: Array,
  },
  computed: {
    showMenu: function () {
      return this.open && this.isSmallScreen();
    },
  },
  mounted: function () {
    this.selectTab(0);
  },
  data () {
    return {
      selectedIndex: 0, // the index of the selected tab,
    }
  },
  methods: {
    isSmallScreen: function () {
      return window.matchMedia("screen and (max-width: 1023px)").matches;
    },
    selectTab: function (i) {
      this.selectedIndex = i;
      // loop over all the tabs
      this.links.forEach((tab, index) => {
        tab.isActive = index === i;
      });
    },
  },
};
</script>
