<template>
    <div class="container">
        <div class="service-price">
            <label for="servicePriceInput">Service Price</label>
            <span class="dollar">$</span>
            <input type="text" id="servicePriceInput" maxlength="7" v-model="servicePrice" @input="validateInput" />
        </div>
        <div class="price-insight">
            <div class="price-display">
                <div class="title">
                    <p>Price insight</p>
                </div>
                <div class="price-container">
                    <span class="price">${{ servicePrice }}</span>
                    <span class="description">Hourly Rate</span>
                </div>
            </div>
            <div class="histogram-container">
                <div class="histogram">
                    <img src="../assets/images/Histogram.svg" />
                    <span class="rate-indicator" :style="indicatorStyle"></span>
                </div>
                <div class="min-max">
                    <div class="min">
                        <p class="price">${{ min }}</p>
                        <p class="title">Min</p>
                    </div>
                    <div class="max">
                        <p class="price">${{ max }}</p>
                        <p class="title">Max</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, computed, } from 'vue';

const servicePrice = ref(50);
const min = 65;
const max = 250;

const indicatorStyle = computed(() => {

    let height = (servicePrice.value / max) * 100;

    let left = ((servicePrice.value - min) / (max - min)) * 100;

    if (servicePrice.value == max) left = 91
    else if (servicePrice.value > max) left = 96
    else if (servicePrice.value == min) left = 10

    if (height > 46) height = 46
    else height = 20

    return {
        height: `${Math.max(20, height)}%`,
        top: `${95 - Math.max(8, height)}%`,
        left: `${Math.max(5, left)}%`
    };
});


const validateInput = (event) => {
    let value = event.target.value.replace(/[^0-9]+/g, '').replace(/^0+/, '0');
    if (value === '') value = '0';
    event.target.value = value;
    servicePrice.value = parseInt(value, 10);
};

</script>

<style>
.container {
    display: flex;
    flex-direction: column;
    gap: 8px;
    margin-top: 40px;
}

.service-price {
    display: flex;
    flex-direction: column;
    gap: 3px;
    position: relative;
    font-weight: 600;
}

.service-price .dollar {
    position: absolute;
    bottom: 16px;
    left: 15px;
    font-size: 17px;
}

.service-price label {
    color: #797979;
    font-weight: 600;
}

.service-price input {
    width: 455px;
    height: 50px;
    border-radius: 12px;
    border: 1px solid #CCCCCC;
    padding-inline-start: 25px;
    font-size: 18px;
    font-weight: 600;
    line-height: 24px;
    letter-spacing: 0px;
    text-align: left;
}

.price-insight {
    position: relative;
    width: 425px;
    height: 280px;
    border-radius: 12px;
    border: 1px solid #CCCCCC;
    padding: 0px 30px;
}

.price-display {
    display: flex;
    flex-direction: column;
}

.price-display .title {
    font-size: 14px;
    font-weight: 600;
    line-height: 16px;
}

.price-container {
    display: flex;
    flex-direction: column;
    position: relative;
    height: 32%;
    padding-bottom: 6px;
}

.price-container .price {
    font-size: 60px;
    font-weight: 700;
    transition: left 0.5s ease;
}

.price-container .description {
    position: absolute;
    bottom: 0;
    font-size: 14px;
    font-weight: 600;
}

.histogram-container {
    position: absolute;
    bottom: 71px;
}

.min-max {
    display: flex;
    position: relative;
}

.min-max .min {
    position: absolute;
    left: 25px;
    top: -16px;
}

.min-max .max {
    position: absolute;
    right: 14px;
    top: -16px;
}

.min-max .title {
    position: absolute;
    color: #A3A3A3;
    font-weight: 600;
    top: 24px;
    right: 1px;
}

.min-max .price {
    font-weight: 600;
}

.histogram {
    position: relative;
    width: 100%;
    height: 100%;
}

.rate-indicator {
    position: absolute;
    width: 4px;
    background-color: #069778;
    border-radius: 2px;
    transition: height 0.5s ease, top 0.5s ease, left 0.5s ease;
}
</style>