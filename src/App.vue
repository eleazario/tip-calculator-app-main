<template>
    <div class="my-auto">
        <div id="title">
            <h4>
                SPLI<br/>
                TTER
            </h4>
        </div>
        <div id="calculator">
            <div class="inputSection">
                <div class="sectionTitleBox">
                    <h4 class="sectionTitle">Bill</h4>
                    <h4 v-if="isBillError" :class="['errorMessage']">Can't be zero</h4>
                </div>
                <div id="billInputBox" :class="['inputBox', { 'errorInputBox': isBillError }, { 'activeInputBox': isBillActive }]">
                    <img src="./images/icon-dollar.svg" alt="">
                    <input v-model="bill" @focus="()=> isBillActive = true" @focusout="()=> isBillActive = false" @input="inputBill" type="number" placeholder="0">
                </div>
                <div class="sectionTitleBox">
                    <h4 class="sectionTitle">Select tip %</h4>
                    <h4 v-if="isTipPercentError" :class="['errorMessage']">Can't be zero</h4>
                </div>
                <div class="tipButtonBox">
                    <TipButton :active="tipPercent" @onUpdateTip="updateActiveTip" :value="5"/>
                    <TipButton :active="tipPercent" @onUpdateTip="updateActiveTip" :value="10"/>
                    <TipButton :active="tipPercent" @onUpdateTip="updateActiveTip" :value="15"/>
                    <TipButton :active="tipPercent" @onUpdateTip="updateActiveTip" :value="25"/>
                    <TipButton :active="tipPercent" @onUpdateTip="updateActiveTip" :value="50"/>
                    <div :class="[ 'customTipBox', { 'activeInputBox': isCustomTipActive }, { 'errorInputBox': isTipPercentError }]">
                        <input id="customTipInputBox" v-model="customTip" @focus="()=> isCustomTipActive = true" @focusout="()=> isCustomTipActive = false" @input="inputCustomTip" :class="['customTip']" type="number" placeholder="Custom">
                    </div>
                    
                </div>
                <div class="sectionTitleBox">
                    <h4 class="sectionTitle">Number of People</h4>
                    <h4 v-if="isPeopleError" :class="['errorMessage']">Can't be zero</h4>
                </div>
                <div id="peopleInputBox" :class="['inputBox', { 'errorInputBox': isPeopleError }, { 'activeInputBox': isPeopleActive }]">
                    <img src="./images/icon-person.svg" alt="">
                    <input v-model="people" @focus="()=> isPeopleActive = true" @focusout="()=> isPeopleActive = false" @input="inputPeople" type="number" placeholder="0">
                </div> 
            </div>
            <div class="resultSection">
                <div class="resultBox">
                    <div class="resultItem">
                        <div>
                            <h4>Tip Amount</h4>
                            <p>/ person</p>
                        </div>
                        <span class="resultValue">${{ tipAmount.toFixed(2) }}</span>
                    </div>
                    <div class="resultItem">
                        <div>
                            <h4>Total</h4>
                            <p>/ person</p>
                        </div>
                        <span class="resultValue">${{ total.toFixed(2) }}</span>
                    </div>
                    <div class="flex-auto flex">
                        <button @click="resetInput" :disabled="bill || tipPercent || people ? false : true" class="resetButton">RESET</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import TipButton from './components/TipButton.vue';

export default {
    name: 'App',
    components: {
        TipButton
    },

    watch: {
    },

    data() {
        return {
            bill: null,
            tipPercent: null,
            customTip: null,
            people: null,
            tipAmount: 0,
            total: 0,

            isBillError: false,
            isTipPercentError: false,
            isPeopleError: false,

            isBillActive: false,
            isCustomTipActive: false,
            isPeopleActive: false,
        }
    },
    methods: {
        resetInput(){
            this.bill = null
            this.tipPercent = null
            this.customTip = null
            this.people = null
            this.tipAmount = 0
            this.total = 0

            this.isBillError = false
            this.isTipPercentError = false
            this.isPeopleError = false
        },
        updateActiveTip(value){
            this.tipPercent = value
            this.calculateTipAmountTotal()
        },
        inputBill(){
            if(this.bill<=0){
                this.isBillError = true
            }
            else{
                this.isBillError = false
            }
            this.calculateTipAmountTotal()
        },
        inputCustomTip(){
            this.tipPercent = this.customTip
            if(this.tipPercent<=0){
                this.isTipPercentError = true
            }
            else{
                this.isTipPercentError = false
            }
            this.calculateTipAmountTotal()
        },
        inputPeople(){
            if(this.people<=0){
                this.isPeopleError = true
            }
            else{
                this.isPeopleError = false
            }
            this.calculateTipAmountTotal()
        },
        calculateTipAmountTotal(){
            if(this.bill && this.tipPercent && this.people){
                this.tipAmount = this.bill * this.tipPercent/100 / this.people
                this.total = this.bill / this.people + this.tipAmount
            }
            else {
                this.tipAmount = 0
                this.total = 0
            }
        }
    }
}
</script>

