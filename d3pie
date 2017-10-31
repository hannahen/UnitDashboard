window.PieChart = function PieChart(data, zoomable, docId) {

	var self = this;

  this.data = data || {};

  this.color = {"Student Media":"#E5797B",  //orange-red
                "Activations":"#90CD9F", //green
                "Out of Home": "#63AEDE", //blue
                "Available": "#FFFFFF",
                "Claimed": "#63AEDE"
                  };
                  
  this.dims = { 'width' : 120,
                'height': 120 };

  this.dims.radius = Math.min(this.dims.width, this.dims.height) / 2;
  this.zoomable = zoomable;

  this.docId = docId;
  
  this.pie_tooltip_percent = d3.select("#"+this.docId).append("div")
    .attr("class", "pie_tooltip_percent")
    .style("opacity", 0)
    .style("display","block")
    .style("position","absolute")
    .style("z-index","9999")
    .style("font-size","16px")
    .style("cursor", "default");

  this.pie_tooltip_product = d3.select("#"+this.docId).append("div")
    .attr("class", "pie_tooltip_product")
    .style("opacity", 0)
    .style("display","block")
    .style("position","absolute")
    .style("z-index","9999")
    .style("font-size","10px")
    .style("cursor", "default");

this.arc = d3.arc()
            .outerRadius(this.dims.radius - 10)
            .innerRadius(this.dims.radius - 22);

this.labelArc = d3.arc()
                  .outerRadius(this.dims.radius - 40)
                  .innerRadius(this.dims.radius - 22);

this.pie = d3.pie()
            .sort(null)
            .value(function(d) { return d.value; });

this.arcOver = d3.arc()
                  .outerRadius(this.dims.radius - 1)
                  .innerRadius(this.dims.radius - 22);

  this.init = function(context) {

  	context = context || self;

            var total = d3.sum(context.data, function(d) {
              return d.value;
            });
            for(var i = 0; i < context.data.length; i++){
               context.data[i].percentage = Math.round(context.data[i].value  / total * 100);
            }
              context.svg = d3.select("#"+context.docId).append("svg")
                  .attr("width", context.dims.width)
                  .attr("height", context.dims.height)
                  .append("g")
                  .attr("class", "grandArc")
                  .attr("transform", "translate(" + context.dims.width / 2 + "," + context.dims.height / 2 + ")");

	           context.g = context.svg.selectAll(".arc")
                    .data(context.pie(context.data))
                    .enter().append("path")
                    .attr("class", "arc");

                    context.g.attr("d", context.arc)
                    .style("stroke", "#fff")
                    .attr("stroke-width",1)
                    .style("fill", function(d) { return context.color[d.data.label]; })
                    .on("mouseenter", function(d) {
                      if(d.data.label == "Student Media") {
                        left = 26;
                      } else {
                        left = 30;
                      }
                      var coords = d3.mouse(this);
                      var color = context.color[d.data.label];
                    if(context.zoomable == true){  // idk
                       d3.select(this)
                         .transition()
                         .duration(200)
                         .attr("stroke-width",3)
                         .attr("d", context.arcOver);
                       }
                    context.pie_tooltip_percent.transition()
                          .duration(0)
                          .text(d.data.percentage + "%")
                          .style("opacity", .9)
                          .style("color", color)
                          .style("left",  38 + "%")
                          .style("top", 30 + "%");
                    context.pie_tooltip_product.transition()
                          .duration(0)
                          .text(d.data.label)
                          .style("opacity", .9)
                          .style("color", color)
                          .style("left",  left + "%")
                          .style("top", 50 + "%");
                  })
                  .on("mouseleave", function(d) {
                    if(context.zoomable == true) {
                        d3.select(this).transition()
                         .attr("d", context.arc)
                         .attr("stroke-width",1);
                       }
                    context.pie_tooltip_percent.transition()
                        .duration(500)
                        .style("opacity", 0);
                    context.pie_tooltip_product.transition()
                        .duration(500)
                        .style("opacity", 0);
                  });
  };

// dynamically update the piechart with new data!
  this.update = function(data, context){
	  context = context || self;

    var total = d3.sum(data, function(d) {
        return d.value;
     });
    data.forEach(function(d) {
        d.percentage = Math.round(d.value  / total * 100);
    });

    var newData = context.pie(data);
    var arcs = context.svg.selectAll("path")
                .data(newData)
                .attr("d", context.arc)

      var newArcs = arcs.enter().append("path")
          .attr("d", context.arc)
          .attr("class", "arc")
          .attr("stroke-width",1)
          .style("stroke", "#fff")
          .style("fill", function(d) { return context.color[d.data.label]; })
          .merge(arcs);
        arcs.exit().remove();

        newArcs.on("mouseover", function(d) {
              if(d.data.label == "Student Media") {
                        left = 26;
                      } else {
                        left = 30;
                      }

            var color = context.color[d.data.label];
            var coords = d3.mouse(this);
              if(context.zoomable == true){  // idk
                 d3.select(this)
                   .transition()
                   .duration(200)
                   .attr("stroke-width",3)
                   .attr("d", context.arcOver);
                 }
                    context.pie_tooltip_percent.transition()
                          .duration(0)
                          .text(d.data.percentage + "%")
                          .style("opacity", .9)
                          .style("color", color)
                          .style("left",  39 + "%")
                          .style("top", 30 + "%");
                    context.pie_tooltip_product.transition()
                          .duration(0)
                          .text(d.data.label)
                          .style("opacity", .9)
                          .style("color", color)
                          .style("left",  left + "%")
                          .style("top", 50 + "%");
                  })
                  .on("mouseout", function(d) {
                    if(context.zoomable == true) {
                        d3.select(this).transition()
                         .attr("d", context.arc)
                         .attr("stroke-width",1);
                       }
                    context.pie_tooltip_percent.transition()
                        .duration(500)
                        .style("opacity", 0);
                    context.pie_tooltip_product.transition()
                        .duration(500)
                        .style("opacity", 0);
                  });

                  }

                  this.init();
  };


