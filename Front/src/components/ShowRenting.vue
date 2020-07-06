<template>
  <div>
    <!-- VER ARTICULO DE FORMA INDIVIDUAL -->
      <div class="articulo" v-show="verArticulos">
         <h3>
          {{articulo.nombre_articulo}}
        </h3>
        <img :src="articulo.imagen" alt="imagen articulo">
       
        <p>
          {{articulo.descripcion}}
        </p>
        <p> Precio: 
          {{articulo.precio}}€/dia
        </p>
        <p :class="{
          green: articulo.disponibilidad === '1',
          red: articulo.disponibilidad === '0',
            }">
          Disponible {{articulo.fecha_fin | moment(" DD-MM-YYYY")}}
        </p>
        <br>
        <button @click="openModal()">Comprar</button> 
        <br>
        <div v-show="modal" class="modal">
          <div class="modalBox">
            <h2> ❓ Reserva producto</h2>
            <h3>Para finalizar tu reserva para alquilar un producto basta con añadir la fecha de inicio y
              la fecha final de la misma así como tu dirección.
            </h3>
            <h3>Cuando finalices la reserva recibirás un mail de confirmación.</h3>
            <h2> ❗️ Recuerda</h2>
            <h3>Una vez reserves tu producto podrás verlo en la seccion de <b>"Mi perfil >
              articulos adquiridos"</b> así como valorar tu experiencia. 
            </h3>
            <h3>Gracias por confiar en nosotros y esperamos hayas tenido buena experiencia  😀</h3>

            <form class="formCompra">
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
      </div>
<!-- VER TODAS LAS CAMARAS -->
    <div class="camaras"
          v-show="!verArticulos">  
          <h2>CAMARAS</h2>
      <div  
           v-for="(camara, index) in camaras" 
           :key="camara.id">
        <img :src="camara.imagen" alt="foto camara para alquilar" 
        @click="mostrarCamarasEvent(index)">
      </div>
    </div>
<!-- VER TODOS LAS CAMARAS -->
<!-- VER TODOS LOS DRONES -->
        <div 
          v-show="!verArticulos">  
          <h2>DRONES</h2>
      <div class="Drones" 
           v-for="(dron, index) in drones" 
           :key="dron.tittle">
        <img :src="dron.imagen" alt="foto dron para alquilar" 
        @click="mostrarDronesEvent(index)">
      </div>
    </div>
<!-- VER TODOS LOS DRONES -->
<!-- VER TODOS LOS ACCESORIOS-->
      <div 
          v-show="!verArticulos">  
          <h2>ACCESORIOS</h2>
      <div class="Accesorios" 
           v-for="(accesorio, index) in accesorios" 
           :key="accesorio.tittle">
        <img :src="accesorio.imagen" alt="foto accesorio para alquilar" 
        @click="mostrarAccesoriosEvent(index)">
      </div>
    </div>
    <br>
<button class="vol"><router-link :to="{ name:'Products'}"> Volver al menú</router-link></button>    
  </div>
</template>

<script>
import Swal from "sweetalert2";
import VModal from "vue-js-modal";
import formatDateToDB from "@/aux/helpers.js"
export default {
name: 'listaalquileres',
props:{
    camaras: Array,
    articulo: Object,
    drones: Array,
    accesorios: Array,
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
  mostrarCamarasEvent(index) {
      let data = this.camaras[index].id;
      this.$emit("go", data);
    },
  mostrarDronesEvent(index) {
      let data = this.drones[index].id;
      this.$emit("go", data);
    },
  mostrarAccesoriosEvent(index) {
      let data = this.accesorios[index].id;
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
.red {
  color: red;
}
.green {
  color: green;
}
div{
display: inline-block;
}
.modal {
position: fixed;
top: 5;
left: 0;
right: 0;
bottom: 0;
background: rgba(0, 0, 0, 0.5);
width: 100%;
padding-bottom: 2rem;
}

.modalBox {
background: black;
margin: 4.5rem;
padding:100px;
border: 1px solid rgb(48, 175, 97);
border-radius: 13px;
width: 80%;
}

.modalBox button{
  margin: 1rem;
}
.modalBox h2{
color: rgb(48, 175, 97);
}
.formCompra{
  background: rgba(172, 159, 147, 0.294);
  border-radius: 20px;
  padding: 2.5rem;
  margin: 0 30rem;
  border: 1px solid rgba(255, 255, 255, 0.616);
}
img {
  position: relative;
  text-align: center;
  display: block;
  transition: transform 700ms;
  border: 1px solid rgba(0, 0, 0, 0.226);
  border-radius: 5px;
  cursor: pointer
}
div img:focus,
div img:hover {
  transform: scale(1.3);
  z-index: 1;
}
.articulo{
  background: rgba(218, 218, 218, 0.192);
  padding: 1rem 10rem;
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.294);
}
.articulo h3{
  color: orange;
}
.articulo p{
  min-width: 10rem;
  max-width: 20rem;
}
.articulo img{
  display: flex;
  margin-left: 2rem;
}
h2{
  letter-spacing: 10px;
  text-align: left;
  margin-left: 6rem;  
  display: flex;
}

router-link{
  text-decoration: none;
}
a{
  color: white;
}
.vol{
  margin-top: 2rem;
}

</style>