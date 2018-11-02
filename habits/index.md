---
layout: default
title: Habit Tracker
permalink: /habits/
---

<script src="d3.min.js" charset="utf-8" integrity="sha512-L5K9Bf852XyB+wrvRFGwWzfhVI+TZqJlgwzX9yvrfhILuzIZfrcQO4au9D9eVDnkQ6oqYr9v2QwJdFo+eKE50Q=="></script>
<script src="moments.min.js" charset="utf-8" integrity="sha512-O2Y8hD83PQtRf8vcr0N+yxwRtErIVaHJ4NOpojzq2yvUmhiJbQIT9OAYu27t+mVk814t+ongBVGx+YGylICVkQ=="></script>
<link rel="stylesheet" type="text/css" href="calendar-heatmap.css" integrity="sha512-CWReRS3h2V/ugg18r/xzg7dvAQ+1E0ZiisYLDwLib77xv9ayK+A/2te7ms8UmHFvFrob2Chpm4gs0bp022KSLg=="> 
<script src="calendar-heatmap.js" integrity="sha512-SQh+WlsNLlt65T+YuXhKsN/+hnf167xtHmT4jvOuNbxys/AYxMxKJQmrBYVCN3PNP8yBuglVwd5rFwIrng8DVQ=="></script>
<script src="jquery-3.3.1.min.js" integrity="sha512-pplvUCmFjG3m3jDtpU+KzEfZcTyxrcV2Fzzo9195orlEucBL+lWtYoKecFzt5Py3x8kHhejNPgJS15oYaxdgpw=="></script>

  <div class="habit_map"></div>

  <script type="text/javascript">

    var chartData;
    $.getJSON("datas.json",function( json ){

      chartData=json.chartData;
      var now = moment().endOf('day').toDate();
    var yearAgo = moment().startOf('day').subtract(1, 'year').toDate();
    var heatmap = calendarHeatmap()
      .data(chartData)
      .selector('.habit_map')
      .tooltipEnabled(true)
      .colorRange(['#f4f7f7', '#79a8a9']);
  
    heatmap();  // render the chart
    });
  
    
  </script>

