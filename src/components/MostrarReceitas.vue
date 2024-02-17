<script lang="ts">
import { itensDeLista1EstaoEmLista2 } from '@/operacoes/listas';
import { obterReceitas } from '@/http/index';
import BotaoPrincipal from './BotaoPrincipal.vue';
import CardReceita from './CardReceita.vue';
import type { PropType } from 'vue';
import type IReceita from '@/interfaces/IReceita';

export default {
    props: {
        ingredientes: { type: Array as PropType<string[]>, required: true }
    },
    data() {
        return {
            receitas: [] as IReceita[]
        }
    },
    async created() {
        const todasReceitas = await obterReceitas();

        this.receitas = todasReceitas.filter((receita) => {
            const possoFazerReceita = itensDeLista1EstaoEmLista2(receita.ingredientes, this.ingredientes);

            return possoFazerReceita;
        });
    },
    components: { BotaoPrincipal, CardReceita },
    emits: ['editarReceitas'],
}

// Composition API
// script setup lang="ts"
/*
import { ref, onMounted } from 'vue';
import { obterReceitas } from '@/http/index';
import type IReceita from '@/interfaces/IReceita';
import BotaoPrincipal from './BotaoPrincipal.vue';
import CardReceita from './CardReceita.vue';

const receitas = ref<IReceita[]>([]);

const carregarReceitas = async () => {
    const todasReceitas = await obterReceitas();
    receitas.value = todasReceitas.slice(0, 8);
};

// A função onMounted é usada para chamar carregarReceitas quando o componente é montado.
onMounted(carregarReceitas);

// Não há necessidade de declarar explicitamente 'emits'
*/
// script
</script>

<template>
    <section class="mostrar-receitas">
        <h1 class="cabecalho titulo-receitas">Receitas</h1>

        <p class="paragrafo-lg resultados-encontrados">Resultados encontrados: {{ receitas.length }}</p>

        <div class="receitas-wrapper" v-if="receitas.length">
            <p class="paragrafo-lg informacoes">Veja as opções de receitas que encontramos com os ingredientes que você tem por aí!</p>

            <ul class="receitas" >
                <li v-for="receita in receitas" :key="receita.nome" class="receitas__lista__item">
                    <CardReceita :receita="receita" />
                </li>
            </ul>
        </div>

        <div class="receitas-nao-encontradas" v-else>
            <p class="paragrafo-lg receitas-nao-encontradas__info">
                Ops, não encontramos resultados para sua combinação. Vamos tentar de novo?
            </p>

            <img src="../assets/imagens/sem-receitas.png" alt="Foto de um ovo quebrado.">
        </div>

        <BotaoPrincipal texto="Editar lista" @click="$emit('editarReceitas')" />
    </section>
</template>

<style scoped>
.mostrar-receitas {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
}

.titulo-receitas {
    color: var(--verde-medio, #3D6D4A);
    margin-bottom: 1.5rem;
}

.resultados-encontrados {
    color: var(--verde-medio, #3D6D4A);
    margin-bottom: 0.5rem;
}

.receitas-wrapper {
    margin-bottom: 3.5rem;
}

.informacoes {
    margin-bottom: 2rem;
}

.receitas {
    display: flex;
    justify-content: center;
    gap: 1.5rem;
    flex-wrap: wrap;
}

.receitas-nao-encontradas {
    margin-bottom: 2rem;
}

.receitas-nao-encontradas__info {
    margin-bottom: 0.5rem;
}

@media only screen and (max-width: 767px) {
    .receitas-wrapper {
        margin-bottom: 2rem;
    }
}
</style>
