<template>
  <div id="app">
    <header>
      <nav>
        <a @click.prevent="item = 'initial'" :class="{ active: item === 'initial' }">Initial</a>
        <a @click.prevent="item = 'synced'" :class="{ active: item === 'synced' }">Synced</a>
      </nav>
    </header>
    <h1 v-if="item === 'initial'">Mirrored List: Initial position</h1>
    <h1 v-else-if="item === 'synced'">Mirrored List: Lists stay in sync</h1>
    <list-mirror-initial v-if="item === 'initial'" />
    <list-mirror-synced v-else-if="item === 'synced'" />
  </div>
</template>

<script lang="ts">
import ListMirrorInitial from './components/ListMirrorInitial.vue';
import ListMirrorSynced from './components/ListMirrorSynced.vue';

export default {
    data: function() {
        return {
            item: 'initial'
        };
    },
    components: { ListMirrorInitial, ListMirrorSynced }
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

  nav { display: flex;
    width: 300px;
    justify-content: space-evenly;
  }

  nav a {
    display: inline-block;
    width: 120px;
    border: 1px solid rgba(255, 255, 255, 0.4);
    text-align: center;
    padding: 2px 0;
    -moz-user-select: -moz-none;
    -khtml-user-select: none;
    -webkit-user-select: none;
    -o-user-select: none;
    user-select: none;
    cursor: pointer;
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
