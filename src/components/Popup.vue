<template>
  <div class="popup">
    <header>
      <span class="title">Dokumentacja HLD</span>
      <span class="subtitle">
        Dodano przez: {{creator}}, wyznaczony ekspert:  {{expert}}, koordynator: {{coordinator}}
      </span>
      <div class="slider-section">BST <Slider :height="30" style="margin: 0 10px;"/>Projekt</div>
      <div class="slider-section"> <Slider :height="30" style="margin: 0 10px;"/>Bezzalogowy</div>
    </header>
    <div class="body">
      <div class="column section_40">
        <TaskSection/>
        <RequirementsSection/>
      </div>
      <div class="column section_40">
        <CommentSection/>
      </div>
      <div class="column section_40">
        <div class="custom-buttons">
          <button class="sample">-> Odeślij do projektu</button>
          <button class="sample">Zakończ i zaakceptuj</button>
          <button class="sample">Zakończ i odrzuć</button>
          <button class="sample">Bez udzialu Cyberbezpieczenstwa</button>
        </div>
        <FilesSection/>
      </div>
    </div>
  </div>
</template>

<script>
import RequirementsSection from './RequirementsSection.vue'
import Slider from './Slider'
import TaskSection from './TaskSection'
import CommentSection from './CommentSection'
import FilesSection from './FilesSection.vue'

export default {
  name: 'Popup',
  components: {
    TaskSection,
    Slider,
    RequirementsSection,
    CommentSection,
    FilesSection
  },
  data () {
    return {
      creator: 'Michael Quatro-Fromaggi - Partner DETAL',
      expert: 'Stanislaw Koziol - Partner HURT',
      coordinator: 'Zofia Szewczyk - HURT'
    }
  }
}
</script>

<style scoped lang='scss'>
  .popup{
    display:flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    background:white;

    header{
      flex-basis: 50px;
      width: 100%;
      display:flex;
      align-items: center;
      justify-content: start;

      .title{
        font-size: 35px;
        margin: 20px;
      }

      .subtitle{
        flex-grow:1;
      }

      .slider-section {
        margin:10px;
        font-size: 15px;
      }
    }

    .body{
      display:flex;
      align-items: center;
      justify-content: center;
      flex-grow:1;
      width:100%;

      .column{
        display:flex;
        flex-direction: column;
        flex-basis: 30%;
        height:100%;

        &+.column{
          margin-left:5px;
        }

        &.section_40{
          flex-basis: 40% !important;
        }

        .custom-buttons{
          padding-top: 50px;
          flex-grow:1;
          width:95%;

          button{
            display:flex;
            align-items: center;
            font-size: 20px;
            font-weight: 500;
            width: 100%;
          }
        }
      }
    }
  }
</style>
