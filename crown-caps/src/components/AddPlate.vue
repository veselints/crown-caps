<template>
  <div>
    test
    <input type="button" value="click" v-on:click="getData()"/>
    <div class="container">
      <div v-for="img in images" :key="img.name">
        <img :src="img.pathLong"/>
        <div>
          <span>{{img.name}}</span>
        </div>
        <div>
          <label>Brand:</label>
          <input v-model="img.brand"/>
        </div>
        <div>
          <label>Words:</label>
          <input v-model="img.words"/>
        </div>
        <div>
          <label>Colors:</label>
          <input v-model="img.colors"/>
        </div>
        <div>
          <label>Figures:</label>
          <input v-model="img.figures"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AddPlate',
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
    //this.importAll(require.context('../assets/large/', true, /\.jpg$/));
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
