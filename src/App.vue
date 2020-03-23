<template>
  <div id="app">
    <header>
      <nav>
        <a v-for="item in Object.keys(items)" @click.prevent="activeItem = item" :class="{active: activeItem === item}">{{ items[item].link }}</a>
      </nav>
    </header>
    <h1>{{ items[activeItem].title }}</h1>
    <list-mirror-initial v-if="activeItem === 'initial'" />
    <list-mirror-initial-and-unrelated v-else-if="activeItem === 'initialunrelated'" />
    <list-mirror-halt v-else-if="activeItem === 'halt'" />
    <list-mirror-halt-style v-else-if="activeItem === 'haltstyle'" />
    <list-mirror-halt-style-clone v-else-if="activeItem === 'haltstyleclone'" />
    <list-mirror-synced v-else-if="activeItem === 'synced'" />
  </div>
</template>

<script lang="ts">
import ListMirrorInitial from './components/ListMirrorInitial.vue';
import ListMirrorSynced from './components/ListMirrorSynced.vue';
import ListMirrorHalt from './components/ListMirrorHalt.vue';
import ListMirrorHaltStyle from './components/ListMirrorHaltStyle.vue';
import ListMirrorHaltStyleClone from './components/ListMirrorHaltStyleClone.vue';
import ListMirrorInitialAndUnrelated from './components/ListMirrorInitialAndUnrelated.vue';

export default {
    data: function() {
        return {
            activeItem: 'initial',
            items: {
                initial: { title: 'Mirrored List: Initial position', link: 'Initial' },
                initialunrelated: { title: 'Mirror and Different List: Initial position', link: 'Initial/Unrelated' },
                halt: { title: 'Lists: Don\'t allow moving between mirrored lists', link: 'Halt' },
                haltstyle: { title: 'Lists: Styling disabled mirror list', link: 'Halt/Styling' },
                haltstyleclone: { title: 'Lists: Styling disabled mirror list + clone', link: 'Halt/Styling/Clone' },
                synced: { title: 'Mirrored List: Lists stay in sync', link: 'Synced' }
            }
        };
    },
    components: { ListMirrorInitialAndUnrelated, ListMirrorInitial, ListMirrorHalt, ListMirrorHaltStyle, ListMirrorHaltStyleClone, ListMirrorSynced }
};
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    outline-width: thin;
  }

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    height: 100vh;
    display: flex;
    flex-direction: column;
  }

  header {
    min-height: 60px;
    background-color: hsl(210, 30%, 55%);
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    border-bottom: 1px solid hsl(210, 40%, 70%);
  }

  nav {
    display: flex;
    flex-wrap: wrap;
    width: 960px;
    justify-content: space-evenly;
  }

  nav a {
    display: inline-block;
    width: 150px;
    border: 1px solid rgba(255, 255, 255, 0.4);
    text-align: center;
    padding: 4px;
    -moz-user-select: -moz-none;
    -khtml-user-select: none;
    -webkit-user-select: none;
    -o-user-select: none;
    user-select: none;
    cursor: pointer;
    transition: background-color 200ms;
  }

  nav a.active {
    background-color: hsl(210, 45%, 40%);;
  }

  nav a:hover {
    background-color: hsl(210, 45%, 45%);;
  }

  h1 {
    background: transparent;
    text-align: center;
  }
  h1, .main {
    background: hsl(210, 15%, 80%);
  }
  h1 {
    padding: 60px 0 20px 0;
    color: #666;
  }

  @media only screen and (max-width: 800px) {
    header {
      min-height: 100px;
    }
    nav {
      justify-content: center;
    }
    nav a {
      margin: 6px;
    }
    h1 {
      font-size: 1.3em;
    }
  }
  @media only screen and (max-width: 600px) {
    header {
      min-height: 150px;
    }
    h1 {
      padding: 40px 0 20px 0;
    }
  }
</style>
