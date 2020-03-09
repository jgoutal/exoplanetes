<template>
    <span class="image"></span>
</template>

<script>
import * as d3 from 'd3'

export default {
    props:["orbper"],
    mounted() {
        console.log(this.orbper)
        var now = d3.timeYear.floor(new Date());
         
        var spacetime = d3.select('span');
        var width = 600,
            height = 600,
            radius = 600;
         
        var radii = {
          "sun": radius / 8,
          "planetOrbit": radius / 2.5,
          "planet": radius / 28,
        
        };
         
        // Space
        var svg = spacetime.append("svg")
          .attr("width", width)
          .attr("height", height)
          .append("g")
            .attr("transform", "translate(" + width / 2 + "," + height / 2 + ")");
         
        // Sun
        svg.append("circle")
          .attr("class", "sun")
          .attr("r", radii.sun)
          .style("fill", "rgba( 241, 165, 11 , 1.0)");
         
        // Planet's orbit
        svg.append("circle")
          .attr("class", "planetOrbit")
          .attr("r", radii.planetOrbit)
          .style("fill", "none")
          .style("stroke", "rgba(238, 89, 52, 0.25)");
         
        // Current position of Planet in its orbit
        var planetOrbitPosition = d3.arc()
          .outerRadius(radii.planetOrbit + 1)
          .innerRadius(radii.planetOrbit - 1)
          .startAngle(0)
          .endAngle(0);
        svg.append("path")
          .attr("class", "planetOrbitPosition")
          .attr("d", planetOrbitPosition)
          .style("fill", "rgba( 238, 89, 52 , 0.75)");
         
        // Planet
        svg.append("circle")
          .attr("class", "planet")
          .attr("r", radii.planet)
          .attr("transform", "translate(0," + -radii.planetOrbit + ")")
          .style("fill", "rgba( 241, 34, 14 , 1.0)");
         
        // Time of day
        var day = d3.arc()
          .outerRadius(radii.planet)
          .innerRadius(0)
          .startAngle(0)
          .endAngle(0);
        svg.append("path")
          .attr("class", "day")
          .attr("d", day)
          .attr("transform", "translate(0," + -radii.planetOrbit + ")")
          .style("fill", "rgba( 151, 36, 36 , 1.0)");
         
        // Update the clock every second
        setInterval(function () {
          now = new Date();
          
          var interpolatePlanetOrbitPosition = d3.interpolate(planetOrbitPosition.endAngle()(), (2 * Math.PI *(365/this.orbper)* d3.timeHours(d3.timeYear.floor(now), now).length / d3.timeHours(d3.timeYear.floor(now), d3.timeYear.ceil(now)).length));
          
          var interpolateDay = d3.interpolate(day.endAngle()(), (2 * Math.PI * d3.timeSeconds(d3.timeDay.floor(now), now).length / d3.timeSeconds(d3.timeDay.floor(now), d3.timeDay.ceil(now)).length));
        
          
          d3.transition().tween("orbit", function () {
            return function (t) {
              // Animate Planet orbit position
              d3.select(".planetOrbitPosition")
                .attr("d", planetOrbitPosition.endAngle(interpolatePlanetOrbitPosition(t)));
         
              // Transition Planet
              d3.select(".planet")
                .attr("transform", "translate(" + radii.planetOrbit * Math.sin(interpolatePlanetOrbitPosition(t) - planetOrbitPosition.startAngle()()) + "," + -radii.planetOrbit * Math.cos(interpolatePlanetOrbitPosition(t) - planetOrbitPosition.startAngle()()) + ")");
         
              // Animate day
              // Transition day
              d3.select(".day")
                .attr("d", day.endAngle(interpolateDay(t)))
                .attr("transform", "translate(" + radii.planetOrbit * Math.sin(interpolatePlanetOrbitPosition(t) - planetOrbitPosition.startAngle()()) + "," + -radii.planetOrbit * Math.cos(interpolatePlanetOrbitPosition(t) - planetOrbitPosition.startAngle()()) + ")");
            };
          });
        }, 1000);
    }
}
 
</script>

