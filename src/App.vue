<template>
  <div id="app">
    <h1>CodeMirror with Vue.js</h1>
    <CodeMirrorEditor :code="code" @run-code="executeCode" />
    <div class="output">
      <h2>Output:</h2>
      <pre>{{ output }}</pre>
    </div>
  </div>
</template>

<script>
  import CodeMirrorEditor from './components/CodeMirrorEditor.vue';

  export default {
    name: 'App',
    components: {
      CodeMirrorEditor
    },
    data() {
      return {
        code: 'console.log("Hello, world!");',
        output: ''
      };
    },
    methods: {
      executeCode(code) {
        try {
          const log = [];
          const consoleLog = console.log;
          console.log = (...args) => log.push(args.join(' '));

          new Function(code)();

          console.log = consoleLog;
          this.output = log.join('\n');
        } catch (error) {
          this.output = `Error: ${error.message}`;
        }
      }
    }
  };
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

  .output {
    margin-top: 20px;
    border: 1px solid #ddd;
    padding: 10px;
    background-color: #f9f9f9;
  }
</style>
