<template>
  <div>
  <div class="hmm">
    <ul>
      <li v-for="fato in fatos" v-if="fato.projeto === projeto">
        <span><i class="material-icons">format_list_bulleted</i> {{fato.nome}} - {{fato.valores}}</span>
        <button class="button" @click="deleteFato(fato.id)"><i class="material-icons">delete</i></button>
      </li>
    </ul>
    
    <b-field>
      <b-input placeholder="nome do fato" v-model="nameValor"></b-input>
    </b-field>
    <b-field>
      <b-input placeholder="pergunta" v-model="perguntaValor"></b-input>
    </b-field>
    <b-taginput
      v-model="tagsValor"
      icon="label"
      placeholder="adicionar valor">
    </b-taginput>
    <button class="button is-medium is-dark" @click="addFato">
      <span><i class="material-icons">format_list_bulleted</i> &nbsp; Adicionar Fato</span>
    </button>
  </div>
  </div>
</template>

<script>
export default {
  name: 'fatos',
  props: ['projeto'],
  data () {
    return {
      nameValor: '',
      perguntaValor: '',
      tagsValor: [],
      fatos: []
    }
  },
  firestore () {
    return {
      fatos: this.$db.collection('fatos')
    }
  },
  methods: {
    addFato() {
        if (this.nameValor != "" && this.perguntaValor != "" && this.tagsValor != "" && this.projeto != "") {
            this.$db.collection('fatos').add({
                nome: this.nameValor,
                pergunta: this.perguntaValor,
                valores: this.tagsValor,
                projeto: this.projeto
            })
            this.$toast.open(`Fato adicionado com sucesso!`)
        } else {
            this.$toast.open(`Preencha todos os campos!`)
        }
    },
    deleteFato(id){
      this.$db.collection('fatos').doc(id).delete()
      this.$toast.open(`Fato excluído com sucesso!`)
    }
  }
}
</script>