<style scoped>
body {
    @apply
    mx-0
}

#title {
    @apply
        text-dark-grayish-cyan
        py-10
        lg:pt-0
        lg:pb-16
        text-center
        text-2xl
        tracking-[0.35em]
}

#calculator {
    @apply
        mx-auto
        bg-white
        rounded-t-3xl
        lg:rounded-3xl
        grid
        grid-cols-1
        lg:grid-cols-2
        max-w-[60rem]
        lg:p-2
        shadow-2xl
        lg:mb-12
}

.inputSection {
    @apply
        p-8
        pb-0
        lg:pb-8
        lg:pr-8
}

.sectionTitleBox {
    @apply
        flex
}

.sectionTitle {
    @apply
        text-dark-grayish-cyan
        mb-2
}

input[type=number] {
    @apply
        caret-strong-cyan
}

.inputBox {
    transition: outline 0.2s ease-in-out 0s;
    @apply
        flex
        mb-8
        rounded-sm
        outline-none
        outline-2
        -outline-offset-1
}

.inputBox > img {
    @apply
        bg-light-grayish-cyan
        bg-opacity-30
        p-4
}

.inputBox > input {
    @apply
        w-full
        bg-light-grayish-cyan
        bg-opacity-30
        pr-4
        text-very-dark-cyan
        text-end
        text-2xl
}

input[type=number]::-webkit-inner-spin-button, 
input[type=number]::-webkit-outer-spin-button { 
  -webkit-appearance: none; 
}

input:focus {
    @apply
        outline-0
}

.tipButtonBox {
    @apply
        grid
        grid-cols-2
        lg:grid-cols-3
        gap-4
        mb-8
}

.customTipBox {
    transition: outline 0.2s ease-in-out 0s;
    @apply
        rounded-md
        outline-none
        outline-2
        -outline-offset-1
}

.inputBox, .tipButtonBox {
    @apply
        mb-10
}

.customTip {
    @apply
        bg-light-grayish-cyan
        bg-opacity-30
        text-2xl
        text-end
        text-very-dark-cyan
        px-4
        rounded-md
        w-full
        h-full
}

#peopleInputBox {
    @apply
        lg:mb-0
}

.activeInputBox {
    @apply
        outline
        outline-strong-cyan
        outline-2
}

.errorInputBox {
    @apply
        outline
        outline-[#d1387b]
        outline-2
}

.errorMessage {
    @apply
        flex-auto
        text-end
        text-[#d1387b]
}

.resultSection {
    @apply
        p-6
        pt-0
        lg:pt-6
        lg:pl-2
}

.resultBox {
    @apply
        flex
        flex-col
        bg-very-dark-cyan
        p-6
        pt-12
        lg:p-10
        lg:pt-16
        rounded-xl
        h-full
}

.resultItem {
    @apply
        flex
        mb-8
        lg:mb-12
}

.resultItem > :first-child > h4 {
    @apply
        text-white
}

.resultItem > :first-child > p {
    @apply
        text-grayish-cyan
}

.resultValue {
    @apply
        flex-auto
        self-center
        text-end
        text-3xl
        lg:text-5xl
        text-strong-cyan
}

.resetButton {
    transition: background-color 0.2s ease-out 0s;
    @apply
        self-end
        w-full
        lg:h-14
        bg-strong-cyan
        text-very-dark-cyan
        text-xl
        py-2
        rounded-md
        hover:bg-[rgba(159,232,223,255)]
        disabled:opacity-20
        hover:disabled:bg-strong-cyan
    }
.resetButton:active {
    transition: background-color 0s ease-in 0s;
    @apply
        active:bg-very-light-grayish-cyan
        disabled:bg-strong-cyan
}

</style>
