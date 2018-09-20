---
layout: page
title: CV
permalink: /cv/
published: true
---
<script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>

## Professional experience

<script type="text/javascript">
  google.charts.load("current", {packages:["timeline"]});
  google.charts.setOnLoadCallback(drawChart);
  function drawChart() {
    var container = document.getElementById('experience_chart');
    var chart = new google.visualization.Timeline(container);
    var dataTable = new google.visualization.DataTable();
    dataTable.addColumn({ type: 'string', id: 'Position' });
    dataTable.addColumn({ type: 'string', id: 'Name' });
    dataTable.addColumn({ type: 'date', id: 'Start' });
    dataTable.addColumn({ type: 'date', id: 'End' });
    dataTable.addRows([
        [ 'Dates', 'GICO', new Date(2008, 07, 01), new Date(2009, 11, 01) ],
        [ 'Dates', 'TES', new Date(2009, 11, 01), new Date(2010, 08, 01) ],
        [ 'Dates', 'UAB', new Date(2010, 09, 01), new Date(2011, 07, 01) ],
        [ 'Dates', 'IFAE', new Date(2011, 07, 01), new Date(2012, 10, 01) ],
        [ 'Dates', 'ISGlobal', new Date(2013, 09, 01), new Date(2018, 02, 01) ],
        [ 'Dates', 'BCH', new Date(2018, 02, 01), new Date(2019, 02, 01) ],
    ]);

    chart.draw(dataTable);
  }
</script>

<div id="experience_chart" style="height: 200px;"></div>

* Bioinformatic Analyst / _Boston Children's Hospital_ / Feb 2018 - PRESENT
* Bioinformatic Analyst / _Barcelona Institute for Global Health_ / Nov 2013 - Jan 2018
* Internship / _Barcelona Institute for Global Health_ / Feb 2013 - Sep 2013
* Software Developer / _Institute of High Energy Physics_ / Sep 2011 - Aug 2012
* Internship / _Institute of High Energy Physics_ / Jun 2011 - Aug 2011
* Teaching Assistant / _Universitat Autonoma de Barcelona_ / Sep 2010 - Jun 2011
* Full Stack Developer / _Universitat Autonoma de Barcelona_ / Nov 2009 - Jul 2010
* Full Stack Developer (Jr) / _GICO Sistemas de Gestion_ / Jul 2008 - Oct 2009

## Education


<script type="text/javascript">
  google.charts.load("current", {packages:["timeline"]});
  google.charts.setOnLoadCallback(drawChart);
  function drawChart() {
    var container = document.getElementById('education_chart');
    var chart = new google.visualization.Timeline(container);
    var dataTable = new google.visualization.DataTable();
    dataTable.addColumn({ type: 'string', id: 'Position' });
    dataTable.addColumn({ type: 'string', id: 'Name' });
    dataTable.addColumn({ type: 'date', id: 'Start' });
    dataTable.addColumn({ type: 'date', id: 'End' });
    dataTable.addRows([
        [ 'Dates', 'BSc in Computer Sciences', new Date(2005, 05, 15), new Date(2012, 07, 01) ],
        [ 'Dates', 'MSc in Bioinformatics', new Date(2012, 09, 15), new Date(2013, 09, 15) ],
        [ 'Dates', 'PhD in Biomedicine', new Date(2013, 11, 01), new Date(2017, 11, 17) ],
    ]);

    chart.draw(dataTable);
  }
</script>

<div id="education_chart" style="height: 200px;"></div>

* PhD in Biomedicine / _Universitat Pompeu Fabra_ / 2017
* MSc in Bioinformatics / _Universitat Autonoma de Barcelona_ / 2013
* BSc in Computer Sciences / _Universitat Autonoma de Barcelona_ /  2012

## Interests

<script type="text/javascript">
    google.charts.load("current", {packages:["corechart"]});
    google.charts.setOnLoadCallback(drawChart);
    function drawChart() {
    var data = google.visualization.arrayToDataTable([
        ['Task', 'Hours per Day'],
        ['Software Development',       10],
        ['Bioinformatics',              5],
        ['Data Visualization',          4],
        ['Public Health',               2],
        ['Omic Data Analysis',          7],
        ['python/R',                    7]
    ]);

    var options = {

        pieHole: 0.4,
    };

    var chart = new google.visualization.PieChart(document.getElementById('interests'));
    chart.draw(data, options);
    }
</script>

<div id="interests" style="width: 200px; height: 200px;"></div>

* Software Development
* Bioinformatics
* Data Visualization
* Public Health
* Omic Data Analysis
* Environmental Epidemiology
* Information Retrieval
* R, python