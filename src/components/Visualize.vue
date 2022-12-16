<script setup>
import * as d3 from 'd3';
import { onMounted, ref } from 'vue';

const props = defineProps({
  input: Array
});

onMounted(() => {

  const svg = d3.select('svg'),
    margin = 50,
    width = svg.attr('width') - margin,
    height = svg.attr('height') - margin;
  const g = svg
    .append('g')
    .attr('transform', 'scale(0.95) translate(' + margin + ',' + margin + ')');
  const parseTime = d3.timeParse('%d-%b-%y');
  const x = d3
    .scaleTime()
    .domain(
      d3.extent(props.input, function (d) {
        return parseTime(d.date);
      })
    )
    .rangeRound([0, width]);

  const y = d3
    .scaleLinear()
    .domain(
      d3.extent(props.input, function (d) {
        return d.amount;
      })
    )
    .rangeRound([height, 0]);
  const line = d3
    .line()
    .x(function (d) {
      return x(parseTime(d.date));
    })
    .y(function (d) {
      return y(d.amount);
    });
  var div = d3.select("body").append("div")
     .attr("class", "tooltip-donut")
     .style("opacity", 0);
  g.append('g')
    .attr('transform', 'translate(0,' + height + ')')
    .call(d3.axisBottom(x));
  g.append('g')
    // .attr("transform", "translate(" + width + ", 0)")
    .call(d3.axisLeft(y))
    .append('text')
    .attr('fill', '#fff')
    .attr('transform', 'rotate(-90)')
    .attr('y', 6)
    .attr('dy', '0.71em')
    .attr('text-anchor', 'end')
    .text('Stonks ($)');
  g.append('path')
    .datum(props.input)
    .attr('fill', 'none')
    .attr('stroke', 'steelblue')
    .attr('stroke-width', 1.5)
    .attr('d', line);
  g.append('g')
    .selectAll('dot')
    .data(props.input)
    .enter()
    .append('circle')
    .attr('cx', function (d) {
      return x(parseTime(d.date));
    })
    .attr('cy', function (d) {
      return y(d.amount);
    })
    .attr('r', 3)
    .style('fill', 'Red');
  //Our new hover effects
  g.selectAll('circle')
    .on('mouseover', function (e, d) {
      // console.log(e, d)
      d3.select(this)
        .transition()
        .duration('50')
        .attr('opacity', '.85')
        .attr('r', 9);
      div.transition()
        .duration(50)
        .style("opacity", 1);
      
      div.html(d.date + "<br/>" + d.amount)
        .style("left", (e.pageX + 10) + "px")
        .style("top", (e.pageY - 15) + "px");
      // .attr("stroke-width", 3.5)
    })
    .on('mouseout', function (d, i) {
      d3.select(this)
        .transition()
        .duration('50')
        .attr('opacity', '1')
        .attr('r', 3);
      div.transition()
               .duration('50')
               .style("opacity", 0);
      // .attr("stroke-width", 1.5)
    });
});
</script>

<template>
  <div>
    <h2>Vue.js and D3 Line Chart</h2>
    <svg width="500" height="500"></svg>
  </div>
</template>
