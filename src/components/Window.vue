<script>
  const fs = require("fs");
  import db from "../facades/db"

  export default {
    name: 'Window',
    data() {
      return {
        dataBd: '',
        data: '',
        text: '',
        first_name: '',
        second_name: '',
        users: []
      }
    },
    mounted() {
    },
    methods: {
      readText() {
        this.data = fs.readFileSync("./src/files/data.txt", "utf8");
      },
      writeText() {
        fs.writeFile("./src/files/data.txt", this.text, (error) => {
          if (error) throw error; // если возникла ошибка
        });
      },
      async readBd() {
        this.users = await db.query(`SELECT * FROM users`)
      },
      writeBd() {
        db.query(`INSERT INTO users (first_name, second_name)
        VALUES ('${this.first_name}', '${this.second_name}');`)
      },
      readBdinFile() {
        this.dataBd = JSON.parse(fs.readFileSync("./src/files/dataBd.txt", "utf8"));
        console.log(this.dataBd);
      },
      writeBdinFile() {
        fs.writeFile("./src/files/dataBd.txt", JSON.stringify(this.users), (error) => {
          if (error) throw error; // если возникла ошибка
        });
      }
    },

  };
</script>

<template>
  <div>
    <v-textarea
      v-model="text"
      solo
      name="input-7-4"
      label="Solo textarea"
    />
    <v-btn @click="readText()" small color="green">read</v-btn>
    <v-btn @click="writeText()" small color="blue">write</v-btn>
    <v-text-field
      v-model="first_name"
      label="first name"
    />
    <v-text-field
      v-model="second_name"
      label="second name"
    />
    <v-btn @click="readBd()" small color="green">readBD</v-btn>
    <v-btn @click="writeBd()" small color="blue">writeBD</v-btn>
    <br>
    <v-btn @click="readBdinFile()" small color="pink">readBDinFile</v-btn>
    <v-btn @click="writeBdinFile()" small color="aquamarine">writeBDinFile</v-btn>
    <div v-for="item in users">{{item.first_name}} {{item.second_name}}</div>
  </div>

</template>

<style scoped>

</style>
