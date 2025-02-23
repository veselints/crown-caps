<template>
    <div>
        <div v-for="pair, i in matches" :key="pair[0].imageName + pair[1].imageName" class="container"
            @click="removePair(i)">
            <div>
                <img :src="getPath(pair[0].imageName)" />
                <div>
                    <span>{{ pair[0].imageName }}</span>
                </div>
            </div>
            <div>
                <img :src="getPath(pair[1].imageName)" />
                <div>
                    <span>{{ pair[1].imageName }}</span>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
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
import L1 from './data/K1.json'
import L2 from './data/K2.json'
import L3 from './data/K3.json'
import small from './data/small.json'
import large from './data/large.json'

import PL from './data/PL.json'

import No_Plate from './data/No_Plate.json'

export default ({
    name: 'Match',
    props: {
    },
    components: {
    },
    data() {
        return {
            allCaps: [],
            matches: [],
            empty: []
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

        let length = this.allCaps.length,
            i;

        for (i = 0; i < length; i++) {
            this.compareRest(i);
        }

        console.log(this.matches.length)
    },

    methods: {
        getPath(imageName) {
            return "./images/" + imageName;
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

.container>div {
    width: 40%;
}

.container img {
    width: 95%;
    padding: 5% 5% 5% 0;
    display: block;
}
</style>
