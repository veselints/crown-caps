<template>
    <div>
        <div>
            <label>Brand:</label>
            <input v-model="brand"/>
        </div>
        <div>
            <label>Words:</label>
            <input v-model="words"/>
        </div>
        <div>
            <label>Colors:</label>
            <input v-model="colors"/>
        </div>
        <div>
            <label>Figures:</label>
            <input v-model="figures"/>
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

    export default({
        name: 'Serach',
        props: {
            msg: String
        },
        data() {
            return {
                allCaps:[],
                result: [],
                brand: "",
                words: "",
                colors: "",
                figures: ""
            }
        },

        mounted() {
            this.allCaps = plate1.concat(plate2, plate3, plate4, plate5, plate6, plate7, 
                plate8, plate9, plate10, plate11, plate12, plate13, plate14, 
                plate15, plate16, plate17, plate18, plate19, plate20, plate21, 
                plate22, plate23, plate24, plate25, plate26, plate27, small);
        },

        methods: {
            getPath(imageName) {
                return "./images/" + imageName.split("_")[0] + "/" + imageName;
            },

            filterByBrand(cap) {
                if (!this.brand) {
                    return true;
                }

                return !!cap.brand && cap.brand.indexOf(this.brand) > -1;
            },

            filterBy(cap, field) {
                let that = this,
                    values = that[field] ? that[field].toLowerCase().split(" ") : [],
                    capValues = cap[field],
                    result = true;

                values.forEach(val => {
                    let currentResult = false;

                    capValues.forEach((v) => {
                        if (v.indexOf(val) > -1) {
                            currentResult = true;
                        }
                    });

                    if (!currentResult) {
                        result = false;
                    }
                });

                return result;
            },

            filterChange(old, field) {
                let val = this[field].toLowerCase(),
                    fields = ["words", "colors", "figures"];

                fields.splice(fields.indexOf(field), 1);

                if (!val) {
                    if (!this.brand && !this.words && !this.colors && !this.figures) {
                            this.result = [];
                        } else {
                            this.result = this.allCaps.filter(this.filterByBrand)
                                .filter(cap => this.filterBy(cap, fields[0]))
                                .filter(cap => this.filterBy(cap, fields[1]));
                        }
                } else {
                    if (!!old && val.indexOf(old) > -1) {
                        //debugger // eslint-disable-line no-debugger
                        this.result = this.result.filter(cap => this.filterBy(cap, field));
                    } else {
                        this.result = this.allCaps
                            .filter(this.filterByBrand)
                            .filter(cap => this.filterBy(cap, "words"))
                            .filter(cap => this.filterBy(cap, "colors"))
                            .filter(cap => this.filterBy(cap, "figures"));
                    }
                }
            }
        },

        watch: {
            brand(newVal, oldVal) {
                let that = this,
                    val = newVal.toLowerCase(),
                    old = oldVal.toLowerCase();

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
                this.filterChange(oldVal.toLowerCase(), "words");
            },

            colors(newVal, oldVal) {
                this.filterChange(oldVal.toLowerCase(), "words");
            },

            figures(newVal, oldVal) {
                this.filterChange(oldVal.toLowerCase(), "words");
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
  height: 300px;
  width: 300px;
  padding: 10px;
  display: block;
}
</style>
