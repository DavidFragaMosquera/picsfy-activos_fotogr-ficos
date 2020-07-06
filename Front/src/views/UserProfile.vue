<template>
  <div>
<!-- CABECERA -->
    <vue-headful title="PerfilUsuario"/>
      <div>
        <div class="cab">
       <h2>Bienvenid@,  {{ userData.login }}!</h2>
       </div>
      <div class="welcome">
       <h3 class="recuerda">Este es tu area personal donde puedes: </h3>
       <h4>✔️  Editar tus datos</h4>
       <h4>✔️  Subir nuevos productos para vender o alquilar</h4>
       <h4>✔️  Ver los productos que has adquirido</h4>
       <h4>✔️  Cambiar tu contraseña</h4>
       <h4>✔️  Eliminar tu cuenta</h4> 
       <h3 class="recuerda">Recuerda que...</h3> 
       <h4>👉 Clickando en las opciones, estas se despliegan,</h4>  
       <h4>volviendo a clickar se vuelven a plegar 👈</h4>   
      </div>
<!-- LISTADO DATOS DE USUARIO -->
      <div>
        <ul class="datosperfil">
          <li> {{ userData.nombre }} {{ userData.apellidos }}</li>
          <li> <img class="imgPerfil" :src="userData.imagen" alt="foto perfil usuario" /></li>
          <li> 🔸 Correo <br> {{ userData.mail }}</li>
          <li> 🔸 Telefono <br>  {{ userData.telefono }}</li>
          <li> 🔸 Dirección <br> {{ userData.direccion }}</li>
          <li> 🔸 Sobre mi <br> {{ userData.descripcion }}</li>

        </ul>
<!-- BOTONES -->
      <div class="botonera">
        <button @click="showEditProfile()">Editar perfil</button>
        <button @click="showNewProduct()">Nuevo articulo</button>
        <button @click="showMyProducts()">Mis productos</button>
        <button @click="showBuyProducts()">Productos adquiridos</button>
        <button @click="showEditPassword()">Cambiar contraseña</button>
        <button @click="deleteUser()">Eliminar cuenta</button>
      </div>
<!-- CAMBIAR CONTRASEÑA -->
    <div class="editPassword" v-show="seeEditPassword">
        <h2 class="editPassword">Cambiar contraseña</h2>
        <form class="formEditPassword">
        <label for="oldpassword">Antigua Contraseña</label>
        <br>
        <input
          type="password"
          v-model="oldPassword"
          placeholder="Contraseña anterior"/>
        <br>
        <label for="password">Nueva contraseña</label>
        <br>
        <input type="password"
               v-model="password" 
              placeholder="Nueva contraseña" />
        <br>
        <label for="passwordRepeat">Repetir nueva contraseña</label>
        <br>
        <input
          type="password"
          v-model="passwordRepeat"
          placeholder="Repite nueva contraseña"/>
        </form>
        <button @click="editPassword()">Editar</button>
        <button class="passwordBack" @click="seeEditPassword = false">Volver</button>
    </div>
<!-- LISTADO ARTICULOS ADQUIRIDOS -->
        <div class="boughtProducts" v-show="!articulosComprados">
          <h2 class="tituloOpc">Productos adquiridos</h2>
          <br>
          <ul v-for="articuloAdquirido in articulosAdquiridos" :key="articuloAdquirido.id">
            <li><img :src="articuloAdquirido.imagen" alt="imagen articulo"></li>
            <li>{{ articuloAdquirido.nombre_articulo }}</li>
            <li>Precio: {{ articuloAdquirido.precio }} €</li>
            <li> Dirección de envio: {{ articuloAdquirido.direccion }}</li>
            <li>Fecha Inicio: {{ articuloAdquirido.fecha_inicio | moment(" DD-MM-YYYY" )}}</li>
            <li>Fecha Finalización: {{ articuloAdquirido.fecha_fin | moment(" DD-MM-YYYY" ) }}</li>
            <li>Pedido nº: {{ articuloAdquirido.id }}</li>
            <button @click="openModal(articuloAdquirido)">Valorar</button>
          </ul>
