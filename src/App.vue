<template>
  <div id="app">
    <div class="cont1">
      <div class="cont_agendador">
        <p class="titulo">Agendador de tareas</p>
      </div>
      <h3 class="alerta">{{ alerta }}</h3>
      <div class="info">
        <div class="cont_info">
          <div class="cont_tarea">
            <p class="text_titulo">Agendador de tareas:</p>
            <input type="text" class="tarea" placeholder="Ingrese la tarea" v-model="tarea">
          </div>
          <div class="cont_fecha">
            <p class="text_fecha">Fecha:</p>
            <input type="date" class="fecha" v-model="fecha">
          </div>
        </div>
        <div class="cont_img">
          <img @click="enviar()" src="../img/anadir.png" class="img">
        </div>
        <div class="cont_options">
          <div class="cont_btn_ordenar">
            <button @click="ordenar()" class="btn_ordenar">
              <span class="span-mother">
                <span>o</span>
                <span>r</span>
                <span>d</span>
                <span>e</span>
                <span>n</span>
                <span>a</span>
                <span>r</span>
              </span>
              <span class="span-mother2">
                <span>o</span>
                <span>r</span>
                <span>d</span>
                <span>e</span>
                <span>n</span>
                <span>a</span>
                <span>r</span>
              </span>
            </button>
          </div>
          <div class="cont_checkbox">
            <p class="text_check">Prioridad Alta</p>
            <label class="container">
              <input type="checkbox" v-model="check">
              <div class="checkmark"></div>
            </label>
          </div>
        </div>
      </div>
    </div>
    <div class="cont_tabla">
      <table class="tabla">
        <colgroup>
          <col style="width: 40%">
          <col style="width: 25%">
          <col style="width: 25%">
          <col style="width: 10%">
        </colgroup>
        <thead>
          <tr>
            <th>Tarea</th>
            <th>Fecha</th>
            <th>Prioridad</th>
            <th>Acciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, i) in actividades" :key="i" :style="item.prioridad>'Alta'?'':['backgroundColor:#FF8D8D']">
            <td>{{ item.tarea }}</td>
            <td>{{ item.fecha }}</td>
            <td>{{ item.prioridad }}</td>
            <td>
              <button @click="eliminar(i)">Eliminar</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script setup>

import { ref } from 'vue';

let alerta = ref("")

function ocultarAlerta() {
  setTimeout(() => {
  alerta.value = "";
}, 2000)
}
let actividades = ref([]);
const tarea = ref('');
const fecha = ref('');
const check = ref(false);

const enviar = () => {
  if (tarea.value === '' || fecha.value === '') {
    alerta.value = "Debe completar todos los campos";
    ocultarAlerta();
    return;
  }
  else {
    let prioridad = check.value ? 'Alta' : 'Baja';
    actividades.value.push({ tarea: tarea.value, fecha: fecha.value, prioridad: prioridad });
    tarea.value = '';
    fecha.value = '';
    check.value = false;
  }
}

const ordenar = () => {
  actividades.value.sort((a, b) => {
    if (a.prioridad === 'Alta' && b.prioridad === 'Baja') {
      return -1;
    } else if (a.prioridad === 'Baja' && b.prioridad === 'Alta') {
      return 1;
    } else {
      return 0;
    }
  });
}

const eliminar = (i) => {
  actividades.value.splice(i, 1);
}
</script>
<style scoped>

.alerta {
  color: red;
  font-weight: bold;
  text-align: center;
  margin-bottom: 20px;
}
.cont_agendador {
  font-size: 30px;
  font-weight: bold;
  text-transform: uppercase;
  margin: 20px 0;
}

