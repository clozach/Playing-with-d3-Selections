<script>
  import { onMount } from "svelte";
  import { datas } from "./DataSource.svelte";

  onMount(() => {
    var array = datas[0];

    function display() {
      var selection = d3
        .select("#wrapper")
        .selectAll("div")
        .data(array, d => d.age);

      selection.selectAll("div").classed("new", false);

      var selectionEnter = selection
        .enter()
        .append("div")
        .classed("new", true)
        .classed("person", true);

      selectionEnter
        .append("p")
        .classed("name", true)
        .html(d => d.name);

      selectionEnter
        .append("p")
        .classed("age", true)
        .html(d => d.age);

      selection
        .exit()
        .transition()
        .duration(2000)
        .styleTween("color", function() {
          return d3.interpolate("red", "black");
        })
        .remove();

      selectionEnter
        .transition()
        .duration(2000)
        .styleTween("color", function() {
          return d3.interpolate("green", "salmon");
        });

      selection.select(".name").html(d => d.name);

      selection.select(".age").html(d => d.age);
    }

    display();

    const pause = 3000;

    setTimeout(function() {
      array = datas[1];
      display();

      setTimeout(function() {
        array = datas[2];
        display();

        setTimeout(function() {
          array = datas[3];
          display();

          setTimeout(function() {
            array = datas[4];
            display();
          }, pause);
        }, pause);
      }, pause);
    }, pause);
  });
</script>

<style>
  .new {
    color: red;
  }
</style>

<div id="wrapper" />
