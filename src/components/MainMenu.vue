<template>
  <div class="menu-wrapper">
    <button class="menu-toggle" @click="toggleMenu">{{ activeCategory == 'all'? 'Menu' : activeCategory }}</button>
    <div class="menu" :class="{ 'menu-open': showMenu }">
      <button 
        :key="'all'" 
        @click="pick('all')"
        :class="{ active: activeCategory === 'all' }"
      >
        All
      </button>
      <button 
        v-for="(category, index) in categories" 
        :key="index" 
        @click="pick(category)"
        :class="{ active: activeCategory === category }"
      >
        {{ category }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    categories: {
      type: Array,
      required: true
    }
  },

  data() {
    return {
      activeCategory: 'all',
      showMenu: false,
    }
  },

  methods: {
    pick(category) {
      this.activeCategory = category;
      this.$emit('pick-category', category);
    },
    toggleMenu(event) {
      this.showMenu = !this.showMenu;
      event.stopPropagation();
    },
    closeMenu() {
      if (this.showMenu) {
        this.showMenu = false;
      }
    }
  },

  mounted() {
    document.body.addEventListener('click', this.closeMenu);
  },

  beforeUnmount() {
    document.body.removeEventListener('click', this.closeMenu);
  }
}
</script>

<style scoped>

    .menu-wrapper {
      position: relative;
      display: flex;
      justify-content: flex-end;
      align-items: center;
    }

    .menu-toggle {
      display: none;
      margin: 0 10px;
      padding: 6px 10px;
      background-color: #2b7efa;
      color: #fff;
      border: none;
      border-radius: 5px;
      font-size: 16px;
      cursor: pointer;
    }

    .active{
      background-color: #1e4681;
    }

    button:hover{
      background-color: #2a5faf;
    }
    
    button {
      margin: 9px 6px;
      padding: 6px 10px;
      background-color: #2b7efa;
      color: #fff;
      border: none;
      border-radius: 5px;
      font-size: 16px;
      cursor: pointer;
    }

    @media screen and (max-width: 1200px) {
      button {
        margin: 6px 4px;
        padding: 6px 6px;
      }
    }

    @media screen and (max-width: 1024px) {
      button {
        font-size: 14px;
        border-radius: 3px;
      }
    }

    @media screen and (max-width: 850px) {
      button {
        margin: 3px;
        padding: 3px;
        font-size: 12px;
        border-radius: 0;
      }
    }

    @media screen and (max-width: 750px) {
      .menu-toggle {
        display: block;
        width:100%;
      }
      
      .menu {
        position: absolute;
        top: 50px;
        right: 0;
        display: none;
        flex-direction: column;
        justify-content: center;
        align-items: flex-start;
        background-color: #fff;
        border: 1px solid #ccc;
        border-radius: 5px;
        box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.3);
        padding: 10px;
        z-index: 9999;
        width:100%;
        max-width: calc(100% - 30px);
      }
      
      .menu-open {
        display: flex;
      }
      
      .menu button {
        margin: 5px 0;
        padding: 6px 10px;
        border-radius: 5px;
        font-size: 16px;
        cursor: pointer;
        width: 100%;
      }
    }
</style>
  