<!-- VALORAR PRODUCTO -->
              <div v-show="modal" class="modal">
                <div class="modalBox">
                    <h2>Valoración de producto</h2>
                    <h4>Puedes valorar tu producto para ayudar a otros usuarios.</h4>
                    <h4>Para ello basta que califiques el producto del 1 al 5 en función
                      de las estrellas que te mostramos a continuación ⭐️ </h4>                 
                    <star-rating
                      @rating-selected="rating = $event"
                      :rating="rating"
                      v-bind:star-size="33">
                    </star-rating>
                    <br>
                     <h4> Segidamente,puedes dejar un comentario para documentar mejor
                      tu experiencia. Gracias 😄</h4>
                      <br>
                    <textarea
                      v-model="comentario"
                      name="commentario"
                      id="commentario"
                      cols="40"
                      rows="10">                     
                    </textarea>
                    <br>
                    <button @click="ratingProduct(articuloValorado, rating, comentario)">Valorar producto</button>
                    <button @click="closeModal()">Volver</button>
                </div>
              </div>
        </div>
<!-- NUEVO ARTICULO -->
    <div class="nuevoArticulo" v-show="seeProduct">
      <h2>Sube un nuevo articulo</h2>
      <form class="formNewArt">
        <fieldset>
          <ul>
            <li>
              <label for="imagen">Subir imagen</label>
              <br>
              <input type="file" id="imagenArticulo" ref="imagenArticulo" @change="handleFileUploadArticulo()" />
            </li>
            <li>
              <label for="nombre_articulo">Nombre</label>
              <br/>
              <input type="text" name="nombre_articulo" id="nombre_articulo" placeholder="Escribe el nombre de tu producto" v-model="nombre_articulo" />
            </li>
            <li>
              <label for="tipo" >Categoria</label>
              <br/>
              <select
                name="tipo"
                id="tipo"
                v-model="tipo">
                <option value="" disabled selected>Selecciona una opción</option>
                <option value="cesion_derechos">Cesion Derechos Imagen</option>
                <option value="alquiler">Alquiler de equipos</option>
              </select>
            </li>

            <li>
              <label for="subtipo">Tipo</label>
              <br/>
              <select
                name="subtipo"
                id="subtipo"
                v-model="subtipo">
                <option value="" disabled selected>Selecciona una opción</option>
                <br>
                <option disabled selected>Cesión de derechos</option>
                <option value="ecommerce_producto">E-commerce y producto</option>
                <option value="bodas_eventos">Bodas y Eventos</option>
                <option value="retrato_naturaleza">Retrato y naturaleza</option>
                <br>
                <option disabled selected>Alquiler de equipos</option>
                <option value="camaras_reflex">Camaras reflex</option>
                <option value="drones">Drones</option>
                <option value="accesorios">Accesorios</option>
              </select>
            </li>

            <li>
              <label for="precio">Precio</label>
              <br/>
              <input
                type="number"
                name="precio"
                id="precio"
                placeholder="Precio en €"
                v-model="precio"/>
            </li>
            <br>
            <li>
            <label for="descripcion">Descripción</label>
            <br>
            <textarea 
            name="descripcion"
            id="descripcion"
            cols="20" 
            rows="10"
            placeholder="Escribe la descripción de tu articulo"
            v-model="descripcion">
            </textarea>
            </li>
          </ul>
        </fieldset>
      </form>
      <button @click="createProduct()">Subir producto</button>
      <button class="volverBot" @click="seeProduct = false">Volver</button>
    </div>
<!-- EDITAR ARTICULO -->
  <div class="editProduct" v-show="seeEditProduct">
      <h2>Editar producto</h2>
        <form class="formEditProduct">
        <label for="nombreArticulo">Nombre articulo</label>
        <br>
        <input type="text" v-model="newNombreArticulo" placeholder="Nombre articulo"/>
        <br>
        <label for="precio">Precio</label>
        <br>
        <input type="text" v-model="newPrecio" placeholder="Precio" />
        <br>
        <label for="descripcion">Descripción</label>
        <br>
        <textarea 
            name="descripcion"
            id="descripcion"
            cols="20" 
            rows="10"
            v-model="newDescripcion">
            </textarea>
        <br>
        <input type="file" id="imagenProducto" ref="imagenProducto" @change="handleFileUploadProducto()" />
        </form>
        <button @click="editProduct()">Modificar</button>
        <button class="editUserProduct" @click="seeEditProduct=false">Volver</button>
    </div>
