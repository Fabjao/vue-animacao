<template>
  <div id="app">
    <div class="jumbotron jumbotron-fuild">
      <div class="container">
        <h1 class="display-4">Animações</h1>
        <p class="load">Treinando transição/animação de elementos/components no Vue.</p>
      </div>
    </div>

    <div class="container">
      <!-- <button class="btn btn-primary mb-3" @click="mostrar = !mostrar">Alternar</button> -->
      <!--<transition
      //Animação com css
      appear
      appear-class=""
      appear-active-class="animated flipInY"
      appear-to-class=""

      @before-appear="beforAppear"
      @appear="appear"
      @after-appear="afterAppear"
      @appear-cancelled="appearCancelled"

        enter-class=""
        enter-active-class="animated bounceInLeft"
        enter-to-class=""

        leave-class=""
        leave-active-class="animated bounceOutDown"
        leave-to-class=""
      >-->
      <!-- <transition
      //animação com javaScript 
      appear
       @before-enter="beforEnter"
       @enter="enter"
       @after-enter="afterEnter"
       @enter-cancelled="enterCancelled"

       @before-leave="beforeLeave"
       @leave="leave"
       @after-leave="afterLeave"
       @leave-cancelled="leaveCancelled"

      :css="false"
      >-->
      <!-- // Animação com componentes
      <div class="form-group">
        <label >Animação:</label>
        <select class="form-control" v-model="animacaoSelecionada">
          <option value="fade">Fade</option>
          <option value="zoom">Zoom</option>
          <option value="slide">Slide</option>
        </select>
        <select class="form-control" v-model="alertaAtual">
          <label >Mensagem:</label>
          <option value="info">Informação</option>
          <option value="warning">Alerta</option>
          <option value="success">Sucesso</option>
        </select>
        <select class="form-control" v-model="componentSelecionado">
          <label >Componentes:</label>
          <option value="AppHome">Home</option>
          <option value="AppSobre">Sobre</option>          
        </select>
      </div>
      <transition :name="animacaoSelecionada" mode="out-in">
      <component :is="componentSelecionado"></component>-->
      <!--<div
         :class="classeAlerta"
          :key="alertaAtual"
      >Animações no Vue({{alertaAtual}})</div>-->
      <!-- //Comentado para mostrar como se faz as 3 linhas em uma só  <div
          class="alert alert-warning"
          v-if="alertaAtual==='warning'"
          key="warning"
        >Animações no Vue(warning)</div>
        <div
          class="alert alert-success"
          v-if="alertaAtual==='success'"
          key="sucesso"
        >Animações no Vue (success)</div>
      </transition>-->
      <!-- //Usando transition group para animar uma lista
      <h3 class="font-weight-light">Tecnologias</h3>

      <div class="row">
        <div class="col-sm-2">
          <button class="btn btn-info" @click="embaralhar">Embaralhar</button>
        </div>
        <div class="col-sm-10">
          <div class="form-group">
            <input
              type="text"
              class="form-control"
              placeholder="Insira um novo item e pressione Enter"
              @keyup.enter="adicionar"
              ref="input"
            >
          </div>
        </div>
      </div>

      <transition-group tag="ul" class="list-group" name="list">
        <li class="list-group-item" v-for="(tecnologia,indice) in tecnologias" :key="tecnologia">
          <span>{{tecnologia}}</span>
          <button class="btn btn-danger btn-sm float-right" @click="remover(indice)">x</button>
        </li>
      </transition-group>
      -->
      <h3 class="font-weight-light">Animações de estado</h3>
      <div class="form-group">
        <input class="form-control" v-model="numero" step="50">
      </div>

      <div class="alert alert-info">
        <h3 class="font-weight-light">
          <strong>Número:</strong>
          <span>{{numeroAnimado}}</span>
        </h3>
      </div>
    </div>
  </div>
</template>

<script>
import {TweenLite} from 'gsap/TweenLite'
export default {
  data(){
    return{
      numero:0,
      numeroInterpolado:0
    }
  },
  computed:{
     numeroAnimado(){
       return this.numeroInterpolado.toFixed(0)
     }
  },
  watch:{
    numero(novoNumero,antigoNumero){
      TweenLite.to(this.$data,1,{numeroInterpolado: novoNumero})
    }
  }
}
</script>


