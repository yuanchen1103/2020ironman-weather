<template>
  <div :id="`line-chart-${id}`"
       style="position: relative;"></div>
</template>

<script>
import * as d3 from 'd3';
import shortid from 'shortid';

const convertHex = (color) => {
  if (color.substring(0, 1) == '#') {
    color = color.substring(1);
  }

  /* Grab each pair (channel) of hex values and parse them to ints using hexadecimal decoding */
  const r = parseInt(color.substring(0, 2), 16);
  const g = parseInt(color.substring(2, 4), 16);
  const b = parseInt(color.substring(4), 16);
  return `rgba(${r}, ${g}, ${b}, 0.4)`;
};

export default {
  props: ['color', 'chartData'],
  name: 'LineChart',
  data() {
    return {
      id: shortid.generate()
    };
  },
  mounted() {
    this.drawChart();
  },
  methods: {
    drawChart() {
      const data = this.chartData;
      
      const margin = {
        top: 20,
        bottom: 20,
        left: 0,
        right: 0
      };
      const chartWidth = 260 - margin.left - margin.right;
      const chartHeight = 88 - margin.top - margin.bottom;

      const x = d3
        .scaleLinear()
        .domain([0, data.length - 1])
        .range([0, chartWidth]);

      const y = d3
        .scaleLinear()
        .domain([d3.min(data.map(e => e.value)), d3.max(data.map(e => e.value))])
        .range([chartHeight, 0]);

      const svg = d3
        .select(`#line-chart-${this.id}`)
        .append('svg')
        .attr('width', chartWidth + margin.left + margin.right)
        .attr('height', chartHeight + margin.top + margin.bottom);

      const g = svg.append('g').attr('transform', `translate(${margin.left}, ${margin.top})`);

      const line = d3
        .line()
        .x((d, i) => x(i))
        .y(d => y(d.value))
        .curve(d3.curveNatural);

      g.append('path')
        .datum(data)
        .attr('class', 'main-line')
        .attr('d', line)
        .attr('fill', 'none')
        .attr('stroke-width', 2)
        .attr('stroke', this.color);

      const area = d3
        .area()
        .x((d, i) => x(i))
        .y0(chartHeight)
        .y1(d => y(d.value))
        .curve(d3.curveNatural);

      const gradient = svg
        .append('defs')
        .append('linearGradient')
        .attr('id', `svgGradient-${this.id}`)
        .attr('x1', '50%')
        .attr('x2', '50%')
        .attr('y1', '0%')
        .attr('y2', '100%');

      gradient
        .append('stop')
        .attr('class', 'start')
        .attr('offset', '0%')
        .attr('stop-color', () => convertHex(this.color))
        .attr('stop-opacity', 1);

      gradient
        .append('stop')
        .attr('class', 'end')
        .attr('offset', '100%')
        .attr('stop-color', 'rgba(216,216,216,0.00)')
        .attr('stop-opacity', 1);

      g.append('path')
        .datum(data)
        .attr('class', 'area')
        .attr('d', area)
        .attr('fill', `url(#svgGradient-${this.id})`);

      svg
        .append('rect')
        .attr('class', 'mask')
        .attr('width', chartWidth)
        .attr('height', chartHeight + margin.top + margin.bottom)
        .attr('fill', '#ffffff')
        .attr('x', 0)
        .transition()
        .duration(1000)
        .attr('x', chartWidth);

      const dot = g
        .append('circle')
        .attr('class', 'dot')
        .attr('r', 5)
        .attr('stroke', this.color)
        .attr('stroke-width', 2)
        .attr('fill', '#ffffff')
        .attr('opacity', '0');

      const messageWrapper = d3
        .select(`#line-chart-${this.id}`)
        .append('div')
        .attr('class', 'line-message-wrapper')
        .attr('id', `line-message-wrapper-${this.id}`)
        .html(`<div class="circle" style="background: ${this.color}"></div><div class="data"></div>`)
        .attr('style', 'display: none;');

      g.selectAll(`.line-hover-block-${this.id}`)
        .data(data)
        .enter()
        .append('rect')
        .attr('class', `line-hover-block-${this.id}`)
        .attr('width', chartWidth / data.length)
        .attr('height', chartHeight)
        .attr('fill', 'transparent')
        .attr('x', (d, i) => x(i) - chartWidth / data.length / 2)
        .on('mouseover', (d, i) => {
          dot
            .attr('cx', () => x(i))
            .attr('cy', () => y(d.value))
            .attr('opacity', 1);

          d3.select(`#line-message-wrapper-${this.id} .data`).html(`${d.value}`);

          messageWrapper.attr('style', () => `display: flex; left: ${x(i) + 15}px; top: ${y(d.value)}px`);
        })
        .on('mouseleave', () => {
          dot.attr('opacity', 0);
          messageWrapper.attr('style', 'display: none;');
        });
    }
  }
};
</script>

<style>
.line-message-wrapper {
  border-radius: 4px;
  width: 76px;
  height: 34px;
  background: #ffffff;
  box-shadow: 0 2px 9px 0 rgba(0, 0, 0, 0.09);
  position: absolute;
  display: flex;
  align-items: center;
  padding-left: 15px;
  z-index: 3;
}

.line-message-wrapper .circle {
  border-radius: 50%;
  height: 11px;
  width: 11px;
}

.line-message-wrapper .data {
  font-size: 12px;
  color: #7e7e7e;
  margin-left: 10px;
}
</style>