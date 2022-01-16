<template>
  <div>
    <input type="button" value="click" v-on:click="getData()"/>
    <div class="container">
      <div v-for="img, i in images" :key="img.name">
        <img :src="img.pathLong"/>
        <div>
          <label>Image name:</label>
          <input v-model="images[i].name"/>
        </div>
        <div>
          <label>Brand:</label>
          <input v-model="images[i].brand"/>
        </div>
        <div>
          <label>Words:</label>
          <input v-model="images[i].words"/>
        </div>
        <div>
          <label>Colors:</label>
          <input v-model="images[i].colors"/>
        </div>
        <div>
          <label>Figures:</label>
          <input v-model="images[i].figures"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      images: [],
      plate: "plate1",
      items: []
    }
  },

  mounted() {
    this.importAll(require.context('../assets/images/plate1/', true, /\.jpg$/));
  },

  methods: {
    importAll(r) {
      r.keys().forEach(key => (this.images.push({ pathLong: r(key), name: key.replace('./', '') })));
    },
    getData() {
      let result = [];

      this.images.forEach((item) => {
        result.push({
          imageName: item.name,
          brand: item.brand,
          words: item.words ? item.words.split(' ') : [],
          colors: item.colors ? item.colors.split(' ') : [],
          figures: item.figures ? item.figures.split(' ') : []
        });
      });

      console.log(result);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  display: flex;
  flex-wrap: wrap;
}

.container img {
  height: 300px;
  width: 300px;
  padding: 10px;
  display: block;
}
</style>
