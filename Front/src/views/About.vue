<template>
  <div class="about">
    <h1>SOBRE MI</h1>
    <p>yo</p>
    <div class="sobreMi">
    <h2>👨‍💻 David Fraga Mosquera </h2>
    <h3> 👉 Primer proyecto como estudiante de programación full-stack </h3>
    <h3> 👉 Reciclaje continuo, nuevos retos y ganas de superarme día a día </h3>
    <h3> 👉 Putoloco de la tecnología gracias al poder de creación de la misma</h3>
    <h3> 🌍 Mis grandes pasiones son la fotografía, la naturaleza y viajar para conocer nuevas culturas</h3>
    </div>
    <div class="tecno">
      <h2>Tecnologías usadas</h2>
      <h3>✔️ VueJs </h3>
      <h3>✔️ NodeJs</h3>
      <h3>✔️ MySQL</h3>
      <h3>✔️ Express </h3>
      <h3>✔️ Axios</h3>
      <h3>✔️ BodyParser</h3>
      <h3>✔️ HapiJoi</h3>
      <h3>✔️ Nodemon</h3>
      <h3>✔️ Moment</h3>
      <h3>✔️ Vue Headful</h3>
      <h3>✔️ Star Rating</h3>
      <h3>✔️ Sweet Aletrt </h3>
      <h3>✔️ SendGrid</h3>
    </div>
    <div class="dedicaciones">
      <h3>Especial dedicación a todos mis compañeros,
        a los que siempre estaban ahí día tras día para rompernos la cabeza, reir
        y aprender unos de otros. Por la fatiga visual y por comernos el futuro 🔥
      </h3>
      <h3> Felix, Raúl, Isa, Brais, Berto y Victor. </h3>
      <h2 class="thx">GRACIAS</h2>
    </div>
    <div class="fin"> 
      <h3>Si quieres saber más sobre mi, puedes visitarme y mandar un mensaje <a href="https://es.linkedin.com/in/david-fraga-mosquera-07b73888">clickando aquí</a></h3>
    </div>

  </div>
</template>
<style scoped>
 h1{
   letter-spacing: 20px;
   text-decoration: overline;
   color: rgb(48, 175, 97);
 }
 h2{
   color: orange;
 }
 p{
  background-image: url('../assets/yo2.jpg');
  font-size: 0;
  background-position: center;
  background-size: 55%;
  background-repeat: no-repeat;
  width: 50rem;
  height: 60rem;
  margin-top: -10rem;
  margin-left: -5rem;
  transition:  all .5s;
  position: relative;
  margin-bottom: 1rem;
  border-radius: 5px;
 }
 p:hover{
  border-radius: 5px;
  background-size: 60%;
  transition: all 0.7s;
  opacity: 0.8; 
}
.sobreMi{
  text-align: left;
  position: absolute; 
  visibility: visible; 
  left: 595px; 
  top: 280px; 
  z-index: 200;
  font-weight: bold;
  background: rgba(104, 103, 103, 0.233);
  padding: 2.5rem;
  border-radius: 20px;
  box-shadow: 0px 8px 6px -6px rgba(78, 73, 98, 0.986); 
}
.tecno{
  text-align: left;
  position: absolute; 
  visibility: visible; 
  left: 1500px; 
  top: 280px; 
  z-index: 200;
  font-weight: bold;
  background: rgba(104, 103, 103, 0.233);
  padding: 2.5rem;
  border-radius: 20px;
  box-shadow: 0px 8px 6px -6px rgba(78, 73, 98, 0.986); 
}
.dedicaciones{
  text-align: left;
  position: absolute; 
  visibility: visible; 
  left: 650px; 
  top: 680px; 
  z-index: 200;
  margin-right: 28rem;
  font-weight: bold;
  background: rgba(104, 103, 103, 0.233);
  padding: 2.5rem;
  border-radius: 20px;
  box-shadow: 0px 8px 6px -6px rgba(78, 73, 98, 0.986); 
}
.thx{
  text-align: center;
  letter-spacing: 10px;
}
.fin{
  margin-top: -5rem;
}
</style>
