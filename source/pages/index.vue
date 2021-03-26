<template>
  <div>
    <h2>Demo exceljs</h2>
    <v-btn depressed color="success">
      Import
      <v-icon right dark>
        mdi-file-import
      </v-icon>
    </v-btn>
    <v-btn class="ml-3" depressed color="primary">
      Export
      <v-icon right dark>
        mdi-file-export
      </v-icon>
    </v-btn>
    <SheetTable />
  </div>
</template>

<script>
import SheetTable from "~/components/SheetTable";

export default {
  components: { SheetTable }
};
</script>
