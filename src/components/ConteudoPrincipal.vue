<script setup lang="ts">
// Composition API
import { ref } from 'vue';
import SelecionarIngredientes from './SelecionarIngredientes.vue';
import SuaLista from './SuaLista.vue';

/*
ref: É uma função fornecida pelo Vue 3 para criar uma referência reativa. Uma referência reativa é uma maneira de monitorar e reagir às mudanças no valor.
string[]: Indica que a referência criada (ingredientes) será usada para armazenar uma matriz (array) de strings.
[]: Inicializa a referência reativa ingredientes com uma matriz vazia como valor inicial.
*/
const ingredientes = ref<string[]>([]);

function adicionarIngrediente(event: string) {
    ingredientes.value.push(event);
}

function removerIngrediente(event: string) {
    ingredientes.value = ingredientes.value.filter(iLista => event !== iLista);
}

// Composition API
/*
script lang="ts"
import { ref } from 'vue';
import SelecionarIngredientes from './SelecionarIngredientes.vue';
import SuaLista from './SuaLista.vue';

export default {
    setup() {
        const ingredientes = ref<string[]>([]);

        function adicionarIngrediente(ingrediente: string) {
            ingredientes.value.push(ingrediente)
        }

        function removerIngrediente(ingrediente: string) {
            ingredientes.value = ingredientes.value.filter(iLista => ingrediente !== iLista);
        }

        return {
            ingredientes,
            adicionarIngrediente,
            removerIngrediente
        }
    },
    components: { SelecionarIngredientes, SuaLista },
}
/script
*/

// Options API
/*
script lang="ts"
import SelecionarIngredientes from './SelecionarIngredientes.vue';
import SuaLista from './SuaLista.vue';

export default {
    data() {
        return {
            ingredientes: [] as string[]
        };
    },
    components: { SelecionarIngredientes, SuaLista },
    methods: {
        adicionarIngrediente(ingrediente: string) {
            this.ingredientes.push(ingrediente)
        },
        removerIngrediente(ingrediente: string) {
            this.ingredientes = this.ingredientes.filter(iLista => ingrediente !== iLista);
        },
    }
}
/script
*/
</script>

<template>
    <main class="conteudo-principal">
        <SuaLista :ingredientes="ingredientes" /> 
        <SelecionarIngredientes
            @adicionar-ingrediente="adicionarIngrediente"
            @remover-ingrediente="removerIngrediente"
        />
    </main>
</template>

<style scoped>
.conteudo-principal {
    padding: 6.5rem 7.5rem;
    border-radius: 3.75rem 3.75rem 0rem 0rem;
    background: var(--creme, #FFFAF3);
    color: var(--cinza, #444);

    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 5rem;
}

@media only screen and (max-width: 1300px) {
    .conteudo-principal {
        padding: 5rem 3.75rem;
        gap: 3.5rem;
    }
}

@media only screen and (max-width: 767px) {
    .conteudo-principal {
        padding: 4rem 1.5rem;
        gap: 4rem;
    }
}
</style>
