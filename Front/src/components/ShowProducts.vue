<template>
  <div class="todo">
    <!-- VER ARTICULOS DE FORMA INDIVIDUAL -->
    <div class="articulo" v-show="verArticulos">
         <h3>
          {{articulo.nombre_articulo}}
        </h3>
        <img :src="articulo.imagen" alt="imagen articulo">
       
        <p>
          {{articulo.descripcion}}
        </p>
        <p> Precio: 
          {{articulo.precio}}€/día
        </p>
        <p>
          Disponible {{articulo.fecha_fin | moment(" DD-MM-YYYY")}}
        </p>
        <br>
        <button @click="openModal()">Comprar</button>
        <br>
<!-- COMPRA -->
          <div v-show="modal" class="modal">
          <div class="modalBox">
            <form>
            <h3>Rellena los datos para finalizar tu pedido, gracias</h3>
                 <label for="fecha_inicio"> Fecha Inicio:</label>
                 <br>
                 <input type="date" id="fecha_inicio" name="fecha_inicio" v-model="datosCompra.fecha_inicio" />
                 <br>
                 <label for="fecha_fin"> Fecha Fin:</label>
                 <br>
                 <input type="date" id="fecha_fin" name="fecha_fin" v-model="datosCompra.fecha_fin" />
                 <br>
                 <label for="direccion"> Dirección:</label>
                 <br>
                 <input type="text" id="direccion" name="direccion" v-model="datosCompra.direccion" />
                 <br>
                 </form>
                 <button @click="buyProductEvent(articulo)">Comprar</button>
                 <button class="voltar" @click="closeModal()">Cerrar</button>
          </div>
        </div>
        <br>
        <button @click="verArticuloEvent">Volver al menu</button>
      </div>
      <!-- VER TODOS LOS ARTICULOS -->
    <div class="articulos"
          v-show="!verArticulos">  
          <h2>Todos los articulos</h2>
      <div class="Allproducts" 
           v-for="(articulo, index) in articulos" 
           :key="articulo.id">
        <img :src="articulo.imagen" alt="foto articulo" 
        @click="mostrarArticuloEvent(index)">
      </div>
    </div>
    <!-- VER CESION DE IMAGENES -->
      <div 
          v-show="!verArticulos">  
        <h2><router-link :to="{ name:'Cesion'}"> Cesion derechos de imagen</router-link></h2>
      <div class="Cesiones" 
           v-for="(cesion, index) in cesiones" 
           :key="cesion.tittle">
        <img :src="cesion.imagen" alt="foto alquiler de equipos" 
        @click="mostrarCesionEvent(index)">
      </div>
    </div>
    <!-- VER ALQUILERES DE PRODUCTOS -->
      <div 
          v-show="!verArticulos">  
        <h2><router-link :to="{ name:'Renting'}"> Alquiler de equipos</router-link></h2>
      <div class="Alquileres" 
           v-for="(alquiler, index) in alquileres" 
           :key="alquiler.tittle">
        <img :src="alquiler.imagen" alt="foto alquiler de equipos" 
        @click="mostrarAlquilerEvent(index)">
      </div>
    </div>
  </div>
</template>

<script>
import Swal from "sweetalert2";
import VModal from "vue-js-modal";
import formatDateToDB from "@/aux/helpers.js"

export default {
name: 'listaproductos', 

props:{
    articulos: Array,
    articulo: Object,
    cesiones: Array,
    alquileres: Array,
    verArticulos: Boolean,
    id: Number,
    comprar: Array, 
    datosCompra: Object, 
    
},

data(){
  return {
    modal:false,
  }
},

methods: {
  mostrarArticuloEvent(index) {  
      let data = this.articulos[index].id;
      this.$emit("go", data);
    },
  mostrarCesionEvent(index) {  
      let data = this.cesiones[index].id;
      this.$emit("go", data);
    },
  mostrarAlquilerEvent(index) {  
      let data = this.alquileres[index].id;
      this.$emit("go", data);
    },
  verArticuloEvent() {
      this.$emit("verArticulo");
    },
  openModal() {
    this.modal = true;

  },
  closeModal(){
    this.modal = false;

},
  buyProductEvent(articulo) {
    let data = articulo.id
    this.$emit("comprar", data);
  }
}
}
</script>

<style scoped>
div{
display: inline-block;
}
.modal {
position: fixed;
top: 5;
left: 0;
bottom: 0;
background: rgba(0, 0, 0, 0.5);
width: 100%;
padding-bottom: 2rem;
}

.modalBox {
background: black;
margin: 15% auto;
padding:100px;
border: 1px solid rgb(48, 175, 97);
border-radius: 13px;
width: 80%;
}

.modalBox button{
  margin: 1rem;
}


img {
  position: relative;
  text-align: left;
  display: block;
  transition: transform 700ms;
}

div img:focus,
div img:hover {
  transform: scale(1.3);
  z-index: 1;
}

h2{
  color: rgb(48, 175, 97);
  text-align: left;
  margin-left: 6rem;  
}

router-link{
  text-decoration: none;
}

</style>