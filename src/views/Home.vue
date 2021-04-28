<template>
  <container>
    <div class="title-page text-center">
      <h4>Javascript Obfuscator</h4>
      <h6>
        Make your code harder to copy and prevent people from stealing your
        work.
      </h6>
      <a @click="showMeaning" class="text-muted"
        >What is obfuscation in programming?</a
      >
    </div>

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
    showMeaning(e) {
      e.preventDefault();
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
  },
};
</script>

<style lang="scss">
h4 {
  font-weight: 600;
}

.swal-modal {
  .swal-icon--info {
    &::before,
    &::after {
      background-color: #a5dc86;
    }
    color: rgba(57, 57, 64, 0.95);
    border: 4px solid #a5dc86;
  }
  .swal-title,
  .swal-text {
    color: #fff;
  }
  .swal-button {
    border: none;
    &:hover {
      color: #fff;
      background-color: #4d4d53 !important;
    }
    background-color: #4d4d53;
    color: #fff;
    font-weight: 600;
  }
  background-color: rgba(57, 57, 64, 0.95);
}

.title-page {
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
