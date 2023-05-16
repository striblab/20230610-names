<script>
import * as d3 from 'd3';
import c3 from 'c3';

import { onMount } from 'svelte';
import jqx from 'jquery';

import rows from './data/names.json';
import rows20b from './data/top20_boys.json';
import rows20g from './data/top20_girls.json';

var data = rows.names;
var data20b = rows20b.tops;
var data20g = rows20g.tops;

function toTitleCase(str)
{
    return str.replace(/\w\S*/g, function(txt){return txt.charAt(0).toUpperCase() + txt.substr(1).toLowerCase();});
}

function urlParam(name) {
  var results = new RegExp('[\\?&]' + name + '=([^&#]*)').exec(window.location.href);
  if (results != null) {
    return decodeURIComponent(results[1]);
  } else {
    return 0;
  }
}

var mainname = "";

var chart;

function switchChart(name,initial){

    var axis = [];
    var dataStreamM = ['M'];
    var dataStreamF = ['F'];
    axis[0] = 'x';
    var indexYear = 1;
    var year = 0;
    var year2 = 0;
    var rate = 0;
    var rate2 = 0;
    var birthNum = 0;
    var birthNum2 = 0;

    for (var j=1930; j<=2021; j++){
    axis[indexYear] = j;
    dataStreamM[indexYear] = 0;
    dataStreamF[indexYear] = 0;
    indexYear++;
    }

  var found = false;
  var index = 0;

  for (var i=0; i < data.length; i++){
    if (name == data[i].name && data[i].gender == "M"){
      found = true;
      index = i;
      for (var k=1; k < dataStreamM.length; k++){
        if (data[index].name != name){ break; }
        if (axis[k] == data[index].year) { 
          dataStreamM[k] = data[index].rate; 
          rate = data[index].rate;
          year = data[index].year;
          birthNum = data[index].births;
          index++; 
        }
      }
  }
}

index = 0;

  for (var i=0; i < data.length; i++){
    if (name == data[i].name && data[i].gender == "F"){
      found = true;
      index = i;
      for (var k=1; k < dataStreamF.length; k++){
        if (data[index].name != name){ break; }
        if (axis[k] == data[index].year) { 
          dataStreamF[k] = data[index].rate; 
          rate2 = data[index].rate;
          year2 = data[index].year;
          birthNum2 = data[index].births;
          index++; 
        }
      }
  }
}

dataStreamM[dataStreamM.length] = null;
dataStreamM[dataStreamM.length] = null;
dataStreamM[dataStreamM.length] = null;
dataStreamM[dataStreamM.length] = null;
dataStreamM[dataStreamM.length] = null;
dataStreamM[dataStreamM.length] = null;
dataStreamM[dataStreamM.length] = null;
dataStreamM[dataStreamM.length] = null;
dataStreamM[dataStreamM.length] = null;
dataStreamM[dataStreamM.length] = null;
dataStreamM[dataStreamM.length] = null;
dataStreamM[dataStreamM.length] = null;
dataStreamF[dataStreamF.length] = null;
dataStreamF[dataStreamF.length] = null;
dataStreamF[dataStreamF.length] = null;
dataStreamF[dataStreamF.length] = null;
dataStreamF[dataStreamF.length] = null;
dataStreamF[dataStreamF.length] = null;
dataStreamF[dataStreamF.length] = null;
dataStreamF[dataStreamF.length] = null;
dataStreamF[dataStreamF.length] = null;
dataStreamF[dataStreamF.length] = null;
dataStreamF[dataStreamF.length] = null;
dataStreamF[dataStreamF.length] = null;
axis[axis.length] = "2022";
axis[axis.length] = "2023";
axis[axis.length] = "2024";
axis[axis.length] = "2025";
axis[axis.length] = "2026";
axis[axis.length] = "2027";
axis[axis.length] = "2028";
axis[axis.length] = "2029";
axis[axis.length] = "2030";

if (year != 2021) {
          rate = 0;
          year = 2021;
          birthNum = 0;
}

if (year2 != 2021) {
          rate2 = 0;
          year2 = 2021;
          birthNum2 = 0;
}

jqx("#infobox").html('<div class="chart-tooltip">' +
  '<div class="tooltip-label">' + year + '</div></div>' +
  '<div class="chart-tooltip"><div class="tooltip-label">Rate</div>' + 
  '<div class="tooltip-value" style="color:#dea381;font-weight:900;">' + d3.format(".1f")(rate) + '</div></div>' +
  '<div class="chart-tooltip"><div class="tooltip-label">Births</div>' +
  '<div class="tooltip-value">' + d3.format(",")(birthNum) + '</div>' +
  '</div>');

jqx("#infobox2").html('<div class="chart-tooltip ">' +
    '<div class="tooltip-label">' + year2 + '</div></div>' +
    '<div class="chart-tooltip"><div class="tooltip-label">Rate</div>' + 
    '<div class="tooltip-value" style="color:#B6AED4;font-weight:900;">' + d3.format(".1f")(rate2) + '</div></div>' +
    '<div class="chart-tooltip"><div class="tooltip-label">Births</div>' +
    '<div class="tooltip-value">' + d3.format(",")(birthNum2) + '</div>' +
    '</div>');


    if (initial == true) {


var  padding = {
        top: 20,
        right: 60,
        bottom: 20,
        left: 60,
    };

var share = "#B0BEC5";

 chart = c3.generate({
        bindto: '#chart',
        padding: padding,
    data: {
        x: 'x',
        columns: [
            axis,
            dataStreamM,
            dataStreamF
        ],
        type: 'line',
        colors: {
            'M': '#DEA381',
            'F': '#B6AED4'
        }
    },
        legend: {
            show: false
        },
        line: {
            connectNull: true,
        },
        point: {
            show: true,
            r: function(d) { if (d.x == 2021) { return 3;} else { return 2; } }
        },
    axis: {
      y: {
            min: 0,
            padding: {bottom: 0},
            tick: {
             count: 4,
             format: d3.format('.1f')
            }
        },
        x: {
        	padding: {right: 0, left: 0},
            tick: {
                values: [1930, 1950, 1970, 1990, 2010, 2030],
                count: 4,
                multiline: false
            }
          }
        },
        grid: {
            focus:{
                show:false
              },
              x: {
                lines: [{
                    value: 2022,
                    text: '',
                    position: 'end',
                    class: 'powerline'
                }]

            }
          },
      tooltip: {
        contents: function(d, defaultTitleFormat, defaultValueFormat, color) {

        var birthNum = 0;
        var birthNum2 = 0;

        for (var k=0; k < data.length; k++){
          if (data[k].name == mainname && data[k].year == d[0].x && data[k].gender == "M"){
            birthNum = data[k].births;
            break;
          }
        }

        for (var w=0; w < data.length; w++){
          if (data[w].name == mainname && data[w].year == d[1].x && data[w].gender == "F"){
            birthNum2 = data[w].births;
            break;
          }
        }

          jqx("#infobox").html('<div class="chart-tooltip">' +
            '<div class="tooltip-label">' + d[0].x + '</div></div>' +
            '<div class="chart-tooltip"><div class="tooltip-label">Rate</div>' + 
            '<div class="tooltip-value" style="color:#dea381;font-weight:900;">' + defaultValueFormat(d[0].value) + '</div></div>' +
            '<div class="chart-tooltip"><div class="tooltip-label">Births</div>' +
            '<div class="tooltip-value">' + d3.format(",")(birthNum) + '</div>' +
            '</div>');

          jqx("#infobox2").html('<div class="chart-tooltip ">' +
            '<div class="tooltip-label">' + d[1].x + '</div></div>' +
            '<div class="chart-tooltip"><div class="tooltip-label">Rate</div>' + 
            '<div class="tooltip-value" style="color:#B6AED4;font-weight:900;">' + defaultValueFormat(d[1].value) + '</div></div>' +
            '<div class="chart-tooltip"><div class="tooltip-label">Births</div>' +
            '<div class="tooltip-value">' + d3.format(",")(birthNum2) + '</div>' +
            '</div>');

        }
      }
});
    } else if (initial == false) {
        jqx("#message").html("Rate per 10,000 births by year"); 
        chart.load({
            columns:[
                dataStreamM,
                dataStreamF
            ]
        });
    }

if (found == false) { 
    jqx("#message").html("Name not found"); 
    chart.load({
        unload: ['M', 'F']
    });
}

}


    function chartTypeM() {

        var padding = {
            top: 20,
            right: 60,
            bottom: 20,
            left: 100,
        };

        var chartType = c3.generate({
            bindto: "#namesGrid",
            padding: padding,
            data: {
                x: 'x',
                columns: [
                    ["x","Walker","Ace","Hayes","Thiago","Archie","Adriel","Legend","Crew","Watson","Callum","Mohamedamin","Khalil","Callahan","Mac","Apollo","Zion","Barrett","Casey","Davis","Kobe","Koda","Kylo","Niko","Luka","Forrest"],
                    ['value', 4.527,4.273,3.488,3.182,3.133,3,2.867,2.805,2.692,2.515,2.333,2.091,2.077,1.933,1.923,1.769,1.692,1.667,1.667,1.667,1.667,1.667,1.533,1.476,1.462]
                ],
                type: 'bar',
                labels: {
                    format: {
                        'value': d3.format('+.0%')
                    }
                }
            },
            transition: {
                duration: 400
            },
            legend: {
                show: false
            },
            tooltip: {
                show: false
            },
            color: {
                pattern: ['#dea381']
            },
            axis: {
                rotated: true,
                y: {
                    max: 6,
                    min: 0,
                    padding: {
                        bottom: 0,
                        top: 0
                    },
                    tick: {
                        values: [0, 3, 6],
                        format: d3.format('+.0%')
                    }
                },
                x: {
                    type: 'category',
                    tick: {
                        multiline: false
                    }
                }
            },
            grid: {
                focus:{
                    show:false
                  },
              }
        });
    }


    function chartTypeF() {

            var padding = {
                top: 20,
                right: 60,
                bottom: 20,
                left: 100,
            };

            var chartType = c3.generate({
                bindto: "#namesGrid2",
                padding: padding,
                data: {
                    x: 'x',
                    columns: [
                          ["x","Everleigh","Sutton","Maeve","Margo","Lainey","Collins","Sage","Astrid","Octavia","Hallie","Loretta","Florence","Elisa","Leni","Navy","Blakely","Carmen","Oaklynn","Blair","Matilda","Raya","Wren","Winter","Annalise","Julianna"],
                          ["value",5.152,4.8,3.667,3.32,3.288,2.897,2.424,2.273,2.211,2.152,2,1.879,1.81,1.81,1.81,1.798,1.793,1.763,1.739,1.739,1.72,1.66,1.606,1.571,1.571]
                    ],
                    type: 'bar',
                    labels: {
                        format: {
                            'value': d3.format('+.0%')
                        }
                    }
                },
                legend: {
                    show: false
                },
                tooltip: {
                    show: false
                },
                color: {
                    pattern: ['#B6AED4']
                },
                axis: {
                    rotated: true,
                    y: {
                        max: 6,
                        min: 0,
                        padding: {
                            bottom: 0,
                            top: 0
                        },
                        tick: {
                            values: [0, 3, 6],
                            format: d3.format('+.0%')
                        }
                    },
                    x: {
                        type: 'category',
                        tick: {
                            multiline: false
                        }
                    }
                },
                grid: {
                    focus:{
                        show:false
                      },
                  }
            });

        }


    onMount(() => {
        if (urlParam('name') != 0 ) { 
             mainname = toTitleCase(urlParam('name')); 

      jqx("#named, #named2").html(mainname);

    } else {
        mainname = "Everleigh";
        jqx("#named, #named2").html(mainname);
    }


		switchChart(mainname,true);

		jqx( document ).ready(function() {

		 jqx('#filter_box').keyup(function(e){
		  mainname = toTitleCase(jqx('#filter_box').val()); 

		      if(e.keyCode == 13)
		      {
		          jqx("#named, #named2").html(mainname);
		          switchChart(mainname,false);
		          history.pushState({urlPath:'/?name=' + mainname},"",'./?name=' + mainname);
		          // window.history.href = './?chart=lookup&name=' + jqx('#filter_box').val();
		        
		      }

		      return false;
		  });
		});

		//top 20 boys
		  for (var i=0; i < data20b.length; i++){
		    jqx("#namesList").append('<div class="chart-tooltip"><div class="tooltip-label">' + data20b[i].NAME + '</div>' + 
		            '<div class="tooltip-value" style="color:#dea381;font-weight:900;">' + d3.format(".1f")(data20b[i].RatePer10k) + '</div></div>');
		  }

		//top 20 girls
		  for (var i=0; i < data20g.length; i++){
		    jqx("#namesList2").append('<div class="chart-tooltip"><div class="tooltip-label">' + data20g[i].NAME + '</div>' + 
		            '<div class="tooltip-value" style="color:#B6AED4;font-weight:900;">' + d3.format(".1f")(data20g[i].RatePer10k) + '</div></div>');
		  }
		chartTypeM();
		chartTypeF();

        console.log("loaded");
    });