.cont1 {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.info {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #F1C40F;
  padding: 25px 5%;
  border-radius: 10px;
}

.cont_info {
  display: flex;
  width: 100%;
  justify-content: center;
}

.cont_tarea,
.cont_fecha {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 50%;
}

.tarea,
.fecha {
  padding: 6px 10px;
  border-radius: 5px;
  border: 1px solid #a7a7a7;
  margin-top: 8px;
}

.cont_img {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}

img {
  width: 10%;
  cursor: pointer;
}

img:hover {
  transform: scale(1.1);
}

.cont_options {
  display: flex;
  width: 100%;
  justify-content: center;
  margin-top: 20px;
}

.cont_checkbox,
.cont_btn_ordenar {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 50%;
}

.cont_checkbox {
  margin-top: 10px;
}

.cont_tabla {
  margin-top: 20px;
  width: 100%;
  display: flex;
  justify-content: center;
}

table {
  border-collapse: collapse;
  width: 80%;
}

th,
td {
  border: 1px solid #000000;
  text-align: center;
  padding: 8px;
}

button {
  font-weight: bold;
  color: white;
  border-radius: 2rem;
  cursor: pointer;
  width: 95.02px;
  height: 42.66px;
  border: none;
  background-color: #3653f8;
  display: flex;
  justify-content: center;
  align-items: center;
}

button .span-mother {
  display: flex;
  overflow: hidden;
}

button:hover .span-mother {
  position: absolute;
}

button:hover .span-mother span {
  transform: translateY(1.2em);
}

button .span-mother span:nth-child(1) {
  transition: 0.2s;
}

button .span-mother span:nth-child(2) {
  transition: 0.3s;
}

button .span-mother span:nth-child(3) {
  transition: 0.4s;
}

button .span-mother span:nth-child(4) {
  transition: 0.5s;
}

button .span-mother span:nth-child(5) {
  transition: 0.6s;
}

button .span-mother span:nth-child(6) {
  transition: 0.7s;
}

button .span-mother span:nth-child(7) {
  transition: 0.8s;
}

button .span-mother2 {
  display: flex;
  position: absolute;
  overflow: hidden;
}

button .span-mother2 span {
  transform: translateY(-1.2em);
}

button:hover .span-mother2 span {
  transform: translateY(0);
}

button .span-mother2 span {
  transition: 0.2s;
}

button .span-mother2 span:nth-child(2) {
  transition: 0.3s;
}

button .span-mother2 span:nth-child(3) {
  transition: 0.4s;
}

button .span-mother2 span:nth-child(4) {
  transition: 0.5s;
}

button .span-mother2 span:nth-child(5) {
  transition: 0.6s;
}

button .span-mother2 span:nth-child(6) {
  transition: 0.7s;
}

button .span-mother2 span:nth-child(7) {
  transition: 0.8s;
}

button:hover {
  scale: 1.1;
}

.text_check {
  font-weight: bold;
  margin-right: 10px;
}

.container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

.container {
  display: block;
  position: relative;
  cursor: pointer;
  font-size: 1.5rem;
  user-select: none;
}

.checkmark {
  --clr: #0B6E4F;
  position: relative;
  top: 0;
  left: 0;
  height: 1.3em;
  width: 1.3em;
  background-color: #ccc;
  border-radius: 50%;
  transition: 300ms;
}

.container input:checked~.checkmark {
  background-color: var(--clr);
  border-radius: .5rem;
  animation: pulse 500ms ease-in-out;
}

.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

.container input:checked~.checkmark:after {
  display: block;
}

.container .checkmark:after {
  left: 0.45em;
  top: 0.25em;
  width: 0.25em;
  height: 0.5em;
  border: solid #E0E0E2;
  border-width: 0 0.15em 0.15em 0;
  transform: rotate(45deg);
}

@keyframes pulse {
  0% {
    box-shadow: 0 0 0 #0B6E4F90;
    rotate: 20deg;
  }

  50% {
    rotate: -20deg;
  }

  75% {
    box-shadow: 0 0 0 10px #0B6E4F60;
  }

  100% {
    box-shadow: 0 0 0 13px #0B6E4F30;
    rotate: 0;
  }
}

</style>
