<script setup>
import { ref } from 'vue'

let textoTemporal = ref('')
let error = ref('')

const emit = defineEmits(["crear"])

function validar(){
    error.value = "";
    let valido = /^([0-9]{1,3},){2}[0-9]{1,3}$/.test(textoTemporal.value);
    if(valido){
        let [r,g,b] = textoTemporal.value.split(",").map(n => +n);
        [r,g,b].forEach(n => valido = valido && n<=255);
        if(valido){
            emit('crear',{r,g,b});
            return textoTemporal.value = '';
        }

        return error.value = 'Deben ser numeros de 0 a 255';
    }

    error.value = 'No pueden estar en blanco';
}

/*
export default {
    setup(props,contexto){
        let textoTemporal = ref('')
        let error = ref('')

        function validar(){
            error.value = "";
            let valido = /^([0-9]{1,3},){2}[0-9]{1,3}$/.test(textoTemporal.value);
            if(valido){
                let [r,g,b] = textoTemporal.value.split(",").map(n => +n);
                [r,g,b].forEach(n => valido = valido && n<=255);
                if(valido){
                    contexto.emit('crear',{r,g,b});
                    return textoTemporal.value = '';
                    
                }

                return error.value = 'Deben ser numeros de 0 a 255';
            }

            error.value = 'No pueden estar en blanco';
        }

        return {
            textoTemporal,
            error,
            validar
        }
    }

}
*/
</script>

<template>
    <!-- <form @submit.prevent="$emit('prueba','hola soy un dato')">--> <!-- primer parametro es el evento, segundo los datos uqe se envian -->
    <form @submit.prevent="validar"> 
        <input type="text" placeholder="rrr,ggg,bbb" v-model="textoTemporal">
        <p class="error" v-show="error">{{ error }}</p>
        <input type="submit" value="crear color">
    </form>
</template>