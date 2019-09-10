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

      const reactGroups = d3
        .select(`#line-chart-${this.id}`)
        .append('svg')
        .attr('width', chartWidth + margin.left + margin.right)
        .attr('height', chartHeight + margin.top + margin.bottom)
        .append('g')
        .attr('transform', `translate(${margin.left}, ${margin.top})`);

      reactGroups
        .selectAll('rect')
        .data(data)
        .enter()
        .append('rect')
        .attr('x', d => x(d.name))
        .attr('y', d => y(d.value))
        .attr('width', x.bandwidth)
        .attr('height', d => chartHeight - y(d.value))
        .attr('fill', '#00BAB6')
        .attr('rx', 3);
    }
  }
};
</script>

<style scoped>
</style>