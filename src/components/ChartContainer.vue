<template>
    <div class="container">

        <select name="Period" @change="onChange($event)">
            <option value="m1">1 day</option>

            <option value="m15">1 week</option>

            <option value="h1">1 month</option>

            <option value="h12">1 year</option>
        </select>

        <canvas ref="canvas"></canvas>
    </div>
</template>

<script>
    import {Line} from 'vue-chartjs'
    import axios from 'axios';
    import numeral from "numeral";

    const options ={
        scales: {
            yAxes: [{
                ticks: {
                    callback: value => numeral(value).format('$0,0')
                }
            }]
        }
    }

    export default {
        name: 'LineChartContainer',
        extends: Line,
        methods: {
            onChange(event) {
                axios.get("https://api.coincap.io/v2/assets/bitcoin/history?interval="+event.target.value)
                    .then(res => {
                        console.log(res.data.data)
                        this.renderChart({
                                labels: (res.data.data.map(c => c.date)),
                                datasets: [{
                                    label: 'Bitcoin price',
                                    data: res.data.data.map(c => c.priceUsd),
                                    borderColor: 'rgba(50, 115, 220, 0.5)',
                                    backgroundColor: 'rgba(50, 115, 220, 0.1)',
                                    pointBorderColor: 'transparent',
                                    pointBackgroundColor: 'transparent',
                                    pointHoverBorderColor: 'blue',
                                    pointHoverBackgroundColor: 'red',
                                    borderWidth: 1
                                }]
                            },options
                        )
            })
        }},

        mounted () {
            axios.get("https://api.coincap.io/v2/assets/bitcoin/history?interval=m15")
                .then(res => {
                    console.log(res.data.data)
                    this.renderChart({
                        labels: (res.data.data.map(c => c.date)),
                        datasets: [{
                            label: 'Bitcoin price',
                            data: res.data.data.map(c => c.priceUsd),
                            borderColor: 'rgba(50, 115, 220, 0.5)',
                            backgroundColor: 'rgba(50, 115, 220, 0.1)',
                            pointBorderColor: 'transparent',
                            pointBackgroundColor: 'transparent',
                            pointHoverBorderColor: 'blue',
                            pointHoverBackgroundColor: 'red',
                            borderWidth: 1
                        }]
                    },options
                    )
                })

        }
    }
</script>

<style>
    .container {
        width: 80%;
    }
</style>