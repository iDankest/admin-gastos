<script setup>
import Imagen from '../assets/img/grafico.jpg'
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
})
</script>
<template>
  <div class="dos-columnas">
    <div class="contenedor-grafico">
      <img :src="Imagen" alt="Grafico">
    </div>
    <div class="contenedor-presupuesto">
        <button
        class="reset-app"
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
            0€
        </p>
      
    </div>
  </div>
</template>



<style scoped>

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
