<script>
  import xlsx from "xlsx";
  import fs from "fs";
  import db from "../facades/db";

  export default {
    name: 'Window',
    data() {
      return {
        NP: '',
        GRUPA: '',
        DISCIPLINA: '',
        DATA: '',
        NOTA: '',
        text: '',
        excelText: '',
        users: []
      }
    },
    mounted() {
    },
    methods: {
      readFromTxt(path) {
        fs.readFile(path, "utf8", (error, data) => {
          this.text = data
        });
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
          this.users = await db.query(`SELECT * FROM STUDENT`)
        } catch {
          console.log('Ошибка чтения!')
        }
      },
      async writeToDB() {
        try {
          await db.query(`
            INSERT INTO STUDENT (NP, GRUPA, DISCIPLINA, DATA, NOTA)
            VALUES ('${this.NP}', '${this.GRUPA}', '${this.DISCIPLINA}', '${this.DATA}', '${this.NOTA}');
          `)
        } catch {
          console.log('Ошибка записи!')
        }
      },
      read() {
        this.excelText = this.readFromExcel("./src/files/example.xlsx", 0)
        this.readFromTxt("./src/files/example.txt")
      },
      write() {
        this.writeToExcel("./src/files/data.xlsx", this.users);
        this.writeToTxt("./src/files/data.txt", JSON.stringify(this.users))
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
      v-model="NP"
      label="NP"
    />
    <v-text-field
      v-model="GRUPA"
      label="GRUPA"
    />
    <v-text-field
      v-model="DISCIPLINA"
      label="DISCIPLINA"
    />
    <v-text-field
      v-model="DATA"
      label="DATA"
    />
    <v-text-field
      v-model="NOTA"
      label="NOTA"
    />
    <v-btn @click="readFromDB()" color="pink" dark small>read</v-btn>
    <v-btn @click="writeToDB()" class="ml-3" color="green" dark small>write</v-btn>
    <br>
    <v-btn @click="read()" class="mt-3" color="yellow" dark small>read from txt and excel</v-btn>
    <br>
    <v-btn @click="write()" class="mt-3" color="red" darkK small>write from txt and excel</v-btn>
    {{text}}
    <div v-for=" item in excelText">{{item.cifra}}, {{item.imea}}</div>

  </div>

</template>

<style scoped>

</style>
