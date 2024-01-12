<template>
  <div class="container">
    <div class="score">
      <label for="">Puntaje: </label>
      <label for="">{{ puntaje }} </label>
      <label for="">Intento: </label>
      <label for="">{{ intento }} </label>
    </div>
    <div class="recursos">
      <img v-for="(url, index) in imagenes" :key="index" :src="url" alt="" />
      <label v-for="(texto, index) in textos" :key="index">{{ texto }}</label>
    </div>

    <button v-on:click="resultados()">JUGAR</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      puntaje: 0,
      intento: 0,
      imagenes: [
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg",
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg",
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg",
      ],
      textos: ["XXXXXXXXXX", "XXXXXXXXXX", "XXXXXXXXXX"],
      historial: [],  
    };
  },
  methods: {
    async jugar() {
      const { answer, image } = await fetch("https://yesno.wtf/api").then(
        (respuesta) => respuesta.json()
      );
      this.historial.push({ texto: answer, imagen: image });
      if (this.historial.length > 10) {
        this.historial.shift(); 
      }
    },
    resultados() {
      this.jugar();
      for (let i in this.historial) {
        this.textos[i] = this.historial[i].texto;
        this.imagenes[i] = this.historial[i].imagen;
      }
      
    },
  },
};
</script>

<style>
.container {
  display: grid;
  justify-content: center;
  align-items: center;
}

.score {
  display: grid;
  align-items: center;
  grid-template-columns: repeat(4, 60px);
}
.recursos {
  display: grid;
  grid-template-columns: repeat(3, 300px);
}
img {
  width: 200px;
}
</style>