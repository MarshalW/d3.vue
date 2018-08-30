<template>
    <div class="graphArea">
        <button @click="handleButtonClick">切换</button>
        <br/>
        <svg>
            <text x="250" y="50">{{currentCountry}}</text>
            <line x1="40" x2="560" y1="250" y2="250" stroke="#ddd"/>
            <transition-group tag="g" name="circle">
                <circle v-for="(r, index) in currentData" :key="index" :cx="getCurrentCx(index)" cy="250" :r="r"/>
            </transition-group>
        </svg>
    </div>
</template>

<script>

let dataSource = [
    ['中国', [30, 45, 88, 23]],
    ['美国', [40, 48, 72, 100, 34]],
    ['日本', [10, 33, 26]],
]
let lineLength = 400

export default {
    name: 'HelloWorld',
    data () {
        return {
            currentIndex: 0,
            currentData: [],
            currentCountry: '--'
        }
    },
    mounted () {
        this.currentData = dataSource[this.currentIndex][1].slice(0)
        this.currentCountry = dataSource[this.currentIndex][0]
    },
    methods: {
        getCurrentCx (index) {
            let spacing = lineLength / dataSource[this.currentIndex][1].length
            return 100 + index * spacing
        },
        handleButtonClick () {
            this.currentIndex++
            if (this.currentIndex == dataSource.length) {
                this.currentIndex = 0
            }

            this.currentData = dataSource[this.currentIndex][1].slice(0)
            this.currentCountry = dataSource[this.currentIndex][0]
        }
    }
}

</script>

<style>
    .graphArea {
        display: flex;
        flex-direction: column;
    }

    button {
        width: 40px;
    }

    svg {
        width: 600px;
        height: 500px;
        border: 1px;
        border-color: gray;
        border-style: solid;
    }

    circle {
        fill: orange;
        stroke: orange;
        fill-opacity: 0.5;
    }

    .circle-enter-active {
        transition: all 2s;
    }

</style>
