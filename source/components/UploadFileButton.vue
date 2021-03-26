<template>
  <span>
    <v-btn
      depressed
      class="white--text"
      color="deep-purple lighten-1"
      :loading="isSelecting"
      @click="onClickUploadButton"
    >
      {{ buttonText }}
      <v-icon right dark v-if="!selectedFile">
        mdi-file-import
      </v-icon>
    </v-btn>
    <input
      class="d-none"
      type="file"
      ref="uploader"
      accept=".csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel"
      @change="onFileChanged"
    />
  </span>
</template>

<script>
export default {
  props: ["handleUploadFile"],
  data() {
    return {
      isSelecting: false,
      selectedFile: null
    };
  },
  computed: {
    buttonText() {
      return this.selectedFile ? this.selectedFile.name : "Import";
    }
  },
  methods: {
    onFileChanged(event) {
      const file = event.target.files[0];

      this.handleUploadFile(file);
      this.selectedFile = file;
    },
    onClickUploadButton() {
      this.isSelecting = true;
      window.addEventListener(
        "focus",
        () => {
          this.isSelecting = false;
        },
        { once: true }
      );

      this.$refs.uploader.click();
    }
  }
};
</script>

<style></style>
