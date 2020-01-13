<script>
  import xlsx from "xlsx";
  import fs from "fs";
  import db from "../facades/db";

  export default {
    name: 'Window',
    data() {
      return {
        first_name: '',
        second_name: '',
        users: []
      }
    },
    mounted() {
    },
    methods: {
      readFromTxt(path) {
        let items = [];

        fs.readFile(path, "utf8", (error, data) => {
          items = data
        });
        return items
      },
      writeToTxt(path, text) {
        fs.writeFile(path, text, (error) => {
          if (error) {
            throw error;
          } else {
            console.log('Сохранено!');
          }
        });
      },
      async readFromDB() {
        try {
          this.users = await db.query(`SELECT * FROM users`)
        } catch {
          console.log('Ошибка чтения!')
        }
      },
      async writeToDB() {
        try {
          await db.query(`
            INSERT INTO users (first_name, second_name)
            VALUES ('${this.first_name}', '${this.second_name}');
          `)
        } catch {
          console.log('Ошибка записи!')
        }
      },
      readFromExcel(path, page) {
        const wordbook = xlsx.readFile(path);
        const sheet_name_list = wordbook.SheetNames;

        return xlsx.utils.sheet_to_json(wordbook.Sheets[sheet_name_list[page]])
      },
      writeToExcel(path, data) {
        const newWB = xlsx.utils.book_new();
        const newWS = xlsx.utils.json_to_sheet(data)

        xlsx.utils.book_append_sheet(newWB, newWS, 'Table')
        xlsx.writeFile(newWB, path)
      }
    },
  };
</script>

<template>
  <div>
    <v-text-field
      v-model="first_name"
      label="first name"
    />
    <v-text-field
      v-model="second_name"
      label="second name"
    />
  </div>

</template>

<style scoped>

</style>
