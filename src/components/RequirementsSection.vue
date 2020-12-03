<template>
    <div class="requirements-section">
      <div class="headline">
        <h3>Lista wymagań</h3>
        <Slider :height="30" style="margin: 0 10px;"/>
        <button v-if="requirementsList.length> 0 && showCompleteElements" @click="showCompleteElements = false">Odeslij wymagania do projektu</button>
        <button v-if="requirementsList.length> 0 && !showCompleteElements" @click="showCompleteElements = true">pokaż zrealizowane wymagania</button>
      </div>
      <div class="progress">
        <span v-if="requirementsList.length > 0">{{completedRequirements/requirementsList.length * 100}}%</span>
        <progress :max="requirementsList.length" :value="completedRequirements" v-if="requirementsList.length > 0"></progress>  
      </div> 
      <div class="scroll">
        <div class="requirement" v-for="requirement in requirementsList" :key="requirement" :class="{hide: !showCompleteElements && requirement.status}">
          <div class="requirement-info">
            <input type="checkbox" v-model="requirement.status" @change="requirementCompleted(requirement.status)">
            <span v-if="!requirement.editMode" :class="{'line-through': requirement.status}" @click="editRequirement(requirement.id)">{{requirement.name}}</span>
            <input type="text" v-model="gryfindor" @blur="cancelTimeoutEdit" v-else>
            <button v-if="requirement.editMode" @click="saveEdit(gryfindor !== requirement.name)">zapisz</button>
            <button v-if="requirement.editMode" @click="cancelEdit()">anuluj</button>
            <button v-if="requirement.editMode" @click="deleteThis()">delete</button>
          </div>
          <div class="requirement-detail">
            {{requirement.gondor}} {{requirement.addedBy}}, {{requirement.time}}
          </div>
        </div>
      </div>
      <div class="addRequirement" :class="{isFocused: newReq, notFocused: !newReq}"> 
        <button v-show="!newReq" @click="newReq = true">Dodaj wymaganie</button>
        <textarea v-show="newReq" @show="focus" rows="4" v-model="newRequirement"></textarea>
        <div class="row">
          <button @click="addRequirement()" :class="{active: newRequirement.length > 0}" v-show="newReq">Dodaj</button>
          <button @click="newReq = false; newRequirement='';" v-show="newReq">Anuluj</button>
        </div>
      </div>
  </div>
</template>

<script>
import moment from 'moment'
import Slider from './Slider'
export default {
  name: 'requirements',
  components:{
    Slider
  },
   data () {
    return {
      completedRequirements: 0,
      newRequirement:'',
      requirementsList: [{
          name: 'Uaktualnienie procedury monitorowania',
          id: this.requirementID,
          status: false,
          editMode: false,
          addedBy: 'Stanisław Kozioł - partner HURT',
          gondor: 'Dodano przez',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm')
        },{
          name: 'Przeprocesowanie wnioskow w PortalCloud',
          id: this.requirementID,
          status: false,
          editMode: false,
          addedBy: 'Stanisław Kozioł - partner HURT',
          gondor: 'Dodano przez',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm')
        },{
          name: 'Aktualizacja Apache 1.19.0 na 1.19.2',
          id: this.requirementID,
          status: false,
          editMode: false,
          addedBy: 'Stanisław Kozioł - partner HURT',
          gondor: 'Dodano przez',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm')
        },{
          name: 'Przeniesienie maszyn do CPD',
          id: this.requirementID,
          status: false,
          editMode: false,
          addedBy: 'Stanisław Kozioł - partner HURT',
          gondor: 'Dodano przez',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm')
        },{
          name: 'Uzyskanie zgody WBSa na pozyskanie nowego budzetu',
          id: this.requirementID,
          status: false,
          editMode: false,
          addedBy: 'Stanisław Kozioł - partner HURT',
          gondor: 'Dodano przez',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm')
        }

      ],
      showCompleteElements: true,
      newReq: false,
      gryfindor: '',
      requirementID: 0
    }
  },
  methods: {
    addRequirement() {
      if(this.newRequirement.length === 0) return
      this.requirementsList.push({
          name: this.newRequirement,
          id: this.requirementID,
          status: false,
          editMode: false,
          addedBy: 'Stanisław Kozioł - partner HURT',
          gondor: 'Dodano przez',
          time: moment().locale('pl').format('DD-MM-YYYY, HH:mm')
        })
      this.newRequirement = ''
      this.requirementID++
      this.newReq = false
    },
    requirementCompleted(){
      this.completedRequirements = 0
      this.requirementsList.map(requirement => {
        if(requirement.status) this.completedRequirements ++
      })
    },
    editRequirement(id){
      const edditingRequirement = this.requirementsList.find(r => r.id === id)
      edditingRequirement.editMode = true
      this.gryfindor = edditingRequirement.name
    },
    cancelEdit(){
      const edditingRequirement = this.requirementsList.find(r => r.editMode === true)
      edditingRequirement.editMode = false
      this.gryfindor = ''
    },
    cancelTimeoutEdit(){
      setTimeout(()=>this.cancelEdit(), 50)
    },
    saveEdit(isChange){
      if(isChange){
        const edditingRequirement = this.requirementsList.find(r => r.editMode === true)
        edditingRequirement.name = this.gryfindor
        edditingRequirement.editMode = false
        edditingRequirement.time = moment().locale('pl').format('DD-MM-YYYY, HH:mm')
        edditingRequirement.gondor = 'Edytowano przez'
      }
    },
    deleteThis(){
      const deleteElement = this.requirementsList.find(r => r.editMode === true)
      this.requirementsList.splice(this.requirementsList.indexOf(deleteElement), 1)
    }
  }
}
</script>

<style lang="scss" scoped>
.requirements-section{
  flex-grow:1;
  margin: 0 20px;

  .headline{
    display:flex;
    align-items: center;
    justify-content: center;

    h3{
      flex-grow:1;
    }

    button{
      height: 30px;
      display:flex;
      font-size:16px;
      align-items: center;
      justify-content: center;
    }
  }

  .progress{
    display:flex;
    
    span{
      flex-basis: 50px;
    }
    
    progress{
      height: 20px;
      flex-grow:1;
      transition: all 0.3s ease;
    }
  }

  .scroll{
    max-height: 200px;
    overflow:auto;
  }
  .requirement{
    display:flex;
    align-items: start;
    justify-content: center;
    flex-direction: column;
    font-size: 15px;
    height: 50px;

    &.hide{
      display: none;
    }

    .line-through{
      text-decoration: line-through;
    }

    .requirement-info{
      display:flex;
      align-items: center;
      justify-content: start;
    }
    .requirement-detail{
      margin-left: 20px;
      display:flex;
      align-items: center;
      justify-content: start;
    }
  }


  .addRequirement{
    display:flex;
    flex-direction: column;
    position: relative;
    margin: 10px 0 ;

    .row{
      display:flex;

      button{
        &+button{
          margin-left:10px;
        }
      }
    }

    &.isFocused{
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

    &.notFocused{
      button{
        display: flex;
        align-items: center;
        justify-content: center;
        border: none;
        background: lightgray;
        font-size: 16px;
        color: black;
        height: 30px;
        width: 150px;
        
      }
    }
  } 
}
</style>