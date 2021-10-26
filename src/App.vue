<template>
  <div id="app">
    <section>
      <b-field>
        <b-checkbox v-model="major">Breaking change</b-checkbox>
        <b-checkbox v-model="scopeOption">Add scope</b-checkbox>
      </b-field>
      <b-field>
        <div class="control">
          <b-button icon-left="help" @click="isModalActive = !isModalActive" />
        </div>
        <b-select placeholder="type" v-model="type">
          <option v-for="type in $options.TYPES_LIST" :value="type" :key="type">
            {{ major ? `${type}!` : type + String.fromCharCode(160) }}
          </option>
        </b-select>
        <b-input
          v-if="scopeOption"
          v-model="scope"
          placeholder="scope"
          maxlength="10"
        >
        </b-input>
        <b-input
          placeholder="description"
          v-model="description"
          maxlength="100"
        >
        </b-input>
        <p class="control">
          <b-button
            type="is-transparent"
            icon-right="clipboard"
            @click="copyHeader"
          />
        </p>
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
