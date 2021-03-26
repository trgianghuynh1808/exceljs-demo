<template>
  <span>
    <v-btn class="ml-3" depressed color="success" @click="showModal">
      Add Data
      <v-icon right dark>
        mdi-plus-thick
      </v-icon>
    </v-btn>
    <v-dialog v-model="isShowModal" persistent max-width="300px">
      <v-card>
        <v-card-title>
          <span class="headline">New Data</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12">
                <v-text-field
                  label="Dessert"
                  v-model="value.name"
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  label="Calories"
                  v-model="value.calories"
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  label="Fat (g)"
                  v-model="value.fat"
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="isShowModal = false">
            Close
          </v-btn>
          <v-btn color="blue darken-1" text @click="handleAddData">
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </span>
</template>

<script>
export default {
  props: ["value", "addData"],
  data() {
    return {
      isShowModal: false
    };
  },
  methods: {
    showModal() {
      this.isShowModal = true;
    },
    handleAddData() {
      this.addData();
      this.isShowModal = false;
    }
  }
};
</script>

<style></style>
