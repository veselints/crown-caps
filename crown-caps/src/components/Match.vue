<template>
    <div>
        <div v-for="pair, i in matches" :key="pair[0].imageName + pair[1].imageName" class="container" @click="removePair(i)">
            <div>
                <img :src="getPath(pair[0].imageName)"/>
                <div>
                    <span>{{pair[0].imageName}}</span>
                </div>
            </div>
            <div>
                <img :src="getPath(pair[1].imageName)"/>
                <div>
                    <span>{{pair[1].imageName}}</span>
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
    import large from './data/large.json'

    export default({
        name: 'Match',
        props: {
        },
        components: {
        },
        data() {
            return {
                allCaps:[],
                matches: [],
                empty: []
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

            let length = this.allCaps.length,
                i;

            for (i = 0; i < length; i++) {
                this.compareRest(i);
            }

            console.log(this.matches.length)
        },

        methods: {
            getPath(imageName) {
                return "./images/" + imageName.split("_")[0] + "/" + imageName;
            },

            removePair(index) {
                this.matches.splice(index, 1);
            },

            compareRest(index) {
                let length = this.allCaps.length,
                    reference = this.allCaps[index],
                    words = reference.words,
                    colors = reference.colors,
                    figures = reference.figures,
                    wordsLeght = words.length,
                    colorsLength = colors.length,
                    figuresLength = figures.length,
                    i, current, validWords, validColors, validFigures;

                for (i = index + 1; i < length; i++) {
                    current = this.allCaps[i];

                    if (reference.brand == current.brand) {
                        if (wordsLeght === current.words.length) {
                            validWords = true;

                            for (let j = 0; j < wordsLeght; j++) {
                                if (!current.words.includes(words[j])) {
                                    validWords = false;
                                    break;
                                }
                            }
//debugger // eslint-disable-line no-debugger
                            if (!validWords) {
                                continue;
                            }

                            if (colorsLength === current.colors.length) {
                                validColors = true;

                                for (let j = 0; j < colorsLength; j++) {
                                    if (!current.colors.includes(colors[j])) {
                                        validColors = false;
                                        break;
                                    }
                                }
    //debugger // eslint-disable-line no-debugger
                                if (!validColors) {
                                    continue;
                                }

                                if (figuresLength === current.figures.length) {
                                        validFigures = true;

                                        for (let j = 0; j < figuresLength; j++) {
                                            if (!current.figures.includes(figures[j])) {
                                                validFigures = false;
                                                break;
                                            }
                                        }

                                        if (!validFigures) {
                                            continue;
                                        }

                                        this.matches.push([reference, current]);
                                }
                            }
                        }
                    }
                }
            }
        }
    })
</script>

<style scoped>
.container {
  display: flex;
  flex-wrap: wrap;
}

.container > div {
    width: 40%;
}

.container img {
  width: 95%;
  padding: 5% 5% 5% 0;
  display: block;
}
</style>
