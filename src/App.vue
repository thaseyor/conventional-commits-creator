<template>
  <div id="app">
    <section style="width:40vw;">
      <b-field>
        <b-checkbox v-model="major">Breaking change</b-checkbox>
        <b-checkbox v-model="scopeOption">Add scope</b-checkbox>
      </b-field>
      <b-field>
        <div
          class="block"
          @click="isModalActive = !isModalActive"
          style="position:absolute;"
        >
          <b-icon
            pack="fas"
            icon="question-circle"
            style="position:absolute;right:0px;padding-top:10px;cursor:pointer"
            size="is-medium"
          >
          </b-icon>
        </div>
        <b-select placeholder="type" v-model="type">
          <option v-for="type in $options.TYPES_LIST" :value="type" :key="type">
            {{ major ? `${type}!` : type + String.fromCharCode(160) }}
          </option>
        </b-select>
        <div class="columns is-gapless" style="width:100%">
          <b-input
            v-if="scopeOption"
            v-model="scope"
            placeholder="scope"
            class="column"
            maxlength="10"
          >
          </b-input>

          <b-input
            placeholder="description"
            v-model="description"
            :class="{ 'is-three-quarters': scopeOption }"
            class="column"
            maxlength="100"
          >
          </b-input>
        </div>
        <div class="block" @click="copyHeader">
          <b-icon
            pack="fas"
            icon="clipboard"
            size="is-medium"
            style="position:relative; top:4px;left:2px; cursor:pointer"
          >
          </b-icon>
        </div>
      </b-field>
      <b-field>
        <b-input type="textarea" placeholder="body/footer"></b-input>
      </b-field>
    </section>
    <b-modal v-model="isModalActive">
      <b-table :data="data" :columns="columns" :show-header="false"></b-table>
    </b-modal>
  </div>
</template>

<script>
import { SnackbarProgrammatic as Snackbar } from "buefy";

export default {
  name: "App",
  data() {
    return {
      type: "feat",
      major: false,
      scopeOption: false,
      isModalActive: false,
      scope: "",
      description: "",
      data: [
        {
          type: "build",
          desc: "Build the project or change external dependencies",
        },
        {
          type: "chore",
          desc: "Miscellaneous commits e.g. modifying .gitignore",
        },
        {
          type: "ci",
          desc: "Setting up CI and working with scripts",
        },
        {
          type: "docs",
          desc: "Documentation update",
        },
        {
          type: "feat",
          desc: "Adding new functionality",
        },
        {
          type: "fix",
          desc: "Bug fixes",
        },
        {
          type: "perf",
          desc: "Changes to improve performance",
        },
        {
          type: "refactor",
          desc: "Code changes without fixing bugs or adding new features",
        },
        {
          type: "revert",
          desc: "Rollback to previous commits",
        },
        {
          type: "style",
          desc: "Codestyle edits (tabs, indents, periods, commas, etc.)",
        },
        {
          type: "test",
          desc: "Changes to tests",
        },
      ],
      columns: [
        {
          field: "type",
          label: "Type",
        },
        {
          field: "desc",
          label: "Description",
        },
      ],
    };
  },
  TYPES_LIST: [
    "build",
    "chore",
    "ci",
    "docs",
    "feat",
    "fix",
    "perf",
    "refactor",
    "revert",
    "style",
    "test",
  ],
  methods: {
    validate() {},

    copyHeader() {
      let text = this.type;
      if (this.major) text += "!";
      if (this.scopeOption && this.scope) text += `(${this.scope})`;
      text += `: ${this.description}`;

      navigator.clipboard.writeText(text).then(
        function() {
          console.log(text);
          Snackbar.open({
            duration: 5000,
            message: "Header has been copied!",
            type: "is-success",
            position: "is-bottom-right",
            queue: false,
          });
        },
        function(err) {
          console.error("Async: Could not copy text: ", err);
        }
      );
    },
  },
};
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
  overflow-y: hidden !important;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  display: flex;
  height: 100vh;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
</style>
