<template>
  <div id="app">
    <button onclick="update(data1)">Variable 1</button>
    <button onclick="update(data2)">Variable 2</button>

<!-- Create a div where the graph will take place -->
<div id="my_dataviz"></div>
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
  name: 'app',
  components: {
  },
  data:()=>{
    return {
      data1 :[
   {group: "A", value: 4},
   {group: "B", value: 16},
   {group: "C", value: 8}
],

data2:[
   {group: "A", value: 7},
   {group: "B", value: 1},
   {group: "C", value: 20}
],
x: d3.scaleBand(),
y: d3.scaleLinear(),
margin :{top: 30, right: 30, bottom: 70, left: 60},
    width : null,
    height :null,
    svg: d3.select("#my_dataviz")
    }
  
  
  },
  mounted(){
    
    this.width = 460 - this.margin.left - this.margin.right;
    this.height = 400 - this.margin.top - this.margin.bottom;
// create 2 data_set


// set the dimensions and margins of the graph


// append the svg object to the body of the page

  this.svg.append("svg")
    .attr("width", this.width + this.margin.left + this.margin.right)
    .attr("height", this.height + this.margin.top + this.margin.bottom)
  .append("g")
    .attr("transform",
          "translate(" + this.margin.left + "," + this.margin.top + ")");

// X axis

  this.x.range([ 0, this.width ])
  .domain(this.data1.map(function(d) { return d.group; }))
  .padding(0.2);
this.svg.append("g")
  .attr("transform", "translate(0," + this.height + ")")
  .call(d3.axisBottom(this.x))

// Add Y axis

  this.y.domain([0, 20])
  .range([ this.height, 0]);
this.svg.append("g")
  .attr("class", "myYaxis")
  .call(d3.axisLeft(this.y));

// A function that create / update the plot for a given variable:

console.log(this.height)
// Initialize the plot with the first dataset
this.update(this.data1,this.x, this.y, this.height, this.svg)
  },
  methods:{
    update(data, x ,y, height, svg){
console.log(x)
  var u = svg.selectAll("rect")
    .data(data)

  u.enter()
    .append("rect")
    .merge(u)
    .transition()
    .duration(1000)
      .attr("x", function(d) { return x(d.group); })
      .attr("y", function(d) { return y(d.value); })
      .attr("width", x.bandwidth())
      .attr("height", function(d) { return height - y(d.value); })
      .attr("fill", "#69b3a2")
}
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