<!-- LISTADO ARTICULOS DE USUARIO -->
    <div class="articulosUsuario" v-show="!showProducts">
      <h2 class="tituloOpc">Mis productos</h2>
        <br>
        <ul v-for="articulo in articulos" :key="articulo.id">
          <li><img :src="articulo.imagen" alt="imagen articulo"></li>
          <li>Nombre: {{ articulo.nombre_articulo }}</li>
          <li>Descripcion: {{ articulo.descripcion }}</li>
          <li>Precio: {{ articulo.precio }} €</li>
          <li>  {{ articulo.fecha_fin }}</li>
          <button @click="showProduct(articulo)">Editar</button>
          <button class="borrarArt" @click="deleteProduct(articulo)">Borrar</button>
        </ul>
   </div>

      </div>
    </div>
<!-- EDICION DATOS USUARIO -->
    <div class="editUs" v-show="showEdit">
      <h2>Editar perfil</h2>
      <form class="formEditUser">
      <br>
      <label for="imagen">Cambiar foto de perfil</label>
      <br>
      <input type="file" id="imagen" ref="imagen" @change="handleFileUpload()" />
      <br>
      <label for="nombre">Nombre</label>
      <br>
      <input type="text" v-model="newNombre" placeholder="Nombre"/>
      <br>
      <label for="apellidos">Apellidos</label>
      <br>
      <input type="text" v-model="newApellidos" placeholder="Apellidos"/>
      <br>
      <label for="mail">e-mail</label>
      <br>
      <input type="text" v-model="newMail" placeholder="Email"/>
      <br>
      <label for="telefono">Telefono</label>
      <br>
      <input type="text" v-model="newTelefono" placeholder="Telefono"/>
      <br>
      <label for="direccion">Dirección</label>
      <br>
      <input type="text" v-model="newDireccion" placeholder="Dirección"/>
      <br>
      <label for="">Biografia</label> <br>
      <textarea v-model="newDescripcion" placeholder="Biografia" rows="10"> </textarea>
      </form>
      <button @click="editUser()">Modificar</button>
      <button class="editUserImage" @click="showEdit=false">Volver</button>
    
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Swal from "sweetalert2";
import VModal from "vue-js-modal"
import StarRating from "vue-star-rating";

