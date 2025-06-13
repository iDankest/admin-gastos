<script setup>
import { formatearCantidad, formatearFecha } from '../helpers/index'
const props = defineProps({
    gasto: {
        type: Object,
        required: true
    }
})
import IconoAhorro from '../assets/img/icono_ahorro.svg'
    import IconoCasa from '../assets/img/icono_casa.svg'
    import IconoComida from '../assets/img/icono_comida.svg'
    import IconoGastos from '../assets/img/icono_gastos.svg'
    import IconoOcio from '../assets/img/icono_ocio.svg'
    import IconoSalud from '../assets/img/icono_salud.svg'
    import IconoSuscripciones from '../assets/img/icono_suscripciones.svg'
    const diccionarioIconos = {
        ahorro : IconoAhorro,
        comida : IconoComida,
        casa : IconoCasa,
        gastos : IconoGastos,
        ocio : IconoOcio,
        salud : IconoSalud,
        suscripciones : IconoSuscripciones
    }
</script>

<template>
    <div class="gasto sombra">
        <div class="contenido">
            <img class="icono" :src="diccionarioIconos[gasto.categoria]" alt="Icono gasto">
            <div class="detalles">
                <p class="categoria">{{ gasto.categoria }}</p>
                <p class="nombre">{{ gasto.nombre }}</p>

                <p class="fecha">
                    fecha: <span>{{ formatearFecha(gasto.fecha) }}</span></p>
            </div>
        </div>
        <p class="cantidad">{{ formatearCantidad(gasto.cantidad) }}</p>
    </div>
</template>

<style scoped>
.gasto{
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 2rem;
}
.contenido{
display: flex;
align-items: center;
gap: 2rem;
}
.icono{
    width: 5rem;
}
.detalles{
    
}
.detalles p{
    margin: 0 0 1rem 0;
}
.categoria{
    font-weight: 900;
    color: var(--gris); 
    text-transform: uppercase;
    font-size: 1.2rem;
}
.nombre{
    color: var(--gris-oscuro);
    font-weight: 700;
    font-size: 2.4rem;
    cursor: pointer;
}
.fecha{
    color: var(--gris-oscuro);
    font-weight: 900;
    font-size: 1.6rem;
}
.fecha span{
    font-weight: 400;
}
.cantidad{
    font-weight: 900;
    font-size: 3rem;
    margin: 0;
}
</style>
