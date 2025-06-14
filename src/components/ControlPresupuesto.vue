<script setup>
import { computed } from 'vue'
import CircleProgress from 'vue3-circle-progress'
import 'vue3-circle-progress/dist/circle-progress.css'
import { formatearCantidad } from '../helpers/index'

const props = defineProps({
    presupuesto: {
        type: Number,
        required: true
    },
    disponible: {
        type: Number,
        required: true
    },
    gastado: {
        type: Number,
        required: true
    }
})
defineEmits(['reset-app'])
const porcentaje = computed(() => {
    return parseInt(((props.presupuesto - props.disponible) / props.presupuesto) * 100)
})
</script>
<template>
  <div class="dos-columnas">
    <div class="contenedor-grafico">
        <p class="porcentaje">{{ porcentaje }}%</p>
      <CircleProgress
      :percent="porcentaje"
      :size="250"
      :border-width="30"
      :border-bg-width="30"
      fill-color="black"
      empty-color="gray"
      transition="500"
      viewport="true"
      :is-gradient="true"
      :gradient="{
        angle: 90,
        startColor: '#090979',
        stopColor: '#00D4FF'
      }"
      :is-shadow="true"
      />
    </div>
    <div class="contenedor-presupuesto">
        <button
        class="reset-app"
        type="button" 
        @click="$emit('reset-app')"
        >Reset App</button>

        <p>
            <span>Presupuesto:</span>
            {{ formatearCantidad(presupuesto) }}
        </p>
        <p>
            <span>Disponible:</span>
            {{ formatearCantidad(disponible) }}
        </p>
        <p>
            <span>Gastado:</span>
            {{ formatearCantidad(gastado) }}
        </p>
      
    </div>
  </div>
</template>



<style scoped>
.contenedor-grafico{
    position: relative;
}
.porcentaje{
    font-size: 3rem;
    margin: auto;
    font-weight: 900;
    color: var(--gris-oscuro);
    text-align: center;
    position: absolute;
    top: calc(50% - 1.5rem);
    left: 0;
    right: 0;
    z-index: 100;
}

.reset-app{
    width: 100%;
    background-color: #db2777;
    color: var(--blanco);
    padding: 1rem;
    text-transform: uppercase;
    border-radius: 1rem;
    font-weight: 900;
    cursor: pointer;
    transition: all .3s ease;
}
.reset-app:hover{
    background-color: #b91c1cce;
    cursor: pointer;
}
    .dos-columnas{
        display: flex;
        flex-direction: column;
        
    }
    .dos-columnas > :first-child{
        margin-bottom: 3rem;
        gap: 4rem;
        align-items: center;
    }
    @media (min-width: 768px){
        .dos-columnas{
            flex-direction: row;
            gap:4rem;
            align-items: center;
        }
        .dos-columnas > :first-child{
          margin-bottom: 0;  
        }
    }
    .contenedor-presupuesto{
        width: 100%;
        
    }
    .contenedor-presupuesto p{
        font-size: 2.4rem;
        text-align: center;
        color: var(--gris-oscuro);
        
    }
    @media (min-width: 768px){
        .contenedor-presupuesto p{
            text-align: left;

            
        }
    }
    .contenedor-presupuesto span{
        font-weight: 900;
        color: var(--azul);
    }
</style>