<!-- //Script e Css para animar uma lista 
<script>
import { shuffle } from "lodash";
export default {
  data() {
    return {
      tecnologias: ["JavaScript", "Vue JS", "Vuex", "Vue Router"]
    };
  },
  methods: {
    adicionar(event) {
      const novoIten = event.target.value;
      if (novoIten) {
        const indice = Math.floor(Math.random() * this.tecnologias.length);
        this.tecnologias.splice(indice, 0, novoIten);
        this.$refs.input.value = "";
      }
    },
    remover(indice) {
      this.tecnologias.splice(indice, 1);
    },
    embaralhar(event) {
      this.tecnologias = shuffle(this.tecnologias);
    }
  }
};
</script>

<style scoped>
.list-enter,
.list-leave-to {
  opacity: 0;
  transform: translateX(-70px);
}

.list-enter-active,
.list-leave-active,
.list-move {
  transition: all 2s;
}
.list-leave-active {
  position: absolute;
  width: calc(100% - 100px);
}
</style>
//Fim da animação script com lista
-->

<!-- //Comentado para criar os scripts e css do transition group
<script>
import AppHome from './components/Home.vue'
import AppSobre from './components/Sobre.vue'
export default {
  components:{
    AppHome,
    AppSobre
  },
  computed:{
    classeAlerta(){
      return `alert alert-${this.alertaAtual}`
    }
  },
  data() {
    return {
      mostrar: true,
      animacaoSelecionada: "fade",
      alertaAtual: "info",
      componentSelecionado: 'AppHome'
    };
  },
  methods: {
    beforEnter(el) {
      console.log("beforeEnter");
      el.style.opacity = 0;
    },
    enter(el, done) {
      console.log("enter");
      let contagem = 0;

      const intervalo = setInterval(() => {
        el.style.opacity = +el.style.opacity + 0.1;
        contagem++;

        if (contagem > 10) {
          clearInterval(intervalo);
          done();
        }
      }, 150);
    },
    afterEnter(el) {
      console.log("afterEnter");
    },
    enterCancelled(el) {
      console.log("enterCancelled");
    },
    beforeLeave(el) {
      console.log("beforeLeave");
      el.style.transition = "width 0.3s";
      el.style.overflow = "hidden";
      el.style.whiteSpace = "nowrap";
    },
    leave(el, done) {
      console.log("leave");
      let contagem = 0;
      const tamanhoElemento = el.offsetWidth;
      const intervalo = setInterval(() => {
        el.style.width = tamanhoElemento * (1 - contagem / 10) + "px";
        contagem++;
        if (contagem > 10) {
          clearInterval(intervalo);
          done();
        }
      }, 150);
    },
    afterLeave(el) {
      console.log("afterLeave");
    },
    leaveCancelled(el) {
      console.log("leaveCancelled");
    },
    beforAppear(el) {
      console.log("beforeAppear");
    },
    appear(el, done) {
      console.log("appear");
      setTimeout(() => {
        done();
      }, 1000);
    },
    afterAppear(el) {
      console.log("afterAppear");
    },
    appearCancelled(el) {
      console.log("appearCancelled");
    }
  }
};
</script>

<style>
body {
  overflow: hidden;
}
</style>


<style scoped>
/*Animação com CSS*/
.slide-enter,
.slide-leave-to {
  opacity: 0;
}
.slide-enter-active {
  animation: slides 2s cubic-bezier(0.87, 0.36, 1, 0.23);
  transition: opacity 2s cubic-bezier(0.87, 0.36, 1, 0.23);
}
.slide-leave-active {
  animation: slides 2s reverse;
  transition: opacity 2s;
}

@keyframes slides {
  0% {
    transform: translateX(-100px);
  }
  100% {
    transform: translateX(0px);
  }
}

/*Entrada / Saida*/

.zoom-enter,
.zoom-leave-to {
  transform: scale(0);
}
.zoom-enter-active,
.zoom-leave-active {
  transition: transform 3s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 3s;
}
/*.fade-enter-to, .fade-leave {
  opacity: 1;
}*/
</style>
-->
