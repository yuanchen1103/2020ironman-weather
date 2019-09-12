<template>
  <div :id="`line-chart-${id}`"
       style="position: relative;"></div>
</template>

<script>
import * as d3 from 'd3';

export default {
  name: 'BarChart',
  data() {
    return {
      id: Date.now()
    };
  },
  mounted() {
    this.drawChart();
  },
  methods: {
    drawChart() {
      const data = [
        {
          value: 10,
          name: '18:35'
        },
        {
          value: 12,
          name: '18:55'
        },
        {
          value: 0,
          name: '19:15'
        },
        {
          value: 3,
          name: '19:35'
        },
        {
          value: 18,
          name: '19:55'
        },
        {
          value: 0,
          name: '20:15'
        },
        {
          value: 5,
          name: '20:35'
        },
        {
          value: 9,
          name: '20:55'
        },
        {
          value: 5,
          name: '21:15'
        },
        {
          value: 4,
          name: '21:35'
        },
        {
          value: 16,
          name: '21:55'
        },
        {
          value: 8,
          name: '22:15'
        }
      ];

      const margin = {
        top: 20,
        bottom: 20,
        left: 10,
        right: 10
      };
      const chartWidth = 470 - margin.left - margin.right;
      const chartHeight = 210 - margin.top - margin.bottom;
      const x = d3
        .scaleBand()
        .domain(data.map(e => e.name))
        .range([0, chartWidth])
        .paddingInner(0.4);

      const y = d3
        .scaleLinear()
        .domain([0, d3.max(data.map(e => e.value))])
        .range([chartHeight, 0]);

      const svg = d3
        .select(`#line-chart-${this.id}`)
        .append('svg')
        .attr('width', chartWidth + margin.left + margin.right)
        .attr('height', chartHeight + margin.top + margin.bottom);

      const g = svg.append('g').attr('transform', `translate(${margin.left}, ${margin.top})`);

      const yAxisCall = d3.axisLeft(y).tickSize(chartWidth);

      g.append('g')
        .attr('class', 'y-axis')
        .call(yAxisCall)
        .attr('transform', `translate(${chartWidth}, 0)`);

      g.selectAll('.bar')
        .data(data)
        .enter()
        .append('rect')
        .attr('class', 'bar')
        .attr('x', d => x(d.name))
        .attr('y', () => chartHeight)
        .attr('width', x.bandwidth)
        .attr('fill', '#00BAB6')
        .attr('rx', 3)
        .attr('height', 0)
        .transition()
        .duration(750)
        .delay((d, i) => i * 30)
        .ease(d3.easeCubic)
        .attr('height', d => chartHeight - y(d.value))
        .attr('y', d => y(d.value));

      g.selectAll('.label')
        .data(data)
        .enter()
        .append('text')
        .attr('class', 'label')
        .attr('x', d => x(d.name) + x.bandwidth() / 2)
        .attr('y', chartHeight + 15)
        .attr('text-anchor', 'middle')
        .attr('font-size', 12)
        .attr('fill', '#9B9B9B')
        .text((d, i) => (i % 2 === 0 ? d.name : ''));

      const dashLine = g
        .append('line')
        .attr('class', 'dash-line')
        .attr('y1', 0)
        .attr('y2', chartHeight)
        .attr('stroke', '#454545')
        .attr('stroke-width', 1)
        .attr('stroke-dasharray', 8)
        .attr('opacity', '0');

      const messageWrapper = d3
        .select(`#line-chart-${this.id}`)
        .append('div')
        .attr('class', 'message-wrapper')
        .html('<div class="circle"></div><div class="data"></div>')
        .attr('style', 'display: none;');

      g.selectAll('.hover-block')
        .data(data)
        .enter()
        .append('rect')
        .attr('class', 'hover-block')
        .attr('width', x.step)
        .attr('height', chartHeight)
        .attr('fill', 'transparent')
        .attr('x', d => x(d.name) - (x.step() - x.bandwidth()) / 2)
        .on('mouseover', (d, i) => {
          dashLine
            .attr('x1', () => x(d.name) + x.bandwidth() / 2)
            .attr('x2', () => x(d.name) + x.bandwidth() / 2)
            .attr('opacity', 1);

          d3.select('.message-wrapper .data').html(`${d.value} mm`);

          messageWrapper
            .attr('style', () => `display: flex; left: ${x(d.name) + 30}px`);
        })
        .on('mouseleave', (d, i) => {
          dashLine.attr('opacity', 0);
          messageWrapper.attr('style', 'display: none;');
        });
    }
  }
};
</script>

<style>
.y-axis text {
  display: none;
}

.y-axis .domain {
  display: none;
}

.y-axis .tick line {
  stroke: #e8e8e8;
}

.y-axis .tick:nth-child(even) {
  display: none;
}

.message-wrapper {
  border-radius: 4px;
  width: 102px;
  height: 34px;
  background: #ffffff;
  box-shadow: 0 2px 9px 0 rgba(0, 0, 0, 0.09);
  position: absolute;
  top: 20px;
  display: flex;
  align-items: center;
  padding-left: 15px;
}

.message-wrapper .circle {
  border-radius: 50%;
  height: 11px;
  width: 11px;
  background-color: #00bab6;
}

.message-wrapper .data {
  font-size: 12px;
  color: #7e7e7e;
  margin-left: 10px;
}
</style>