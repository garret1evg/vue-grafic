<template>
    <div class="container">
        <nav>

            <ul>
                <li class="item"  value="m1"><a @click="onChange($event)" href = "#" >1D</a></li>
                <li class="item"  value="m15"><a @click="onChange($event)" href = "#">1W</a></li>
                <li class="item"  value="h1"><a @click="onChange($event)" href = "#">1M</a></li>
                <li class="item"  value="h12"><a @click="onChange($event)" href = "#">1Y</a></li>
            </ul>
        </nav>


        <canvas ref="canvas"></canvas>
    </div>
</template>

<script>
    import {Line} from 'vue-chartjs'
    import axios from 'axios';
    import numeral from "numeral";


    const options ={
        legend: {
            display: false
        },
        scales: {

            yAxes: [{
                position : 'right',
                ticks: {
                    callback: value => numeral(value).format('$0,0')
                },gridLines:{
                    display: false
                }
            }],
            xAxes: [{
                gridLines:{
                    display: false
                }
            }]




        }

    }

    export default {
        name: 'LineChartContainer',
        extends: Line,
        methods: {
            onChange(event) {
                //console.log(event)
                axios.get("https://api.coincap.io/v2/assets/bitcoin/history?interval="+event.path[1]._value)
                    .then(res => {

                        this.renderChart({
                                labels: (res.data.data.map(c => c.date)),
                                datasets: [{
                                    label: 'Bitcoin price',
                                    data: res.data.data.map(c => c.priceUsd),
                                    borderColor: '#40E0D0',
                                    backgroundColor: 'rgba(0, 0, 0, 0)',
                                    pointBorderColor: 'transparent',
                                    pointBackgroundColor: 'transparent',
                                    pointHoverBorderColor: 'blue',
                                    pointHoverBackgroundColor: 'red',
                                    borderWidth: 2
                                }]
                            },options
                        )
            })
        }},

        mounted () {
            axios.get("https://api.coincap.io/v2/assets/bitcoin/history?interval=m1")
                .then(res => {

                    this.renderChart({
                        labels: (res.data.data.map(c => c.date)),
                        datasets: [{
                            label: 'Bitcoin price',
                            data: res.data.data.map(c => c.priceUsd),
                            borderColor: '#40E0D0',
                            backgroundColor: 'rgba(0, 0, 0, 0)',
                            pointBorderColor: 'transparent',
                            pointBackgroundColor: 'transparent',
                            pointHoverBorderColor: 'blue',
                            pointHoverBackgroundColor: 'red',
                            borderWidth: 2
                        }]
                    },options
                    )
                })

        }
    }

</script>

<style>
    nav{
        float:right;
    }
    ol, ul {
        list-style: none;
    }
    li {
        display: inline-block;
        padding: 20px 0 20px;
        vertical-align: middle;
    }
    a:hover, a:focus, a:active {
        color: #000;
        text-decoration: none;
    }
    a {
        font-family: Arial, 'Helvetica Neue', Helvetica, sans-serif;
        text-decoration: none;
        transition: color 0.1s, background-color 0.1s;
    }
    a {
        position: relative;
        display: block;
        padding: 16px 0;
        margin: 0 12px;
        letter-spacing: 1px;
        font-size: 18px;
        line-height: 16px;
        font-weight: 900;
        text-transform: uppercase;
        transition: color 0.1s,background-color 0.1s,padding 0.2s ease-in;
        color: #666;
    }
    a::before {
        content: '';
        display: block;
        position: absolute;
        bottom: 3px;
        left: 0;
        height: 3px;
        width: 100%;
        background-color: #40E0D0;
        transform-origin: right top;
        transform: scale(0, 1);
        transition: color 0.1s,transform 0.2s ease-out;
    }
    a:active::before {
        background-color: #666;
    }
    a:hover::before, a:focus::before {
        transform-origin: left top;
        transform: scale(1, 1);
    }

</style>