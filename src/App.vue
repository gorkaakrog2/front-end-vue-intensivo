<script setup>
  import{ ref, onMounted } from 'vue'
  import Formulario from "./Formulario.vue"
  import Item from "./item.vue"

  let colores = ref([])

  onMounted(() => {
    fetch("https://backend-colores-intensivo.onrender.com/colores")
    .then(respuesta => respuesta.json())
    .then(respuestaProcesada => colores.value = respuestaProcesada)
  })

  function crearColor(color){
    fetch("https://backend-colores-intensivo.onrender.com/colores/nuevo", {
      method : "POST",
      body : JSON.stringify(color),
      headers : {
        "content-type" : "application/json"
      }
    })
    .then(respuesta => respuesta.json())
    .then(({ id, error }) => {
      if(!error){
        return colores.value.push({id,...color});
      }
      console.log("Erro al usuario");
    })
  }

  function borrarColor(id){
    console.log('Borraremos el --->' + id);
    //this.colores = this.colores.filter( color => color.id != id );
    fetch(`https://backend-colores-intensivo.onrender.com/colores/borrar/${id}`, {
      method : "DELETE"
    })
    .then(respuesta => respuesta.json())
    .then(({error}) => {
      if(!error){
        return colores.value = colores.value.filter( color => color.id != id );
      }
      console.log("Error al usuario");
    })
  }

  /*
  export default{
    data(){
      return {
        colores : []
      }
    },
    methods : {
      crearColor(color){
        fetch("http://localhost:4000/colores/nuevo", {
          method : "POST",
          body : JSON.stringify(color),
          headers : {
            "content-type" : "application/json"
          }
        })
        .then(respuesta => respuesta.json())
        .then(({ id, error }) => {
          if(!error){
            return this.colores.push({id,...color});
          }
          console.log("Erro al usuario");
        })
      },
      borrarColor(id){
        console.log('Borraremos el --->' + id);
        //this.colores = this.colores.filter( color => color.id != id );
        fetch(`http://localhost:4000/colores/borrar/${id}`, {
          method : "DELETE"
        })
        .then(respuesta => respuesta.json())
        .then(({error}) => {
          if(!error){
            return this.colores = this.colores.filter( color => color.id != id );
          }
          console.log("Error al usuario");
        })
      }
    },
    components : {
      Formulario,
      Item
    },
    mounted(){
      fetch("http://localhost:4000/colores")
      .then(respuesta => respuesta.json())
      .then(respuestaProcesada => this.colores = respuestaProcesada);
    }
  }
  */
</script>

<template>
  <!--<Formulario @prueba="x => console.log(x)"/>-->
  <Formulario @crear="crearColor"/>
  <ul>
    <Item v-for="({ id,r,g,b }) in colores" :id="id" :r="r" :g="g" :b="b" @borrar="borrarColor"/>
  </ul>
</template>