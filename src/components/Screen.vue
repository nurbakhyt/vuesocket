<template>
  <div
    class="hello"
    :class="{'hello--medved': state === 0}"
    @click.stop="handleClick"
  >
    <img alt="Vue logo" src="../assets/logo.png" width="250">
    <span
      class="label"
      :class="{'label--medved': state === 0}"
    >{{ statuses[state] }}</span>
    <h2>Нажмите на экран</h2>
  </div>
</template>

<script>
export default {
  name: 'Screen',
  data() {
    return {
      state: 0,
      ws: null,
      statuses: ['MEDVED', 'PREVED']
    };
  },
  mounted() {
    this.ws = new WebSocket('ws://pm.tada.team/ws');
    this.ws.onmessage = this.onMessage;
    document.title = this.statuses[this.state];
  },
  methods: {
    handleClick() {
      this.state = this.state > 0 ? 0 : 1;
      document.title = this.statuses[this.state];
      this.ws.send(JSON.stringify({"state": this.state}));
    },
    onMessage(e) {
      this.state = JSON.parse(e.data).state;
      document.title = this.statuses[this.state];
    }
  }
}
</script>

<style scoped>
  .hello {
    background-color: #41B883;
    display: flex;
    cursor: pointer;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100%;
    width: 100%;
  }

  .hello--medved {
    background-color: #35495E;
  }

  .label {
    background-color: #35495E;
    color: #fff;
    border: none;
    border-radius: 1em;
    outline: none;
    padding: .5em 1em;
  }

  .label--medved {
    background-color: #41B883;
  }

  h2 {
    color: #ccc;
    font-weight: 300;
    margin-top: 1em;
    margin-bottom: 1em;
  }
</style>
