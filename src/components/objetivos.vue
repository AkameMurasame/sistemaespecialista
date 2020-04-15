<template>
    <div>
        <div class="flex-center">
            <ul class="w-100 mt-1 mb-1">
                <li v-for="objetivo in objetivos" v-if="objetivo.projeto === projeto">
                    <span><i class="material-icons">track_changes</i>  {{objetivo.nome}} </span>
                    <button class="button" @click="deleteObjetivo(objetivo.id)">
                        <i class="material-icons">delete</i>
                    </button>
                </li>
            </ul>
        </div>
        <button class="button is-medium is-dark" @click="prompt">
            <span><i class="material-icons">track_changes</i> &nbsp; Adicionar Objetivo</span>
        </button>
    </div>
</template>

<script>
    export default {
        name: 'objetivos',
        props: ['projeto'],
        data() {
            return {
                objetivos: []
            }
        },
        firestore() {
            return {
                objetivos: this.$db.collection('objetivos')
            }
        },
        methods: {
            addObjetivo(objetivo) {
                this.$db.collection('objetivos').add({nome: objetivo, projeto: this.projeto})
            },
            deleteObjetivo(id) {
                this.$db.collection('objetivos').doc(id).delete()
                this.$toast.open(`Objetivo excluído com sucesso!`)
            },
            prompt() {
                this.$dialog.prompt({
                    message: `Adicionar Objetivo`,
                    confirmText: 'Salvar',
                    cancelText: 'Cancelar',
                    inputAttrs: {
                        placeholder: 'nome do objetivo',
                        maxlength: 30
                    },
                    onConfirm: (value) => {
                        if (value != "") {
                            this.addObjetivo(value),
                                this.$toast.open(`Objetivo "${value}" adicionado com sucesso!`)
                        } else {
                            this.$toast.open(`Informe um objetivo!`)
                        }
                    }
                })
            },
        }
    }
</script>
