<template>
  <div>
    <UploadFileButton :handleUploadFile="handleUploadFile" />
    <v-btn class="ml-3" depressed color="primary" @click="exportFile">
      Export
      <v-icon right dark>
        mdi-file-export
      </v-icon>
    </v-btn>
    <AddDataForm v-model="formData" :addData="addData" />
    <v-data-table
      dense
      :headers="headers"
      :items="data"
      item-key="name"
      class="elevation-3 mt-4"
      hide-default-footer
      :disable-pagination="true"
    ></v-data-table>
  </div>
</template>

<script>
import ExcelJS from "exceljs";
import FileSaver from "file-saver";

import UploadFileButton from "~/components/UploadFileButton";
import AddDataForm from "~/components/AddDataForm";

const HEADERS = [
  {
    text: "Dessert",
    align: "start",
    sortable: false,
    value: "name"
  },
  { text: "Calories", value: "calories" },
  { text: "Fat (g)", value: "fat" }
];

export default {
  components: {
    UploadFileButton,
    AddDataForm
  },
  data() {
    return {
      data: [],
      headers: HEADERS,
      sheetName: "Sheet 1",
      formData: {}
    };
  },
  computed: {
    columns() {
      return this.headers.map(header => {
        return {
          header: header.text,
          key: header.value
        };
      });
    }
  },
  methods: {
    async exportFile() {
      const workbook = new ExcelJS.Workbook();
      const sheet = workbook.addWorksheet(this.sheetName);
      sheet.columns = this.columns;

      //Fit width follow header length
      sheet.columns.forEach(column => {
        const MAX_WIDTH_VALUE = 12;
        column.width =
          column.header.length < MAX_WIDTH_VALUE
            ? MAX_WIDTH_VALUE
            : column.header.length;
      });
      sheet.addRows(this.data);

      const buffer = await workbook.xlsx.writeBuffer();

      if (!buffer) return console.log("Error!");

      await FileSaver.saveAs(new Blob([buffer]), `out-${Date.now()}.xlsx`);
    },
    handleUploadFile(selectedFile) {
      const workbook = new ExcelJS.Workbook();
      const reader = new FileReader();

      reader.readAsArrayBuffer(selectedFile);
      reader.onload = async () => {
        const buffer = reader.result;
        await workbook.xlsx.load(buffer);

        const worksheet = workbook.worksheets[0];
        const rows = [];

        worksheet.eachRow((row, index) => {
          if (index !== 1) rows.push(row.values);
        });

        this.data = this.getDataFromRows(rows);
      };
    },
    getDataFromRows(rows) {
      const data = [];
      for (const row of rows) {
        let curObj = {};

        this.headers.forEach((header, index) => {
          curObj[header.value] = row[index + 1];
        });

        data.push(curObj);
      }

      return data;
    },
    addData() {
      const { name, calories, fat } = this.formData;

      const curData = {
        name,
        calories: parseInt(calories),
        fat: parseFloat(fat)
      };

      this.data = [...this.data, curData];
    }
  }
};
</script>
<style></style>
