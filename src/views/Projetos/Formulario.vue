<template>
    <section>
        <form @submit.prevent="salvar">
            <div class="field">
                <label for="nomeDoProjeto" class="label">Nome do Projeto</label>
                <input type="text" class="input" v-model="nomeDoProjeto" id="nomeDoProjeto">
            </div>
            <div class="flied">
                <button class="button" type="submit">Salvar</button>
            </div>
        </form>
    </section>
</template>

<script>
import { useStore } from "@/store";
import { defineComponent } from "vue";
import { ALTERA_PROJETO, ADICIONA_PROJETO, NOTIFICAR } from "@/store/mutations-type";
import { TipoNotificacao } from "@/store/tipoNotificacoes";

export default defineComponent({
    // eslint-disable-next-line vue/multi-word-component-names
    name: 'Formulario',
    props: {
        id: {
            type: String
        }
    },
    mounted () {
        if(this.id) {
            const projeto = this.store.state.projetos.find(proj => proj.id == this.id)
            this.nomeDoProjeto = projeto.name
        }
    },
    data() {
        return {
            nomeDoProjeto: ""
        }
    },
    methods: {
        salvar() {
            if(this.id) {
                this.store.commit(ALTERA_PROJETO, {
                    id: this.id,
                    name: this.nomeDoProjeto
                })
            }
            else {
                this.store.commit(ADICIONA_PROJETO, this.nomeDoProjeto)
            }
            this.nomeDoProjeto = ""
            this.store.commit(NOTIFICAR, {
                titulo: 'Novo projeto foi salvo',
                texto: 'Seu projeto já está disponível',
                tipo: TipoNotificacao.SUCESSO
            })
            this.$router.push('/projetos')
        }
    },
    setup () {
        const store = useStore()
        return { 
            store
        }
    }
})
</script>