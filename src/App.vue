<template>
  <div id="app">
    <section class="hero is-dark">
      <div class="hero-body">
        <div class="container">
          <h1 class="title is-marginless">Pixela Dashboard</h1>

          <div class="level is-marginless">
            <div class="level-left">
              <div class="level-item">
                <h2 class="subtitle is-marginless">&ensp;ふくすうの <a 
                  href="https://pixe.la/" 
                  class="has-text-primary"><u>Pixela</u></a> グラフを いちがめんで かんり</h2>
              </div>
            </div>
            <div class="level-right">
              <div class="level-item">
                <div class="field">
                  <input 
                    v-model="shortMode"
                    id="switchExample" 
                    type="checkbox" 
                    name="switchExample" 
                    class="switch is-rtl is-small is-outlined">
                  <label 
                    for="switchExample"
                    class="is-small">Short mode {{ shortMode ? 'ON' : 'OFF' }}</label>
                </div>
              </div>
            </div>
          </div>
          
          <div class="level">
            <div class="level-left"/>
            <div class="level-right">
              <div class="level-item">
                <label class="label is-small has-text-white is-marginless">USER-ID&ensp;&ensp;</label>
                <label class="label is-small has-text-white is-marginless">{{ user }}</label>
              </div>
              <label class="label is-small has-text-white is-marginless">/&ensp;</label>
              <div class="level-item">
                <label class="label is-small has-text-white is-marginless">USER-TOKEN&ensp;&ensp;</label>
                <div class="control">
                  <input 
                    v-model="token" 
                    type="text" 
                    class="input is-small">
                </div>
              </div>
            </div>
          </div>

        </div>
      </div>
    </section>

    <div class="columns">
      <div class="column"/>
    </div>

    <div
      v-if="!shortMode"
      class="container">
      <div class="columns">
        <div class="column">
          <Pixela
            v-for="(g, key, index) in graphs"
            :key="index"
            :user= "user"
            :token= "token"
            :graph-name="g" />
        </div>
      </div>
    </div>

    <div
      v-if="shortMode"
      class="container">
      <div class="columns is-centered is-multiline">
        <PixelaShortMode
          v-for="(g, key, index) in graphs"
          :key="index"
          :user= "user"
          :token= "token"
          :graph-name="g" />
      </div>
    </div>

  </div>
</template>

<script>
import Pixela from './components/Pixela.vue'
import PixelaShortMode from './components/PixelaShortMode.vue'
import config from './pixela-config.json'

export default {
  name: 'App',
  components: {
    Pixela,
    PixelaShortMode
  },
  data() {
    return {
      user: config.user,
      token: '',
      graphs: config.graphs,
      shortMode: false
    }
  }
}
</script>

<style lang="scss">
// Bulma
@import "../node_modules/bulma/bulma.sass";
@import '../node_modules/bulma-extensions/bulma-switch/dist/css/bulma-switch.min.css';

// Nu font
@font-face {
  font-family: "Nu-Ankomochi";
  //src: url("~@/assets/fonts/NuAnkoMochi-Square-2.otf") format("otf");
  src: url("~@/assets/fonts/NuAnkoMochi-Square-2.otf");
}

#app {
  // font-family: 'Avenir', Helvetica, Arial, sans-serif;
  font-family: 'Nu-Ankomochi';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  //background-color: #212529;
  margin-top: 20px;
}

h1, h2 {
  font-family: 'Nu-Ankomochi';
}

</style>
