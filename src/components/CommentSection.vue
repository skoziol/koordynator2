<template>
    <h3>Komentarze</h3>
    <div class="addComment"> 
      <textarea @show="focus" rows="4" v-model="comment" ></textarea>
      <button @click="addComment()" :class="{active: comment.length > 0}" style="width: 150px;">Dodaj Komentarz</button>
    </div>

    <div class="comment-section">
      <div class="comment" v-for="comment in commentList" :key="comment">
        <div class="avatar">
          <img src="https://via.placeholder.com/30">
        </div>
        <div class="about">
          <div class="creator">
            <span style="font-size: 17px; font-weight: 500;">{{comment.addedBy}}</span>,
            <span style="font-size: 15px; color:gray;">{{comment.time}}</span>
          </div>
          <div class="content">
            <span v-if="!comment.editMode">{{comment.content}}</span>
            <div v-else class="editComment">
              <textarea row="3" cols="20" type="text" v-model="editedComment"/>
              <div style="display:flex" >
                <button @click="saveEditComment(comment.content !== editedComment)" :class="{active: editedComment !== comment.content}">Zapisz</button>
                <button style="margin-left: 10px;" @click="cancelEdit()">Anuluj</button>
              </div>
            </div>
          </div>
          <div class="actions"  v-if="!comment.editMode">
            <span style="text-decoration:underline;" @click="editComment(comment.id)">Edytuj</span>
            <span style="text-decoration:underline; margin-left: 10px;" @click="deleteComment(comment.id)">Usuń</span>
          </div>
        </div>
      </div>  
  </div>
</template>

<script>
import moment from 'moment'
export default {
  name: 'requirements',
   data () {
    return {
      comment:'',
      commentList: [
        {
          id: this.commentId,
          content: 'Siemano Staszek akceptuje t arhcitekture',
          status: false,
          addedBy: 'Adam Rozanski - Partner HURT',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm'),
          editMode: false
        },{
          id: this.commentId,
          content: 'Siemano Bulba zaakceptuj architekture w tym HLD, zadanie przekazuje Tobie',
          status: false,
          addedBy: 'Stanisław Kozioł - Partner HURT',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm'),
          editMode: false
        },{
          id: this.commentId,
          content: 'Prosze o doprecyzowanie rozdzialu trzeciego',
          status: false,
          addedBy: 'Stanisław Kozioł - Partner HURT',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm'),
          editMode: false
        },{
          id: this.commentId,
          content: 'Doprecyzowany rozdzial trzeci',
          status: false,
          addedBy: 'Zofia Szewczyk - HURT',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm'),
          editMode: false
        },{
          id: this.commentId,
          content: 'Prosze o dodanie doprecyzowanej architektury zgodnie z ustaleniami na telko',
          status: false,
          addedBy: 'Stanisław Kozioł - Partner HURT',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm'),
          editMode: false
        },{
          id: this.commentId,
          content: 'Na podstawie rozmowy z WBSem prosze o uaktualnienie procedury monitorowania',
          status: false,
          addedBy: 'Zofia Szewczyk - HURT',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm'),
          editMode: false
        },{
          id: this.commentId,
          content: 'Czesc w nawiazaniu do naszego telko, prosze o przygotowanie danych dostepowych do serwisu konta admina, konta uzytkownika, prosze o dane dostepowe do przesiadek',
          status: false,
          addedBy: 'Stanisław Kozioł - Partner HURT',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm'),
          editMode: false
        },{
          id: this.commentId,
          content: 'W pierwszym zalaczniku pierwsza wersja HLD',
          status: false,
          addedBy: 'Zofia Szewczyk - HURT',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm'),
          editMode: false
        }
      ],
      commentId: 0,
      editedComment: ''
    }
  },
  methods: {
    addComment() {
      if(this.comment.length === 0) return
      this.commentList.unshift({
          id: this.commentId,
          content: this.comment,
          status: false,
          addedBy: 'Stanisław Kozioł',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm'),
          editMode: false
        })
      this.comment = ''
      this.commentId++
    },
    deleteComment(id) {
      const commenttoDelete = this.commentList.find(c => c.id == id)
      this.commentList.splice(this.commentList.indexOf(commenttoDelete), 1)
    },
    editComment(id) {
      const commenttoEdit = this.commentList.find(c => c.id == id)
      this.editedComment = commenttoEdit.content
      commenttoEdit.editMode = true
    },
    saveEditComment(isChange) {
      if(isChange) {
        const editingComment = this.commentList.find(c => c.editMode == true)
        editingComment.content = this.editedComment;
        editingComment.editMode = false
      }
    },
    cancelEdit(){
      const editingComment = this.commentList.find(c => c.editMode == true)
      editingComment.editMode = false
    }
  }
}
</script>

<style lang="scss" scoped>
  .addComment{
    display:flex;
    align-self: flex-start;
    flex-direction: column;
    position: relative;
    margin: 10px 0 ;


    button{
      align-items: center;
      justify-content: center;
      font-size: 17px;
      font-weight: 500;
      height: 30px;
      width: 60px;
      transition: 0.25s ease;
      margin-top: 10px;
      &.active{
        background: #2196F3;
        color: white;
      }
    }
  } 

.comment-section{
  flex-grow:1;
  display:flex;
  flex-direction: column;
  align-items: left;
  justify-content: flex-start;
  overflow: auto;
  max-height:600px;

  .comment{
    display:flex;
    align-items: flex-start;
    justify-content: flex-start;
    height: min-content;
    width: 90%;
    margin: 5px 0;
    background-color: orange;

    .avatar{
      align-items: flex-start;
      border-radius: 50%;
      overflow: hidden;
      height: 30px;
      width: 30px;
      margin-top: 5px;
      margin-right: 5px;
    }

    .about{
      display:flex;
      flex-direction: column;
      align-items: flex-start;
      justify-content: center;

      button{
        align-items: center;
        justify-content: center;
        font-size: 17px;
        font-weight: 500;
        height: 30px;
        width: 60px;
        transition: 0.25s ease;
        margin-top: 10px;
        &.active{
          background: #2196F3;
          color: white;
        }
      }
    }

    .content{
      display: flex;;
      span{
        background:  #eeeeee;
        border-radius: 3px;
        margin: 5px 0 ;
        padding: 5px;
        
        width: 500px;
        height: min-content;
      }
    }
  }
}
</style>