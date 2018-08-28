<template>
    <div class="graphArea">
        <button @click="handleButtonClick">切换</button>
        <br/>
        <svg>
            <line x1="40" x2="560" y1="250" y2="250" stroke="#ddd"/>
        </svg>
    </div>
</template>

<script>
import * as d3 from 'd3'

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
            currentIndex: 0
        }
    },
    mounted () {
        let svg = d3.select(this.$el).select('svg')
        svg.append('text').attr('x', 250).attr('y', 50).text(dataSource[this.currentIndex][0])

        //创建圆
        svg.append('g').selectAll('circle')
            .data(dataSource[this.currentIndex][1])
            .enter()
            .append('circle')
            .attr('class', 'myCircle')
            .attr('cx', (d, i) => {
                let spacing = lineLength / dataSource[this.currentIndex][1].length
                return 100 + i * spacing
            })
            .attr('cy', 250)
            .attr('r', (d) => {
                return d
            })
    },
    methods: {
        handleButtonClick () {
            this.currentIndex++
            if (this.currentIndex == dataSource.length) {
                this.currentIndex = 0
            }

            let svg = d3.select(this.$el).select('svg')
            svg.select('text').text(dataSource[this.currentIndex][0])

            let circle = svg.select('g').selectAll('circle').data(dataSource[this.currentIndex][1])
            circle.exit().remove()
            let newCircle = circle.enter().append('circle')
                .attr('class', 'myCircle')
                .attr('r', 0)
                .attr('cx', (d, i) => {
                    let spacing = lineLength / dataSource[this.currentIndex][1].length
                    return 100 + i * spacing
                })
            circle = newCircle.merge(circle)

            circle.transition().duration(500)
                .attr('cx', (d, i) => {
                    let spacing = lineLength / dataSource[this.currentIndex][1].length
                    return 100 + i * spacing
                })
                .attr('cy', 250)
                .attr('r', (d) => {
                    return d
                })
            svg.select('text').text(dataSource[this.currentIndex][0])
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

    .myCircle {
        fill: orange;
        stroke: orange;
        fill-opacity: 0.5;
    }
</style>
