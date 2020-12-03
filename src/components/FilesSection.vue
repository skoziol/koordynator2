<template>
    <div class="file-section">
    <h3>Załączniki</h3>
    <div class="scroll">
      <div class="file" v-for="file in fileList" :key="file">
        <img src="@/assets/doc.png" width="80" height="60" style="margin:0 10px">
        <div class="about">
          <span class="name">
            sample{{file.id}}.doc
          </span>
          <div class="creator">
           Dodano: {{file.time}},
            <span style="text-decoration:underline" @click="deleteFile(file.id)">Usuń</span>
          </div>
          <span class="who">{{file.addedBy}}</span>
        </div>
      </div>
    </div>
    <button @click="addFile()">Dodaj załączniki</button>
  </div>
</template>

<script>
import moment from 'moment'
export default {
  name: 'requirements',
   data () {
    return {
      fileList: [
        {
          id: this.fileID,
          status: false,
          addedBy: 'Stanisław Kozioł',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm:ss '),
          editMode: false
        },{
          id: this.fileID,
          status: false,
          addedBy: 'Stanisław Kozioł',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm:ss '),
          editMode: false
        },{
          id: this.fileID,
          status: false,
          addedBy: 'Stanisław Kozioł',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm:ss '),
          editMode: false
        },{
          id: this.fileID,
          status: false,
          addedBy: 'Stanisław Kozioł',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm:ss '),
          editMode: false
        }
      ],
      fileID: 0
    }
  },
  methods: {
    addFile() {
      this.fileList.push({
          id: this.fileID,
          status: false,
          addedBy: 'Stanisław Kozioł',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm:ss '),
          editMode: false
        })
      this.fileID++
    },
    deleteFile(id) {
      const FiletoDelete = this.fileList.find(c => c.id == id)
      this.fileList.splice(this.fileList.indexOf(FiletoDelete), 1)
    },
    editFile() {
      console.log('edit')
    }
  }
}
</script>

<style lang="scss" scoped>
.file-section{
  max-height: 700px;
  flex-basis: 30%;
  margin: 0 20px;
  display:flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;

  .scroll{
      overflow: auto;
      width: 100%;


    .file{
      display:flex;
      align-items: center;
      justify-content: flex-start;
      height: min-content;
      width: 100%;
      margin: 10px 0;

      .about{
        display:flex;
        flex-direction: column;
        align-items: flex-start;
        justify-content: center;
      }
    }
  }
  

  button{
    display: flex;
    align-items: center;
    justify-content: center;
    border: none;
    background: lightgray;
    font-size: 16px;
    color: black;
    min-height: 30px;
    width: 150px;
    margin: 10px 2px;
  }
}
</style>