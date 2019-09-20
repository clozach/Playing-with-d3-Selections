<script>
  // https://bl.ocks.org/mbostock/3808234
  import { onMount } from "svelte";
  import Balloon from "./Balloon.svelte";

  let width;
  let height;

  const letterSpacing = 32;
  const travelDistance = 60;

  const balloonCreator = () => {
    const templateBalloon = document.getElementsByClassName(
      "template-balloon"
    )[0];

    console.log(`this: ${JSON.stringify(this)}`);

    // Inspired by https://stackoverflow.com/questions/18517376/d3-append-duplicates-of-a-selection @eagor
    var clone = templateBalloon.cloneNode(true);
    // clone.getElementsByTagName("text")[0].innerHTML = d;
    clone.classList.toggle("template-balloon");
    return clone;
  };

  onMount(() => {
    const alphabet = "abcdefghijklmnopqrstuvwxyz".split("");

    const g = d3.select("svg").select("g");

    function update(data) {
      var t = d3.transition().duration(750);

      // JOIN new data with old elements.
      var svgs = g
        .selectAll("svg")
        .data(data, d => d)
        .join(
          enter =>
            enter
              .append(balloonCreator)
              .attr("class", "enter")
              .attr("dy", ".35em") // 👈 https://stackoverflow.com/questions/19127035/what-is-the-difference-between-svgs-x-and-dx-attribute
              .attr("y", -travelDistance)
              .attr("x", function(d, i) {
                return i * letterSpacing;
              })
              .style("fill-opacity", 0)
              .call(e =>
                e
                  .transition(t)
                  .attr("y", 0)
                  .style("fill-opacity", 1)
              ),
          update =>
            update
              .attr("class", "update")
              .attr("y", 0)
              .style("fill-opacity", 1)
              .call(u =>
                u.transition(t).attr("x", function(d, i) {
                  return i * letterSpacing;
                })
              ),
          exit =>
            exit.attr("class", "exit").call(x =>
              x
                .transition(t)
                .attr("y", travelDistance)
                .style("fill-opacity", 0)
                .remove()
            )
        );
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
  <g id="balloon-group" transform="translate(32, 449.5)" />
  <defs>
    <g id="empty-balloon">
      <Balloon id="template" />
    </g>
  </defs>
</svg>
