<template>
    <div>
        <div>
            <h2><label for="brand">Brand:</label></h2>
            <multiselect id="brand"
                    v-model="brand"
                    placeholder="Type text to search by brand"
                    :options="brandsOptions" 
                    :multiple="false" 
                    :taggable="true"
                    :internalSearch="false"
                    @search-change="search($event, 'allBrands', 'brandsOptions')"></multiselect>
        </div>
        <div>
            <h2><label for="words">Words:</label></h2>
            <multiselect id="words"
                    v-model="words"
                    placeholder="Type text to search by word"
                    :options="wordsOptions" 
                    :multiple="true" 
                    :taggable="true"
                    :internalSearch="false"
                    @search-change="search($event, 'allWords', 'wordsOptions')"></multiselect>
        </div>
        <div>
            <h2><label for="colors">Colors:</label></h2>
            <multiselect id="colors"
                    v-model="colors"
                    placeholder="Type text to search by color"
                    :options="colorsOptions" 
                    :multiple="true" 
                    :taggable="true"
                    :internalSearch="false"
                    @search-change="search($event, 'allColors', 'colorsOptions')"></multiselect>
        </div>
        <div>
            <h2><label for="figures">Figures:</label></h2>
            <multiselect id="figures"
                    v-model="figures"
                    placeholder="Type text to search by figure"
                    :options="figuresOptions" 
                    :multiple="true" 
                    :taggable="true"
                    :internalSearch="false"
                    @search-change="search($event, 'allFigures', 'figuresOptions')"></multiselect>
        </div>
        <div class="container">
            <div v-for="cap in result" :key="cap.imageName">
                <img :src="getPath(cap.imageName)"/>
                <div>
                    <span>{{cap.imageName}}</span>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Multiselect from 'vue-multiselect'

    import plate1 from './data/plate1.json'
    import plate2 from './data/plate2.json'
    import plate3 from './data/plate3.json'
    import plate4 from './data/plate4.json'
    import plate5 from './data/plate5.json'
    import plate6 from './data/plate6.json'
    import plate7 from './data/plate7.json'
    import plate8 from './data/plate8.json'
    import plate9 from './data/plate9.json'
    import plate10 from './data/plate10.json'
    import plate11 from './data/plate11.json'
    import plate12 from './data/plate12.json'
    import plate13 from './data/plate13.json'
    import plate14 from './data/plate14.json'
    import plate15 from './data/plate15.json'
    import plate16 from './data/plate16.json'
    import plate17 from './data/plate17.json'
    import plate18 from './data/plate18.json'
    import plate19 from './data/plate19.json'
    import plate20 from './data/plate20.json'
    import plate21 from './data/plate21.json'
    import plate22 from './data/plate22.json'
    import plate23 from './data/plate23.json'
    import plate24 from './data/plate24.json'
    import plate25 from './data/plate25.json'
    import plate26 from './data/plate26.json'
    import plate27 from './data/plate27.json'
    import small from './data/small.json'
    import large from './data/large.json'

    export default({
        name: 'Serach',
        props: {
        },
        components: {
            Multiselect
        },
        data() {
            return {
                allCaps:[],
                result: [],
                brand: "",
                words: [],
                colors: [],
                figures: [],
                allBrands: [],
                allWords: [],
                allColors: [],
                allFigures: [],
                brandsOptions: [],
                wordsOptions: [],
                colorsOptions: [],
                figuresOptions: []
            }
        },

        // ====================
        // Implement also options sources upon other option change!!!!!!
        // ====================
        mounted() {
            this.allCaps = plate1.concat(plate2, plate3, plate4, plate5, plate6, plate7, 
                plate8, plate9, plate10, plate11, plate12, plate13, plate14, 
                plate15, plate16, plate17, plate18, plate19, plate20, plate21, 
                plate22, plate23, plate24, plate25, plate26, plate27, small, large);

            this.allCaps.forEach(cap => {
                if (cap.brand && this.allBrands.indexOf(cap.brand) === -1) {
                    this.allBrands.push(cap.brand);
                }

                cap.words.forEach(word => {
                    if (this.allWords.indexOf(word) === -1) {
                        this.allWords.push(word);
                    }
                });

                cap.colors.forEach(color => {
                    if (this.allColors.indexOf(color) === -1) {
                        this.allColors.push(color);
                    }
                });

                cap.figures.forEach(figure => {
                    if (this.allFigures.indexOf(figure) === -1) {
                        this.allFigures.push(figure);
                    }
                });
            });

            this.allBrands.sort((a, b) => a.localeCompare(b));
            this.allWords.sort((a, b) => a.localeCompare(b));
            this.allColors.sort((a, b) => a.localeCompare(b));
            this.allFigures.sort((a, b) => a.localeCompare(b));
        },

        methods: {
            getPath(imageName) {
                return "./images/" + imageName.split("_")[0] + "/" + imageName;
            },

            search(query, source, destination) {
                if (query && query.length !== 0) {
                    this[destination] = this[source].filter(brand => brand.indexOf(query.toLowerCase()) === 0);
                } else {
                    this[destination] = this[source];
                }
            },

            filterByBrand(cap) {
                if (!this.brand) {
                    return true;
                }

                return !!cap.brand && cap.brand === this.brand;
            },

            filterBy(cap, field) {
                let that = this,
                    values = that[field],
                    capValues = cap[field],
                    result = true;

                values.forEach(val => {
                    //let currentResult = false;

                    // capValues.forEach((v) => {
                    //     if (v.indexOf(val) > -1) {
                    //         currentResult = true;
                    //     }
                    // });

                    if (!capValues.includes(val)) {
                        result = false;
                    }
                });

                return result;
            },

            filterChange(old, field) {
                let val = this[field],
                    fields = ["words", "colors", "figures"];

                fields.splice(fields.indexOf(field), 1);

                if (!val || val.length === 0) {
                    if (!this.brand && this.words.length === 0 && this.colors.length === 0 && this.figures.length === 0) {
                            this.result = [];
                        } else {
                            this.result = this.allCaps.filter(this.filterByBrand)
                                .filter(cap => this.filterBy(cap, fields[0]))
                                .filter(cap => this.filterBy(cap, fields[1]));
                        }
                } else {
                    this.result = this.allCaps
                        .filter(this.filterByBrand)
                        .filter(cap => this.filterBy(cap, "words"))
                        .filter(cap => this.filterBy(cap, "colors"))
                        .filter(cap => this.filterBy(cap, "figures"));
                }
            }
        },

        watch: {
            brand(newVal, oldVal) {
                let that = this,
                    val = newVal ? newVal.toLowerCase() : "",
                    old = oldVal ? oldVal.toLowerCase() : "";

                if (!val) {
                    if (that.words.length === 0 &&
                        that.colors.length === 0 &&
                        that.figures.length === 0) {
                            that.result = [];
                        } else {
                            that.result = that.allCaps.filter(cap => that.filterBy(cap, "words"))
                                .filter(cap => that.filterBy(cap, "colors"))
                                .filter(cap => that.filterBy(cap, "figures"));
                        }
                } else {
                    if (!!old && val.indexOf(old) > -1) {
                        that.result = that.result.filter(that.filterByBrand);
                    } else {
                        that.result = that.allCaps
                            .filter(that.filterByBrand)
                            .filter(cap => that.filterBy(cap, "words"))
                            .filter(cap => that.filterBy(cap, "colors"))
                            .filter(cap => that.filterBy(cap, "figures"));
                    }
                }
            },

            words(newVal, oldVal) {
                this.filterChange(oldVal, "words");
            },

            colors(newVal, oldVal) {
                this.filterChange(oldVal, "words");
            },

            figures(newVal, oldVal) {
                this.filterChange(oldVal, "words");
            }
        }
    })
</script>

<style scoped>
.container {
  display: flex;
  flex-wrap: wrap;
}

.container img {
  width: 95%;
  padding: 5% 5% 5% 0;
  display: block;
}

/* @media only screen and (max-width: 575px) {
    .container > div {
        width: 100%;
    }
} */

@media only screen and (min-width: 1201px) {
    .container > div {
        width: 20%;
    }
}

@media only screen and (max-width: 1200px) {
    .container > div {
        width: 25%;
    }
}

@media only screen and (max-width: 992px) {
    .container > div {
        width: 33%;
    }
}

@media only screen and (max-width: 768px) {
    .container > div {
        width: 50%;
    }
}






</style>
