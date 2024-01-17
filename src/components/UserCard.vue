<template>
    <template v-if="isConfirmId">
        <q-card class="my-card">
            <q-card-section class="bg-primary text-white">
                <div class="text-h6">{{ UserId }}用のカード</div>
            </q-card-section>

            <q-separator />

            <q-card-actions align="right">
                <template v-for="row, irow in CardData" v-bind:key="row">
                    <div class="column">
                        <template v-for="vObj, icol in row" v-bind:key="vObj">
                            <q-btn class="q-ma-xs border" :class="vObj.isOpen ? 'bg-grey-8 text-white' : 'bg-white'"
                                @click="changeState(irow, icol)" flat>{{
                                    vObj.value }}</q-btn>
                        </template>
                    </div>
                </template>
            </q-card-actions>
        </q-card>
    </template>
    <template v-else>
        <div class="column q-gutter-y-md flex flex-center">
            <div>
                以下の項目を入力してください
            </div>
            <q-input type="number" label="入学年度" filled outlined v-model="UserYear"></q-input>
            <q-input type="number" label="学籍番号下３桁" filled outlined v-model="UserNumber"></q-input>
            <q-btn :disable="UserYear === '' || UserNumber === ''" @click="getBingCard(UserYear, UserNumber)"
                color="primary" label="ビンゴカードの取得"></q-btn>
        </div>
    </template>
</template>

<script>
import { defineComponent, ref } from 'vue'
import items from "../data/items.json"

function SeededRandom(seed) {
    let m = 0x80000000;
    let a = 1103515245;
    let c = 12345;
    this.state = seed ? seed : Math.floor(Math.random() * (m - 1));
    this.nextInt = function () {
        this.state = (a * this.state + c) % m;
        return this.state;
    }
    this.nextFloat = function () {
        return this.nextInt() / (m - 1);
    }
}

export default defineComponent({
    name: 'UserCard',

    setup() {
        const UserYear = ref("")
        const UserNumber = ref("")
        const UserId = ref("")
        const isConfirmId = ref(false)

        function sampleWithoutReplacement(array, size, random) {
            let shuffled = array.slice(0), i = array.length, temp, index;
            while (i--) {
                index = Math.floor((i + 1) * random.nextFloat());
                temp = shuffled[index];
                shuffled[index] = shuffled[i];
                shuffled[i] = temp;
            }
            return shuffled.slice(0, size);
        }

        const getValueObj = (val) => {
            return { value: val, isOpen: false }
        }
        const CardData = ref([...Array(5)].map(() =>
            [...Array(5)].map((val = "") => getValueObj(val))
        ))

        const getFormat = (year, number) => {
            return `JB${year.toLowerCase()}S${number.toLowerCase()}`
        }

        return {
            UserYear,
            UserNumber,
            CardData,
            UserId,
            isConfirmId,

            getFormat,
            getValueObj,
            SeededRandom,
            sampleWithoutReplacement,
        }
    },
    methods: {
        getBingCard(year, number) {
            this.CardData = new Array()
            this.UserId = this.getFormat(year, number)
            const seed_str = this.UserYear + this.UserNumber
            const seed = Number(seed_str)
            const random = new SeededRandom(seed)
            const CardData1d = this.sampleWithoutReplacement(items, 25, random)

            while (CardData1d.length) this.CardData.push(CardData1d.splice(0, 5).map((val) => this.getValueObj(val)));

            this.isConfirmId = true
        },
        changeState(irow, icol) {
            this.CardData[irow][icol].isOpen = !this.CardData[irow][icol].isOpen
        }
    }
})
</script>

<style>
.border {
    border: 1px solid #eaeaea;
}
</style>