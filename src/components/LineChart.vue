<template>
  <div :id="`line-chart-${id}`"></div>
</template>

<script>
import * as d3 from 'd3';

export default {
  name: 'LineChart',
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

      g.selectAll('rect')
        .data(data)
        .enter()
        .append('rect')
        .attr('x', d => x(d.name))
        .attr('y', d => chartHeight)
        .attr('width', x.bandwidth)
        .attr('fill', '#00BAB6')
        .attr('rx', 3)
        .attr('height', d => 0)
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
  stroke: #E8E8E8;
}

.y-axis .tick:nth-child(even) {
  display: none;
}
</style>