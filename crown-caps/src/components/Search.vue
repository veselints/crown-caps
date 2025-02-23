<template>
    <div>
        <div>
            <h2><label for="brand">Brand:</label></h2>
            <multiselect id="brand" v-model="brand" placeholder="Type text to search by brand" :options="brandsOptions"
                :multiple="false" :taggable="true" :internalSearch="false"
                @search-change="search($event, 'allBrands', 'brandsOptions')"></multiselect>
        </div>
        <div>
            <h2><label for="words">Words:</label></h2>
            <multiselect id="words" v-model="words" placeholder="Type text to search by word" :options="wordsOptions"
                :multiple="true" :taggable="true" :internalSearch="false"
                @search-change="search($event, 'allWords', 'wordsOptions')"></multiselect>
        </div>
        <div>
            <h2><label for="colors">Colors:</label></h2>
            <multiselect id="colors" v-model="colors" placeholder="Type text to search by color"
                :options="colorsOptions" :multiple="true" :taggable="true" :internalSearch="false"
                @search-change="search($event, 'allColors', 'colorsOptions')"></multiselect>
        </div>
        <div>
            <h2><label for="figures">Figures:</label></h2>
            <multiselect id="figures" v-model="figures" placeholder="Type text to search by figure"
                :options="figuresOptions" :multiple="true" :taggable="true" :internalSearch="false"
                @search-change="search($event, 'allFigures', 'figuresOptions')"></multiselect>
        </div>
        <div class="container">
            <div v-for="cap in result" :key="cap.imageName">
                <img :src="getPath(cap.imageName)" />
                <div>
                    <span>{{ cap.imageName }}</span>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Multiselect from 'vue-multiselect'

import A1 from './data/A1.json'
import A2 from './data/A2.json'
import A3 from './data/A3.json'
import B1 from './data/B1.json'
import B2 from './data/B2.json'
import B3 from './data/B3.json'
import B4 from './data/B4.json'
import C1 from './data/C1.json'
import C2 from './data/C2.json'
import C3 from './data/C3.json'
import D1 from './data/D1.json'
import D2 from './data/D2.json'
import D3 from './data/D3.json'
import D4 from './data/D4.json'
import E1 from './data/E1.json'
import E2 from './data/E2.json'
import E3 from './data/E3.json'
import F1 from './data/F1.json'
import F2 from './data/F2.json'
import F3 from './data/F3.json'
import F4 from './data/F4.json'
import G1 from './data/G1.json'
import G2 from './data/G2.json'
import G3 from './data/G3.json'
import H1 from './data/H1.json'
import H2 from './data/H2.json'
import H3 from './data/H3.json'
import H4 from './data/H4.json'
import I1 from './data/I1.json'
import I2 from './data/I2.json'
import I3 from './data/I3.json'
import J1 from './data/J1.json'
import J2 from './data/J2.json'
import J3 from './data/J3.json'
import J4 from './data/J4.json'
import K1 from './data/K1.json'
import K2 from './data/K2.json'
import K3 from './data/K3.json'
import L1 from './data/L1.json'
import L2 from './data/L2.json'
import L3 from './data/L3.json'
import small from './data/small.json'
import large from './data/large.json'

import No_Plate from './data/No_Plate.json'

import PL from './data/PL.json'

export default ({
    name: 'Serach',
    props: {
    },
    components: {
        Multiselect
    },
    data() {
        return {
            allCaps: [],
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
        this.allCaps = A1.concat(A2, A3,
            B1, B2, B3, B4,
            C1, C2, C3,
            D1, D2, D3, D4,
            E1, E2, E3,
            F1, F2, F3, F4,
            G1, G2, G3,
            H1, H2, H3, H4,
            I1, I2, I3,
            J1, J2, J3, J4,
            K1, K2, K3,
            L1, L2, L3,
            small, large,
            No_Plate, PL);

        this.allCaps.forEach(cap => {
            if (cap.brand && this.allBrands.indexOf(cap.brand) === -1) {
                this.allBrands.push(cap.brand);
            }

            if (cap.words) {
                cap.words.forEach(word => {
                    if (this.allWords.indexOf(word) === -1) {
                        this.allWords.push(word);
                    }
                });
            }

            if (cap.colors) {
                cap.colors.forEach(color => {
                    if (this.allColors.indexOf(color) === -1) {
                        this.allColors.push(color);
                    }
                });
            }

            if (cap.figures) {
                cap.figures.forEach(figure => {
                    if (this.allFigures.indexOf(figure) === -1) {
                        this.allFigures.push(figure);
                    }
                });
            }
        });

        this.allBrands.sort((a, b) => a.localeCompare(b));
        this.allWords.sort((a, b) => a.localeCompare(b));
        this.allColors.sort((a, b) => a.localeCompare(b));
        this.allFigures.sort((a, b) => a.localeCompare(b));
    },

    methods: {
        getPath(imageName) {
            return "./images/" + imageName;
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
                capValues = cap[field] || [],
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
    .container>div {
        width: 20%;
    }
}

@media only screen and (max-width: 1200px) {
    .container>div {
        width: 25%;
    }
}

@media only screen and (max-width: 992px) {
    .container>div {
        width: 33%;
    }
}

@media only screen and (max-width: 768px) {
    .container>div {
        width: 50%;
    }
}
</style>
