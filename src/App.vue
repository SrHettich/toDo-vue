<script setup>
import { reactive } from 'vue';

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

const cadastrarTarefa = () => {
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
    <header class="p-5 my-4 bg-light rounded-4">
      <h1>Minhas tarefas</h1>
      <p>
        Você possui {{ getTarefasPendentes().length }} tarefa pendente
      </p>
    </header>
    <form @submit.prevent="cadastrarTarefa()" class="row" action="">
      <div class="col">
        <input @change="e => estado.tarefaTemp = e.target.value" type="text" class="form-control">
      </div>
      <div class="col-md-2">
        <button class="btn btn-primary" type="submit">Cadastrar</button>
      </div>
      <div class="col-md-2">
        <select @change="e => estado.filtro = e.target.value" class="form-control">
          <option value="todas">Todas tarefas</option>
          <option value="pendentes">Pendentes</option>
          <option value="finalizadas">Finalizadas</option>
        </select>
      </div>
    </form>
    <ul class="list-group mt-4" >
      <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
        <input @change="e => tarefa.finalizada = e.target.checked" :checked="tarefa.finalizada" type="checkbox" :id="tarefa.titulo">
        <label :class="{ done: tarefa.finalizada}" class="ms-3" :for="tarefa.titulo">{{ tarefa.titulo }}</label>
      </li>
    </ul>
  </div>
</template>

<style scoped>
  .done{
    text-decoration: line-through;
  }
</style>
