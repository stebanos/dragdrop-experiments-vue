<template>
  <div id="app">
    <header>
      <nav>
        <a @click.prevent="item = 'initial'" :class="{ active: item === 'initial' }">Initial</a>
        <a @click.prevent="item = 'initialunrelated'" :class="{ active: item === 'initialunrelated' }">Initial / Unrelated</a>
        <a @click.prevent="item = 'halt'" :class="{ active: item === 'halt' }">Halt</a>
        <a @click.prevent="item = 'haltstyle'" :class="{ active: item === 'haltstyle' }">Halt / Styling</a>
        <a @click.prevent="item = 'synced'" :class="{ active: item === 'synced' }">Synced</a>
      </nav>
    </header>
    <h1>{{ titles[item] }}</h1>
    <list-mirror-initial v-if="item === 'initial'" />
    <list-mirror-initial-and-unrelated v-else-if="item === 'initialunrelated'" />
    <list-mirror-halt v-else-if="item === 'halt'" />
    <list-mirror-halt-style v-else-if="item === 'haltstyle'" />
    <list-mirror-synced v-else-if="item === 'synced'" />
  </div>
</template>

<script lang="ts">
import ListMirrorInitial from './components/ListMirrorInitial.vue';
import ListMirrorSynced from './components/ListMirrorSynced.vue';
import ListMirrorHalt from './components/ListMirrorHalt.vue';
import ListMirrorHaltStyle from './components/ListMirrorHaltStyle.vue';
import ListMirrorInitialAndUnrelated from './components/ListMirrorInitialAndUnrelated.vue';

export default {
    data: function() {
        return {
            item: 'initial',
            titles: {
                initial: 'Mirrored List: Initial position',
                initialunrelated: 'Mirror and Different List: Initial position',
                halt: 'Lists: Don\'t allow moving between mirrored lists',
                haltstyle: 'Lists: Styling disabled mirror list',
                synced: 'Mirrored List: Lists stay in sync'
            }
        };
    },
    components: { ListMirrorInitialAndUnrelated, ListMirrorInitial, ListMirrorHalt, ListMirrorHaltStyle, ListMirrorSynced }
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
    height: 80px;
    background-color: hsl(210, 30%, 55%);
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    border-bottom: 1px solid hsl(210, 40%, 70%);
  }

  nav {
    display: flex;
    width: 810px;
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
</style>
