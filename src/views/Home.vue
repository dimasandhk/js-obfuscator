<template>
  <container>
    <PageTitle @def-clicked="showMeaning" />

    <input-container>
      <textarea
        spellcheck="false"
        class="form-control shadow-none"
        cols="35"
        rows="10"
        v-model="value"
      ></textarea>
      <button
        @click="obfuscateCode"
        class="btn btn-block btn-dark shadow-none mt-3"
      >
        Obfuscate
      </button>
    </input-container>

    <result-container>
      <div class="result-container" v-show="result !== ''">
        <div class="container">
          <h3>
            Result :
            <button @click="copyResult" class="btn btn-dark shadow-none">
              Copy
            </button>
          </h3>
          <p>
            {{ result }}
          </p>
        </div>
      </div>
    </result-container>
  </container>
</template>

<script>
// Modules
import copy from "clipboard-copy";
import swal from "sweetalert";
import format from "js-beautify";
import Obfuscator from "javascript-obfuscator";

// Components
import InputContainer from "../components/InputContainer.vue";
import HomeContainer from "../components/HomeContainer.vue";
import ResultContainer from "../components/ResultContainer.vue";
import PageTitle from "../components/PageTitle.vue";

export default {
  name: "Home",
  data: () => ({
    value: "",
    result: "",
  }),
  methods: {
    obfuscateCode() {
      try {
        const result = Obfuscator.obfuscate(this.value);
        this.result = format(result.getObfuscatedCode().replace(/\s/g, " "));
      } catch (err) {
        this.result = "";
        swal("Error", `${err}`, "error");
      }
    },
    copyResult() {
      copy(this.result);
      swal("Copied!", "Copied Obfuscated JS Code", "info", {
        timer: 1000,
      });
    },
    showMeaning() {
      swal(
        "What is obfuscation?",
        "Code obfuscation is the technique of making the source code of an application difficult to read and comprehend so it becomes almost impossible for any unauthorized third-party group or individual, using any available tools, to reverse engineer it",
        "info"
      );
    },
  },
  components: {
    "input-container": InputContainer,
    container: HomeContainer,
    "result-container": ResultContainer,
    PageTitle,
  },
};
</script>

<style lang="scss">
h4 {
  font-weight: 600;
}

.page-title {
  a {
    &:hover {
      color: #fff !important;
    }
    cursor: pointer;
    transition: 0.5s;
  }
}

.result-container {
  padding: 10px;
  border-radius: 7px;
  border: 1px solid #fff;
}
</style>
