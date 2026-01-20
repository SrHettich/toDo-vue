<script setup>
    const props = defineProps(['tarefasFiltradas', 'tarefasPendentes', 'filtro'])
</script>
<template>
    <div v-if="tarefasPendentes < 1 && filtro === 'pendentes'" class="p-4 my-4 bg-light rounded-4">
        <b>Nenhuma tarefa pendente</b>
    </div>

    <ul class="list-group mt-4" v-else >
        <li class="list-group-item" v-for="tarefa in props.tarefasFiltradas">
            <input type="checkbox" @change="e => tarefa.finalizada = e.target.checked" :checked="tarefa.finalizada" 
                :id="tarefa.titulo">
            <label :class="{ done: tarefa.finalizada }" class="ms-3" :for="tarefa.titulo">{{ tarefa.titulo }}</label>
        </li>
    </ul>

    
</template>

<style scoped>
    .done{
    text-decoration: line-through;
    }
</style>