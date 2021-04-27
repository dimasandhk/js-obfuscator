<template>
  <div class="home mt-5">
    <div class="container">
      <h4 class="text-center">Javascript Obfuscator</h4>
      <div class="input-code mt-4">
        <div class="row justify-content-center">
          <div class="col-12 col-md-12 col-lg-6">
            <div class="input-group mb-3">
              <input
                type="text"
                v-model="value"
                class="form-control shadow-none"
              />
              <div class="input-group-append">
                <button
                  class="btn btn-dark shadow-none"
                  @click="obfuscateCode"
                  type="button"
                >
                  Obfuscate
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Obfuscator from "javascript-obfuscator";

export default {
  name: "Home",
  data: () => ({
    value: "",
  }),
  methods: {
    obfuscateCode() {
      const result = Obfuscator.obfuscate(this.value);
      console.log(result.getObfuscatedCode());
    },
  },
};
</script>

<style lang="scss" scoped>
h4 {
  font-weight: 600;
}
</style>
