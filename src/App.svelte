<script>
  // https://bl.ocks.org/mbostock/3808234
  import { onMount } from "svelte";
  import Balloon from "./Balloon.svelte";

  let width;
  let height;

  const letterSpacing = 32;
  const travelDistance = 60;

  onMount(() => {
    const alphabet = "abcdefghijklmnopqrstuvwxyz".split("");

    const svg = d3.select("svg");
    const g = svg
      .append("g")
      .attr("id", "balloon-group")
      .attr("transform", `translate(${letterSpacing}, ${height / 2})`);

    function update(data) {
      var t = d3.transition().duration(750);

      // JOIN new data with old elements.
      var svgs = g.selectAll("svg").data(data, d => d);

      // EXIT old elements not present in new data.
      svgs
        .exit()
        .attr("class", "exit")
        .transition(t)
        .attr("y", travelDistance)
        .style("fill-opacity", 0)
        .remove();

      // UPDATE old elements present in new data.
      svgs
        .attr("class", "update")
        .attr("y", 0)
        .style("fill-opacity", 1)
        .transition(t)
        .attr("x", function(d, i) {
          return i * letterSpacing;
        });

      // ENTER new elements present in new data.
      const appended = svgs.enter().append(() => {
        const templateBalloon = document.getElementsByClassName(
          "template-balloon"
        )[0];
        const balloonGroup = document.getElementById("balloon-group");

        console.log(balloonGroup);

        // Inspired by https://stackoverflow.com/questions/18517376/d3-append-duplicates-of-a-selection @eagor
        var clone = templateBalloon.cloneNode(true);
        // clone.getElementsByTagName("text")[0].innerHTML = d;
        clone.classList.toggle("template-balloon");
        // balloonGroup.append(clone);
        return clone;
      });

      const attred = appended
        .attr("class", "enter")
        .attr("dy", ".35em") // 👈 https://stackoverflow.com/questions/19127035/what-is-the-difference-between-svgs-x-and-dx-attribute
        .attr("y", -travelDistance)
        .attr("x", function(d, i) {
          return i * letterSpacing;
        })
        .style("fill-opacity", 0)
        // .text(function(d) {
        //   return d;
        // })
        .transition(t)
        .attr("y", 0)
        .style("fill-opacity", 1);
    }

    // The initial display.
    update(alphabet);

    // Grab a random sample of letters from the alphabet, in alphabetical order.
    d3.interval(function() {
      update(
        d3
          .shuffle(alphabet)
          .slice(0, Math.floor(Math.random() * 26))
          .sort()
      );
    }, 1500);
  });
</script>

<style>
  svg {
    font-family: "Helvetica", "Arial", sans-serif;
    height: 100%;
    width: 100%;
    background: linear-gradient(to top, #ddfdff, #6dd5fa, #2980b9);
    position: absolute;
    top: 0;
    left: 0;
    z-index: -1;
  }
</style>

<svelte:window bind:innerWidth={width} bind:innerHeight={height} />

<svg {width} {height}>
  <defs>
    <g id="empty-balloon">
      <Balloon id="template" />
    </g>
  </defs>
</svg>
