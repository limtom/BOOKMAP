<template>
  <div>
    <form action="">
      <div class="container">
        <div class="row d-flex flex-column no-gutters">
          <div class="col col-true">
            <div
              class="
                v-input v-input--dense
                theme--light
                v-text-field
                v-text-field--is-booted
                v-text-field--enclosed
                v-text-field--outlined
                ma-0
              "
            >
              <div class="v-input__control">
                <div class="v-input__slot">
                  <fieldset aria-hidden="true">
                    <legend style="width: 0px"></legend>
                  </fieldset>
                  <div class="v-text-field__slot">
                    <input placeholder="" type="" />
                  </div>
                </div>
                <div class="v-text-field__details">
                  <div class="v-messages theme--light">
                    <div class="v-messages__wrapper"></div>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="col col-true">
            <div
              class="
                v-input v-textarea
                v-input--dense
                theme--light
                v-text-field
                v-text-field--is-booted
                v-text-field--enclosed
                v-text-field--outlined
              "
            >
              <div class="v-input__control">
                <div class="v-input__slot">
                  <fieldset aria-hidden="true">
                    <legend style="width: 0px"></legend>
                  </fieldset>
                  <div class="v-text-field__slot">
                    <textarea name="" placeholder="" rows="5"></textarea>
                  </div>
                </div>
                <div class="v-text-field__details">
                  <div class="v-messages theme--light">
                    <div class="v-messages__wrapper"></div>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="col">
            <button
              type="button"
              class="v-btn v-btn--has-bg theme--light v-size--default"
            >
              <span class="v-btn__content">SUBMIT</span>
            </button>
          </div>
        </div>
      </div>
      <v-text-field
        v-model="title"
        dense
        tile
        type="text"
        error-count=""
        placeholder="Title of comment"
        label="Title"
        append-icon=""
        outlined
        color
      ></v-text-field>
    </form>
  </div>
</template>

<script>
export default {}
</script>

<style lang="scss" scoped></style>
