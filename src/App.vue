<template>
    <div>
        <label>Ilość: </label>
        <input v-model="quantity" type="text" :style="colorQuantity" placeholder="Podaj ilość" />
        <div v-show="invalidQuantity">Wpisano niepoprawną ilość</div>
    </div>
    <div>
        <label>Cena netto: </label>
        <input v-model="net" type="text" :style="colorNet" placeholder="Podaj cenę netto" />
        <div v-show="invalidNet">Wpisano niepoprawną kwotę netto</div>
    </div>
    <div>
        <label>Podatek w %: </label>
        <input v-model="tax" type="text" :style="colorTax" placeholder="Podaj podatek w %" />
        <div v-show="invalidTax">Wpisano niepoprawną kwotę podatku</div>
    </div>
    <div v-show="forUnitPresent">Cena brutto za jeden produkt: {{ calculateUnitGross }}</div>
    <div v-show="allPresent">Cena brutto za wszystkie produkty: {{ calculateTotalGross }}</div>
</template>

<script>
export default {
    name: "App",
    data() {
        return {
            quantity: "",
            net: "",
            tax: "",
        };
    },
    computed: {
        quantityNumber() {
            return Number(this.quantity.replace(",", "."));
        },
        netNumber() {
            return Number(this.net.replace(",", "."));
        },
        taxNumber() {
            return Number(this.tax.replace(",", "."));
        },
        taxAmount() {
            return (this.netNumber * this.taxNumber) / 100;
        },
        invalidQuantity() {
            return isNaN(this.quantityNumber)
        },
        invalidNet() {
            return isNaN(this.netNumber)
        },
        invalidTax() {
            return isNaN(this.taxNumber)
        },
        calculateUnitGross() {
            if (this.invalidNet || this.invalidTax) {
                return "Podane nieprawidłowe wartości"
            }
            return (this.netNumber + this.taxAmount).toFixed(2);
        },
        calculateTotalGross() {
            if (this.invalidNet || this.invalidTax || this.invalidQuantity) {
                return "Podane nieprawidłowe wartości"
            }
            return ((this.netNumber + this.taxAmount) * this.quantityNumber).toFixed(2);
        },
        allPresent() {
            return this.quantity !== "" && this.net !== "" && this.tax !== "";
        },
        forUnitPresent() {
            return this.net !== "" && this.tax !== "";
        },
        colorQuantity() {
            return {
                color: this.invalidQuantity ? "red" : "",
            }
        },
        colorNet() {
            return {
                color: this.invalidNet ? "red" : "",
            }
        },
        colorTax() {
            return {
                color: this.invalidTax ? "red" : "",
            }
        }
    },
}
</script>

<style>
#app {
    font-family: Helvetica, Arial, sans-serif;
    text-align: center;
    margin-top: 60px;
}
</style>
