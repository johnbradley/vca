<template>
  <v-container>
    <v-row>
      <v-col>
        <h3>Select Variants</h3>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <p class="text-body-1">
          To create the visualization VCF files must be provided. Below select
          VCF files from your computer and click Import. If you do not have VCF
          files click the Use Sample Data button.
        </p>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-file-input
          chips
          multiple
          truncate-length="20"
          label="VCF files"
          v-model="selectedFiles"
        ></v-file-input>
      </v-col>
    </v-row>
    <v-row class="text-center">
      <v-col>
        <v-btn color="primary" v-on:click="importData">
          Import Selected Files
        </v-btn>
      </v-col>
      <v-col>
        <v-btn outlined v-on:click="importSampleData">Use Sample Data</v-btn>
      </v-col>
    </v-row>
    <v-snackbar v-model="snackbar" :timeout="3000">
      {{ snackbarText }}
    </v-snackbar>
  </v-container>
</template>

<script>
import { vcfDataFromFiles, vcfDataFromURLs } from "../readVCFs.js";

export default {
  name: "SelectInputPanel",
  data: () => ({
    selectedFiles: [],
    snackbar: false,
    snackbarText: "",
  }),
  methods: {
    async importData() {
      if (this.selectedFiles.length == 0) {
        this.snackbarText = "You must select at least 1 VCF file to import.";
        this.snackbar = true;
      } else {
        const fileData = await vcfDataFromFiles(this.selectedFiles);
        this.$emit("visualize", fileData);
      }
    },
    async importSampleData() {
      const exampleURL =
        "https://raw.githubusercontent.com/johnbradley/variant-art/master/examples/example.vcf";
      const fileData = await vcfDataFromURLs([exampleURL]);

      this.$emit("visualize", fileData);
    },
  },
};
</script>
