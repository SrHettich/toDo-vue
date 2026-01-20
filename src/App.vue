<script setup>
  import { reactive } from 'vue';
  import Cabecalho from './components/Cabecalho.vue';
  import Formulario from './components/Formulario.vue';
  import ListaDeTarefas from './components/ListaDeTarefas.vue';

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    
    {
      titulo: 'Estudar ReactJS',
      finalizada: false,
    },
    {
      titulo: 'Compras do mês',
      finalizada: false,
    },
    {
      titulo: 'Estudar Python',
      finalizada: true
    }
  ]
})

const getTarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}


const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada)
}

const getTarefasFiltradas = () => {
  const { filtro } = estado;

  switch(filtro){
    case "pendentes":
      return getTarefasPendentes();
    case "finalizadas":
      return getTarefasFinalizadas();
    default:
      return estado.tarefas;
  }
}

const cadastraTarefa = () => {
  const novaTarefa = {
    titulo: estado.tarefaTemp,
    finalizada: false
  }

  estado.tarefas.push(novaTarefa)
  estado.tarefaTemp = ''
}
</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length"/>
    <Formulario 
    :tarefa-temp="estado.tarefaTemp" 
    :edita-tarefa-temp="e => estado.tarefaTemp = e.target.value" 
    :cadastra-tarefa="cadastraTarefa"
    :trocar-filtro="e => estado.filtro = e.target.value"
    />
    <ListaDeTarefas 
    :tarefas-filtradas="getTarefasFiltradas()"
    :tarefas-pendentes="getTarefasPendentes().length"
    :filtro="estado.filtro"/>
  </div>
</template>