</script>


<div class="strib-styles ssa ssb ssc">

    <section class="container-lg leadin">
        <p>Each year, the Social Security Administration releases data showing the popularity of first names throughout the country, state-by-state.  And each year, the Star Tribune analyzes that data to determine which names are most popular in Minnesota, both currently and over time.</p>
        
        <p>Enter a name into the search box to reveal how it's trended in Minnesota from the early 20th century to now.</p>

        <p>&nbsp;</p>
    </section>


    <div id="wrapper">

      <div class="slide" id="lookup">
    <div id="filter"><input type="search" id="filter_box" placeholder="Type name and press enter" /></div>

    <h2 class="chartTitle">Popularity of the name <span id="named"></span> in Minnesota</h2>
    <div class="chatter" id="message">Rate per 10,000 births by year</div>

    <div id="chart"></div>

    <div class="infobox legendary orange3">Male</div>
    <div class="infobox legendary purple2">Female</div>

    <div class="infobox chartBox" id="infobox"></div>
    <div class="infobox chartBox" id="infobox2"></div>

    </div>

    <div class="rule"></div>
      <div class="slide" id="rank">
        <h2 class="chartTitle">Top Minnesota names</h2>
        <div class="chatter">The most popular male and female names in 2022.</div>
        <div class="breaker"></div>

    <div class="infobox listBox" id="nameBox">

      <div class="legendary orange3">
        <div class="chart-tooltip">
          <div class="tooltip-label">Male names</div>
          <div class="tooltip-value">Rate</div>
          </div>
      </div>

      <div id="namesList"></div>

    </div>
    <div class="infobox listBox" id="nameBox2">

      <div class="legendary purple2">
        <div class="chart-tooltip">
        <div class="tooltip-label">Female names</div>
        <div class="tooltip-value">Rate</div>
        </div>
      </div>

      <div id="namesList2"></div>

    </div>
  </div>


  <div class="rule"></div>
  <div class="slide" id="trend">
        <h2 class="chartTitle">Trending Minnesota names</h2>
        <div class="chatter">Names with the largest rate change between 2018 and 2022.</div>
        <div class="breaker"></div>

    <div class="infobox listBox" id="nameBox">

      <div class="legendary orange3">Male names</div>

      <div id="namesGrid" class="grid"></div>
    </div>
    <div class="infobox listBox" id="nameBox2">

      <div class="legendary purple2">Female names</div>

      <div id="namesGrid2" class="grid"></div>

    </div>

    <div class="breaker"></div>
    <div class="source">Data is based on Social Security applications and names are only listed in the data if there were at least five babies, of the same gender, with that name in a given year. Names are broken down by either male or female; those are the only options provided on the Society Security application form.</div>
    <div class="byline">Source: Social Security Administration</div>
  </div>

  </div>

</div>