export default {
  name: "Profile",
  components: { StarRating },
  data() {
    return {
      id: null,
      userData: {},
      articulos:[],
      articulosAdquiridos:[],
      showEdit: false,
      showProducts: false,
      articulosComprados: false,
      seeProduct: false,
      seeEditProduct: false,
      seeEditPassword: false,
      require: false,
      modal: false,
      newDescripcion: "",
      newNombre: "",
      newApellidos: "",
      fecha_namiento:"",
      newFechaNacimiento: "null",
      newMail: "",
      newTelefono: "",
      newDireccion: "",
      newNombreArticulo: "",
      newPrecio: "",
      newDescripcion: "",
      nombre_articulo:"",
      precio:"",
      tipo:"",
      subtipo:"",
      descripcion:"",
      imagen:"", 
      imagenProducto:"",
      imagenArticulo:"",
      rating: 0,
      comentario:"",
      oldPassword: "",
      password: "",
      passwordRepeat: "",
      articuloValorado:""
    };
  },
  methods: {
// OBTENCION DATOS DEL USUARIO
    getUserData() {
      const self = this;
      const data = localStorage.getItem("id");
      const token = localStorage.getItem("token");
      axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;    
        axios
          .get("http://localhost:3000/user/" + data)
          .then(function(response) {
            self.userData = response.data.data;
            if(!!self.userData.imagen) {
              self.userData.imagen = "http://localhost:3000/uploads/" + self.userData.imagen;    
            } else {
              self.userData.imagen = "http://localhost:3000/uploads/avatar.jpg";
            }
          })
          .catch(function(error) {
             Swal.fire({
            icon: 'error',
            title: 'Oops...',
            text: 'No tienes acceso, logueate!',
            timer: 2000
        });
        this.$router.push("/");
          });    
    },
// OBTENCION PRODUCTOS DEL USUARIO
     getUserProducts(){
      const self = this;
      const data = localStorage.getItem("id");
      const token = localStorage.getItem("token");
      axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;    
        axios
          .get("http://localhost:3000/user/products/" + data)
          .then(function(response) {
          self.showProducts = true;
          self.articulos = response.data.data.map((articulo) => {
            articulo.imagen = "http://localhost:3000/uploads/" + articulo.imagen;
            return articulo
          });
          })
          .catch(function(error) {
            console.error(error.response.data.message)
          });
       },
      showMyProducts(){
        this.showProducts = !this.showProducts;
      },
// EDICION PERFIL DEL USUARIO
    handleFileUpload() {
      this.imagen = this.$refs.imagen.files[0];
    },
    editUser() {
      const self = this;
      const data = localStorage.getItem("id");
      const token = localStorage.getItem("token");
      let formData = new FormData();
      formData.append("imagen", self.imagen);
      formData.append("descripcion", self.newDescripcion);
      formData.append("nombre", self.newNombre);
      formData.append("apellidos", self.newApellidos);
      formData.append("mail", self.newMail);
      formData.append("telefono", self.newTelefono);
      formData.append("direccion", self.newDireccion);
      axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;
      axios
        .put("http://localhost:3000/user/" + data, formData, {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        })
        .then(function(response) {
          self.showEdit = true;
          Swal.fire({
            icon: "success",
            title: "Usuario editado correctamente, gracias!",
            timer: "3000"
          });
        location.reload();
        })
        .catch(function(error) {
          console.error(error.response.data.message);
        });
    },
    showEditProfile() {
      this.newDescripcion = this.userData.descripcion;
      this.newNombre = this.userData.nombre;
      this.newApellidos = this.userData.apellidos;
      this.newMail = this.userData.mail;
      this.newTelefono = this.userData.telefono;
      this.newDireccion = this.userData.direccion;
      this.showEdit = true;
    }, 
// CAMBIAR CONTRASEÑA
    editPassword() {
      const self = this;
      const data = localStorage.getItem("id");
      const token = localStorage.getItem("token");
      axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;
      axios
        .put("http://localhost:3000/user/password/" + data, {
          oldPassword: self.oldPassword,
          newPassword: self.password,
          newPasswordRepeat: self.passwordRepeat,
        })
        .then(function(response) {
          Swal.fire({
            icon: "success",
            title: "Contraseña cambiada con exito!",
            timer: "3000"
          });
          self.emptyPassword();
          self.seeEditPassword = true;
        })
        .catch(function(error) {
          console.error(error.response.data.message);
          Swal.fire({
            icon: 'error',
            title: 'Oops...',
            text: 'Puede que tu contraseña antigua no sea la correcta o que no coincidan las contraseñas',
            timer: 3333
          });
        });
    },
    showEditPassword() {
      this.seeEditPassword = true;
      this.password = "";
      this.passwordRepeat = "";
    },
    emptyPassword() {
      this.oldPassword = "";
      this.newPassword = "";
      this.newPasswordRepeat = "";
    },
// CREAR NUEVO PRODUCTO
    handleFileUploadArticulo() {
      this.imagenArticulo = this.$refs.imagenArticulo.files[0]
    },
    createProduct(){
      this.validatingProduct();
      const self = this;
      const token = localStorage.getItem("token");
      let formData = new FormData();
      formData.append("imagen", self.imagenArticulo);
      formData.append("nombre_articulo", self.nombre_articulo);
      formData.append("descripcion", self.descripcion);
      formData.append("precio", self.precio);
      formData.append("tipo", self.tipo);
      formData.append("subtipo", self.subtipo);
      axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;
      axios
        .post("http://localhost:3000/products" , formData, {
          nombre_articulo: self.nombre_articulo,
          descripcion: self.descripcion,
          precio: self.precio,
          tipo: self.tipo,
          subtipo: self.subtipo,
          imagen: self.imagenArticulo
        }, {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        })
        .then(function(response) {
          Swal.fire({
            icon: "success",
            title: "Articulo creado con exito, gracias!",
            timer: "3000"
          });
          self.emptyProduct();
          self.seeProduct = false;
        })
        .catch(function(error) {
          console.error(error.response.data.message);
        });
    },
    validatingProduct() {
      if (
        this.nombre_articulo === "" ||
        this.descripcion === "" ||
        this.tipo === "" ||
        this.subtipo === "" ||
        this.precio === ""
      ) {
        this.correctData = false;
        this.require = true;
      } else {
        this.correctData = true;
        this.require = false;
      }
    },
    emptyProduct() {
      this.nombre_articulo = "";
      this.descripcion = "";
      this.tipo = "";
      this.subtipo = "";
      this.precio = "";
    },
    showNewProduct() {
      this.seeProduct = true;
    },
// EDITAR PRODUCTO
    handleFileUploadProducto() {
      this.imagenProducto = this.$refs.imagenProducto.files[0];
    },

    editProduct() {      
      const self = this;
      const id = self.id;
      const idUser = localStorage.getItem("id");
      const token = localStorage.getItem("token");
      let formData = new FormData();
      formData.append("imagen", self.imagenProducto);
      formData.append("nombre_articulo", self.newNombreArticulo);
      formData.append("precio", self.newPrecio);
      formData.append("descripcion", self.newDescripcion);
      axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;
      axios
        .put("http://localhost:3000/products/" + id, formData, {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        })
        .then(function(response) {
          self.seeEditProduct = false;
          Swal.fire({
            icon: "success",
            title: "Tu articulo se ha modificado correctamente",
            timer: "3000"
          });
          location.reload();
        })
        .catch(function(error) {
          console.error(error.response.data.message);
        });
    },
    showProduct(articulo) {
      this.id = articulo.id
      this.newNombreArticulo = articulo.nombre_articulo;
      this.newPrecio = articulo.precio;
      this.newDescripcion = articulo.descripcion;
      this.seeEditProduct = true;
    },
// BORRAR PRODUCTO
 deleteProduct(articulo) {
      const self = this;
      const id = articulo.id;
      const idUser = localStorage.getItem("id");
      const token = localStorage.getItem("token");
      axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;
      Swal.fire({
        title: "Estás seguro?",
        text: "Si lo borras no podrás deshacer cambios",
        icon: "warning",
        showCancelButton: true,
        cancelButtonColor: "#d33",
        confirmButtonText: "Si, borrar",
      }).then((result) => {
        if (result.value) {
          axios
            .delete ("http://localhost:3000/products/" + id)
            .then(function(response) {
              location.reload();
            })
            .catch(function(error) {
              console.error(error.response.data.message);
            });
          Swal.fire({
            title: "Articulo borrado",
            icon: "success",
            confirmButtonText: "OK",
            timer: "2000"
          });
        }
      });
    },
// MOSTRAR ARTICULOS ADQUIRIDOS
    getBuyProducts(){
      const self = this;
      const data = localStorage.getItem("id");
      const token = localStorage.getItem("token");
      axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;    
        axios
          .get("http://localhost:3000/user/products/acquired/" + data)
          .then(function(response) {
          self.articulosComprados = true;
          self.articulosAdquiridos = response.data.data.map((articuloAdquirido) => {
            articuloAdquirido.imagen = "http://localhost:3000/uploads/" + articuloAdquirido.imagen;
            return articuloAdquirido
          });
          })
          .catch(function(error) {
            console.error(error.response.data.message)
          });
       },
      showBuyProducts(){
        this.articulosComprados = !this.articulosComprados;
    },
// VALORAR PRODUCTO ADQUIRIDO
    ratingProduct(articuloAdquirido, rating, comentario) {
      self = this;
      const id = articuloAdquirido.id;
      const token = localStorage.getItem("token");
      const data = localStorage.getItem("id");
      axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;
      axios
      .post("http://localhost:3000/pedidos/rating/" + id, {
        valoracion: rating,
        comentario: comentario
      })
      .then(function(response) {
        Swal.fire({
            icon: "success",
            title: "Articulo valorado correctamente",
            timer: "4000"
        });
          location.reload();
      })
      .catch(function(error){
        console.error(error.response.data.message);
        Swal.fire({
            icon: 'error',
            title: 'Oops...',
            text: 'Ya has valorado el producto y solo puedes hacerlo una vez',
            timer: 3000
        });
      })
    },
    openModal(articuloAdquirido) {
      this.articuloValorado = articuloAdquirido;
      this.modal = true;
    },
    closeModal() {
      this.modal = false;
    },
// ELIMINAR CUENTA
  deleteUser() {
      const self = this;
      const data = localStorage.getItem("id");
      const token = localStorage.getItem("token");
      axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;
      Swal.fire({
        icon: "warning",
        title: "¿Eliminar cuenta?",
        text: "Si confirmas tu cuenta se eliminará",
        cancelButtonColor: "#d33",
        showCancelButton: true,
        confirmButtonText: "Si, borrar cuenta",
      }).then((result) => {
        if (result.value) {
          axios
            .put("http://localhost:3000/user/disable/" + data)
            .then(function(response) {
              localStorage.removeItem("token");
              localStorage.removeItem("login");
              localStorage.removeItem("id");
              axios.defaults.headers.common["Authorization"] = "";
              self.$router.push("/");
            })
            .catch(function(error) {
              console.error(error.response.data.message);
            });
          Swal.fire({
            title: "Cuenta eliminada",
            text: "No te preocupes, podrás reactivarla siempre que quieras",
            timer: "3333"
          })
        }
      });
    },

  },
  created() {
    this.getUserData();
    this.getUserProducts();
    this.getBuyProducts();
  },
};
</script>

<style scoped>

.modal {
  position: relative;
  top: -65rem;
  left: 0;
  right: 50rem;
  padding: 5rem;
  bottom: 0; 
  width: 100%;
  background: rgb(0, 0, 0);
}
.modalBox {
  margin-left: 35rem;
  padding: 0rem 5rem;
  width: 20%;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  border-radius: 2%;
  align-items: center;
  background: rgba(172, 159, 147, 0.294);
  border: 1px solid rgba(255, 255, 255, 0.616);
  box-shadow: 0px 8px 6px -2px rgba(78, 90, 100, 0.671); 

}
.modalBox button{
  margin: 1rem;
}

.botonera {
  padding-top: 3rem;
  text-align: left;
  margin-left: 6rem;
  
}
.botonera button{
  margin: 1rem;
  box-shadow:
    0 2.8px 2.2px rgba(0, 0, 0, 0.034),
    0 6.7px 5.3px rgba(0, 0, 0, 0.048),
    0 12.5px 10px rgba(0, 0, 0, 0.06),
    0 22.3px 17.9px rgba(0, 0, 0, 0.072),
    0 41.8px 33.4px rgba(0, 0, 0, 0.086),
    0 100px 80px rgba(0, 0, 0, 0.12);
  font-size: 1rem;
  cursor: pointer;
}
.imgPerfil {
  border-radius: 50%;
  margin: -28rem 10rem;
  transition: transform .9s ease;
  box-shadow:
    0 2.8px 2.2px rgba(0, 0, 0, 0.034),
    0 6.7px 5.3px rgba(0, 0, 0, 0.048),
    0 12.5px 10px rgba(0, 0, 0, 0.06),
    0 22.3px 17.9px rgba(0, 0, 0, 0.072),
    0 41.8px 33.4px rgba(0, 0, 0, 0.086),
    0 100px 80px rgba(0, 0, 0, 0.12);
}
.imgPerfil:hover {
  position: relative;
  transform: scale(1.1);
  border-radius: 50%;
  box-shadow: 0px 8px 6px -6px rgba(78, 90, 100, 0.479); 
}
.datosperfil{
  position: relative;
  background: rgba(37, 33, 37, 0.253);
  margin: -26.5rem 0 0 42rem;
  padding: 2rem 2rem 14rem 1px;
  text-align: center;
  border-radius: 9px;
  font-size: 1.5rem;
  box-shadow: 0px 8px 6px -6px rgba(2, 3, 3, 0.685); 
}
.datosperfil li:first-child{
  color: rgb(226, 135, 16);
  font-weight: bold;
}
.welcome {
  position: relative;
  text-align: left;
  background: rgba(116, 110, 110, 0.191);
  border-radius: 0.5rem;
  padding: 1rem 5rem;
  margin: -1rem 76rem;
  margin-left: 5rem;
  box-shadow: 0px 8px 6px -6px rgba(2, 3, 3, 0.685); 
}
.recuerda{
    color: rgb(48, 175, 97);
    text-align: center;
}
h2{
  color: rgb(48, 175, 97);
  text-align: center;  
}
.formEditUser, .formNewArt {
  background: rgba(116, 110, 110, 0.191);
  margin: 0 40rem;
  padding-bottom: 2rem;
  border-radius: 2%;
}
 .formEditPassword, .formEditProduct {
  background: rgba(116, 110, 110, 0.191);
}
.editUs input, .editProduct input {
  margin-bottom: 1rem;
}
.editUs h3, .boughtProducts h3{
    color: rgb(48, 175, 97);
}
.nuevoArticulo li {
  margin-bottom: 1rem ;
  margin-right: 1.3rem;
}
.nuevoArticulo button{
  box-shadow: 0px 8px 6px -6px rgba(78, 90, 100, 0.479); 
}
.volverBot, .editUserImage, .borrarArt, .editUserProduct, .passwordBack{
  background: rgba(0, 0, 0, 0.219);
  margin-left: 1rem;
  margin-top: 1rem;
  box-shadow: 0px 8px 6px -6px rgba(78, 90, 100, 0.479); 
}
.articulosUsuario, .boughtProducts {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem;
  justify-content: center;
  flex-grow: initial;
}
.articulosUsuario li, .boughtProducts li, .editPassword input{
  margin-bottom: 1rem;
}

</style>