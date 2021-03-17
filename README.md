# techsights.github.io

## 6.1. Javascript

---

Javascript is used for the functions of datatables.

---

### 6.1.1. Javascript Function

---
This function able to create tables, columns and rows based on the csv.

```C
var CsvToHtmlTable = CsvToHtmlTable || {};

CsvToHtmlTable = {
    init: function (options) {
        options = options || {};
        var csv_path = options.csv_path || "";
        var el = options.element || "table-container";
        var allow_download = options.allow_download || true;
        var csv_options = options.csv_options || {};
        var datatables_options = options.datatables_options || {};
        var custom_formatting = options.custom_formatting || [];
        var customTemplates = {};
        $.each(custom_formatting, function (i, v) {
            var colIdx = v[0];
            var func = v[1];
            customTemplates[colIdx] = func;
        });

        var $table = $("<table class='table table-striped table-condensed display nowrap style = width:'100%'' id='" + el + "-table'></table>");
        var $containerElement = $("#" + el);
        $containerElement.empty().append($table);

        $.when($.get(csv_path)).then(
            function (data) {
                
                var csvData = $.csv.toArrays(data, csv_options);
                
                var $tableHead = $("<thead></thead>");
                var csvHeaderRow = csvData[0];
                var $tableHeadRow = $("<tr></tr>");
                for (var headerIdx = 0; headerIdx < csvHeaderRow.length; headerIdx++) {
                    $tableHeadRow.append($("<th></th>").text(csvHeaderRow[headerIdx]));
                }
                $tableHead.append($tableHeadRow);

                $table.append($tableHead);
                var $tableBody = $("<tbody></tbody>");
                for (var rowIdx = 1; rowIdx < csvData.length; rowIdx++) {
                    var $tableBodyRow = $("<tr></tr>");
                    for (var colIdx = 0; colIdx < csvData[rowIdx].length; colIdx++) {
                        var $tableBodyRowTd = $("<td></td>");
                        var cellTemplateFunc = customTemplates[colIdx];
                        var rowData = csvData[rowIdx][colIdx];
                        if (cellTemplateFunc) {
                            $tableBodyRowTd.html(cellTemplateFunc(rowData));
                        } else {
                            if (rowData.length > 15) {
                                rowData = rowData.substring(0, 25) + '...';
                            }
                            $tableBodyRowTd.text(rowData);
                        }
                        $tableBodyRow.append($tableBodyRowTd);
                        $tableBody.append($tableBodyRow);
                    }
                }
                $table.append($tableBody);
                
                $table.DataTable(datatables_options);

                if (allow_download) {
                    $containerElement.append("<p><a class='btn btn-info' href='" + csv_path + "'><i class='glyphicon glyphicon-download'></i> Download as CSV</a></p>");
                }
            });
    }
};

```

## 7.1. Website

---

Website is to display the 4 crawlers from Reddit, Stack Overflow, Twitter, Github and the sentiment data for each social media platform.

---

### 7.1.1. Head of HTML

---
This header for HTML is to input title and css for styling.

```C
    <!DOCTYPE html>
    <html lang="en">

    <head>
    <meta charset="utf-8" />
    <link rel="apple-touch-icon" sizes="76x76" href="../assets/img/apple-icon.png">
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
    <title>
        TechSights
    </title>
    <meta content='width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0, shrink-to-fit=no'
        name='viewport' />
    <!-- Fonts and icons  -->
    <link href="https://maxcdn.bootstrapcdn.com/font-awesome/latest/css/font-awesome.min.css" rel="stylesheet">
    <!-- CSS Files -->
    <link href="../assets/css/bootstrap.min.css" rel="stylesheet" />
    <link href="../assets/css/paper-dashboard.css?v=2.0.1" rel="stylesheet" />

    <!-- Bootstrap core CSS -->
    <link rel="stylesheet" href="css/bootstrap.min.css">
    <link rel="stylesheet" href="css/bootstrap-theme.min.css">
    <link href="css/dataTables.bootstrap.css" rel="stylesheet">

    <link rel="stylesheet" href="scss/paper-dashboard/_tables.scss">
    <link rel="stylesheet" href="scss/paper-dashboard/_images.scss">

</head>
```

---

### 7.2.1. Body of HTML

---
This body for HTML is to have the content of the datatables.

---

#### 7.2.2. Logo & Navigation Bar

---

This body for HTML is to have navigation bar, website logo.

```C
<body class="">
  <div class="wrapper ">
    <div class="sidebar" data-color="white" data-active-color="danger">
      <div class="logo">
        <a href="https://jzxr.github.io/techsights/" class="simple-text logo-normal">
          <div class="logo-image-small">
            <img src="/assets/img/AI.png">
          </div>
        </a>
      </div>
      <div class="sidebar-wrapper">
        <ul class="nav">

          <li class="active ">
            <a href="./index.html">
              <i class="nc-icon nc-tile-56"></i>
              <p>Table List</p>
            </a>
          </li>

        </ul>
      </div>
    </div>
    <div class="main-panel">
      <!-- Navbar -->
      <nav class="navbar navbar-expand-lg navbar-absolute fixed-top navbar-transparent">
        <div class="container-fluid">
          <div class="navbar-wrapper">
            <a class="navbar-brand" href="javascript:;">TechSights Datatables</a>
          </div>
          <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navigation"
            aria-controls="navigation-index" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-bar navbar-kebab"></span>
            <span class="navbar-toggler-bar navbar-kebab"></span>
            <span class="navbar-toggler-bar navbar-kebab"></span>
          </button>
          <div class="collapse navbar-collapse justify-content-end" id="navigation">
            <form>
              <div class="input-group no-border">
              </div>
            </form>
          </div>
        </div>
      </nav>
      <!--End of Navbar-->
```

#### 7.2.3. Reddit Datatables

---
This body content is for crawled Reddit information nformation to be displayed in a table. With the different tabs, it can toggle within 7 programming languages.

```C
    <div class="content">
        <div class="row">
          <div class="col-md-12">
            <div class="card">
              <div class="container-fluid">
                <div class="row">
                  <div class="col-lg-10">
                    <div class="table-responsive ">
                      <h2 class="title-7 m-b-40">Reddit</h2>
                      <ul class="nav nav-tabs" role="tablist">
                        <li role="presentation" class="active">
                          <a href="#tab-table1" role="tab" data-toggle="tab">C</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table2" role="tab" data-toggle="tab">C#</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table3" role="tab" data-toggle="tab">HTML</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table4" role="tab" data-toggle="tab">Java</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table5" role="tab" data-toggle="tab">Javascript</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table6" role="tab" data-toggle="tab">Python</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table7" role="tab" data-toggle="tab">R Programming</a>
                        </li>
                      </ul>
                      <div class="tab-content">
                        <div role="tabpanel" class="tab-pane active" id="tab-table1">
                          <div id='reddit-cprogramming'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table2">
                          <div id='reddit-csharp'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table3">
                          <div id='reddit-html'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table4">
                          <div id='reddit-java'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table5">
                          <div id='reddit-javascript'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table6">
                          <div id='reddit-python'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table7">
                          <div id='reddit-rprogramming'></div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
```

#### 7.2.4. Stack Overflow Datatables

---
This body content is for crawled stack overflow information to be displayed in a table. With the different tabs, it can toggle within 7 programming languages.

```C
        <div class="row">
          <div class="col-md-12">
            <div class="card">
              <div class="container-fluid">
                <div class="row">
                  <div class="col-lg-6">

                    <h2 class="title-7 m-b-40">Stackoverflow</h2>
                    <ul class="nav nav-tabs" role="tablist">
                      <li role="presentation">
                        <a href="#tab-table8" role="tab" data-toggle="tab">C</a>
                      </li>
                      <li role="presentation" class="active">
                        <a href="#tab-table9" role="tab" data-toggle="tab">C#</a>
                      </li>
                      <li role="presentation">
                        <a href="#tab-table10" role="tab" data-toggle="tab">HTML</a>
                      </li>
                      <li role="presentation">
                        <a href="#tab-table11" role="tab" data-toggle="tab">Java</a>
                      </li>
                      <li role="presentation">
                        <a href="#tab-table12" role="tab" data-toggle="tab">Javascript</a>
                      </li>
                      <li role="presentation">
                        <a href="#tab-table13" role="tab" data-toggle="tab">Python</a>
                      </li>
                      <li role="presentation">
                        <a href="#tab-table14" role="tab" data-toggle="tab">R Programming</a>
                      </li>
                    </ul>
                    <div class="tab-content">
                      <div role="tabpanel" class="tab-pane" id="tab-table8">
                        <div id='stackoverflow-cprogramming'></div>
                      </div>
                      <div role="tabpanel" class="tab-pane active" id="tab-table9">
                        <div id='stackoverflow-csharp'></div>
                      </div>
                      <div role="tabpanel" class="tab-pane " id="tab-table10">
                        <div id='stackoverflow-html'></div>
                      </div>
                      <div role="tabpanel" class="tab-pane " id="tab-table11">
                        <div id='stackoverflow-java'></div>
                      </div>
                      <div role="tabpanel" class="tab-pane " id="tab-table12">
                        <div id='stackoverflow-javascript'></div>
                      </div>
                      <div role="tabpanel" class="tab-pane " id="tab-table13">
                        <div id='stackoverflow-python'></div>
                      </div>
                      <div role="tabpanel" class="tab-pane " id="tab-table14">
                        <div id='stackoverflow-rprogramming'></div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
```

#### 7.2.5. Twitter Datatables

---
This body content is for crawled twitter information to be displayed in a table. There are two tables to display the recent posts and the top posts. With the different tabs, it can toggle within 7 technology terms for each table.

```C
<div class="row">
          <div class="col-md-12">
            <div class="card">
              <div class="container-fluid">
                <div class="row">
                  <div class="col-lg-10">
                    <div class="table-responsive ">
                      <h2 class="title-7 m-b-40">Twitter (Recent Posts)</h2>
                      <ul class="nav nav-tabs" role="tablist">
                        <li role="presentation" class="active">
                          <a href="#tab-table15" role="tab" data-toggle="tab">100 Days Of Code</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table16" role="tab" data-toggle="tab">Artificial
                            Intelligence</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table17" role="tab" data-toggle="tab">Data Science</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table18" role="tab" data-toggle="tab">Deep Learning</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table19" role="tab" data-toggle="tab">DEV Community</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table20" role="tab" data-toggle="tab">Machine Learning</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table21" role="tab" data-toggle="tab">Neural Network</a>
                        </li>
                      </ul>
                      <div class="tab-content">
                        <div role="tabpanel" class="tab-pane active" id="tab-table15">
                          <div id='recent-100DaysOfCode'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane" id="tab-table16">
                          <div id='recent-ArtificialIntelligence'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table17">
                          <div id='recent-DataScience'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table18">
                          <div id='recent-DeepLearning'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table19">
                          <div id='recent-DEVCommunity'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table20">
                          <div id='recent-MachineLearning'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table21">
                          <div id='recent-NeuralNetworks'></div>
                        </div>
                      </div>


                      <h2 class="title-7 m-b-40">Twitter (Popular Posts)</h2>
                      <ul class="nav nav-tabs" role="tablist">
                        <li role="presentation" class="active">
                          <a href="#tab-table22" role="tab" data-toggle="tab">100 Days Of Code</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table23" role="tab" data-toggle="tab">Artificial
                            Intelligence</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table24" role="tab" data-toggle="tab">Data Science</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table25" role="tab" data-toggle="tab">Deep Learning</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table26" role="tab" data-toggle="tab">DEV Community</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table27" role="tab" data-toggle="tab">Machine Learning</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table28" role="tab" data-toggle="tab">Neural Network</a>
                        </li>
                      </ul>
                      <div class="tab-content">
                        <div role="tabpanel" class="tab-pane active" id="tab-table22">
                          <div id='top-100DaysOfCode'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane" id="tab-table23">
                          <div id='top-ArtificialIntelligence'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table24">
                          <div id='top-DataScience'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table25">
                          <div id='top-DeepLearning'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table26">
                          <div id='top-DEVCommunity'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table27">
                          <div id='top-MachineLearning'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table28">
                          <div id='top-NeuralNetworks'></div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
```

#### 7.2.6. Github Datatables

---
This body content is for crawled github information to be displayed in a table. With the different tabs, it can toggle within 7 programming language projects.

```C
<div class="row">
          <div class="col-md-12">
            <div class="card">
              <div class="container-fluid">
                <div class="row">
                  <div class="col-lg-10">
                    <div class="table-responsive ">
                      <h2>Github Projects</h2>
                      <ul class="nav nav-tabs" role="tablist">
                        <li role="presentation" class="active">
                          <a href="#tab-table29" role="tab" data-toggle="tab">C Projects</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table30" role="tab" data-toggle="tab">C# Projects</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table31" role="tab" data-toggle="tab">HTML Projects</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table32" role="tab" data-toggle="tab">Java Projects</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table33" role="tab" data-toggle="tab">Javascript Projects</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table34" role="tab" data-toggle="tab">Python Projects</a>
                        </li>
                        <li role="presentation">
                          <a href="#tab-table35" role="tab" data-toggle="tab">R Projects</a>
                        </li>
                      </ul>
                      <div class="tab-content">
                        <div role="tabpanel" class="tab-pane active" id="tab-table29">
                          <div id='github-cprogramming'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane" id="tab-table30">
                          <div id='github-csharp'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table31">
                          <div id='github-html'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table32">
                          <div id='github-java'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table33">
                          <div id='github-javascript'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table34">
                          <div id='github-python'></div>
                        </div>
                        <div role="tabpanel" class="tab-pane " id="tab-table35">
                          <div id='github-rprogramming'></div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
```

#### 7.2.7. Sentiment Data & Charts

---
This body content is for data analytics to be displayed on website as pictures or gifs. For each social media platform, they have their own trend to be displayed.

```C
        <div class="row">
          <div class="col-md-12">
            <div class="card">
              <div class="container-fluid">
                <div class="row">
                  <div class="col-lg-10">
                    <div class="table-responsive ">
                      <div class="float-container">
                        <h2>Data Analytics</h2>
                        <div class="float-left">
                          <h3>Reddit</h3>
                          <h5>Description</h5>
                          <p>The pie chart shows the percentage of the amount of post for each programming
                            <br />language on reddit for year 2020 to 2021 as a whole.
                          </p>
                          <h5>Insights</h5>
                          <p>Topic with the highest amount of vote: Python, R Programming, C#</p>
                          <p> <em>Note: Insights are subjected to changes when there's an update of the crawler.</em>
                          </p>
                          <br />
                          <img src="reddit/reddit_prglanguage.png" alt="The trend of programming languages">
                          <p>Updated on: 14 March 2021</p>
                        </div>
                        <div class="float-right">
                          <h3>Stackoverflow</h3>
                          <h5>Description</h5>
                          <p>The pie chart shows the percentage of the amount of views for each programming
                            <br />languages on Stackoverflow as a whole.
                          </p>
                          <h5>Insights</h5>
                          <p>Topic with the highest amount of vote: HTML, C#, Javascript</p>
                          <p> <em>Note: Insights are subjected to changes when there's an update of the crawler.</em>
                          </p>
                          <br />
                          <img src="stackoverflow/stackoverflow_prglanguages.png"
                            alt="The trend of programming languages">
                          <p>Updated on: 14 March 2021</p>
                        </div>
                        <br />
                        <div class="float-left">
                          <h3>Twitter Topics</h3>
                          <h5>Description</h5>
                          <p>The comparative bar graph shows comparisons between the tweeter retweet count
                            <br />for each programming topics for each day over the past 8 days.
                            <br />This allows easier comparison for the number of twitter retweet for each topic
                            <br /> over time.
                          </p>
                          <img src="twitter/twitter_topics.png" alt="The trend of programming languages">
                          <p>Updated on: 14 March 2021</p>
                        </div>
                        <div class="float-right">
                          <h3>Twitter Popular Topics</h3>
                          <h5>Description</h5>
                          <p>The pie chart shows the percentage of the retweet count of
                            <br />twitter posts for each programming topic over the past 8 days as a whole.
                          </p>
                          <h5>Insights</h5>
                          <p>Topic with the highest amount of vote: DEVCommunity, 100DaysOfCode,<br /> NeuralNetworks.
                          </p>
                          <p> <em>Note: Insights are subjected to changes when there's an update of the crawler.</em>
                          </p>
                          <img src="twitter/twitter_populartopic.png" alt="The trend of programming languages">
                          <p>Updated on: 14 March 2021</p>
                        </div>

                        <div class="float-left">
                          <br />
                          <br />
                          <br />
                          <br />
                          <h3>Twitter Retweets</h3>
                          <h5>Description</h5>
                          <p>This is a moving graph that shows how the amount of retweets for
                            <br />each topic changed over the past 8 days.
                          </p>
                          <img src="twitter/retweetovertime.gif" alt="The trend of programming languages">
                          <p>Updated on: 14 March 2021</p>
                        </div>
                        <br />
                        <div class="float-right">
                          <h3>Github</h3>
                          <h5>Description</h5>
                          <p>The pie chart shows the percentage of the amount of votes each programming
                            <br />language on github as a whole.
                          </p>
                          <h5>Insights</h5>
                          <p>Topic with the highest amount of vote: Java, Javascript, Python.</p>
                          <p> <em>Note: Insights are subjected to changes when there's an update of the crawler.</em>
                          </p>
                          <img src="reddit/reddit_prglanguage.png" alt="The trend of programming languages">
                          <p>Updated on: 14 March 2021</p>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
```

#### 7.2.8. Scripts/Functions from Javascript

---

This body content

```C
<script type="text/javascript" src="js/jquery.min.js"></script>
        <script type="text/javascript" src="js/bootstrap.min.js"></script>
        <script type="text/javascript" src="js/jquery.csv.min.js"></script>
        <script type="text/javascript" src="js/jquery.dataTables.min.js"></script>
        <script type="text/javascript" src="js/dataTables.bootstrap.js"></script>
        <script type="text/javascript" src="js/convertcsv.js"></script>

        <script type="text/javascript">
          function format_link(link) {
            if (link)
              return "<a href='" + link + "' target='_blank'>" + "Link" + "</a>";
            else
              return "";
          }
          //Reddit
          CsvToHtmlTable.init({
            csv_path: 'reddit/reddit-c_programming.csv',
            element: 'reddit-cprogramming',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[3, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'reddit/reddit-csharp.csv',
            element: 'reddit-csharp',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[3, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'reddit/reddit-html.csv',
            element: 'reddit-html',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[3, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'reddit/reddit-java.csv',
            element: 'reddit-java',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[3, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'reddit/reddit-javascript.csv',
            element: 'reddit-javascript',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[3, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'reddit/reddit-Python.csv',
            element: 'reddit-python',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[3, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'reddit/reddit-rprogramming.csv',
            element: 'reddit-rprogramming',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[3, format_link]]
          });

          //Stackoverflow
          CsvToHtmlTable.init({
            csv_path: 'stackoverflow/stackoverflow-c_programming.csv',
            element: 'stackoverflow-cprogramming',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            datatables_options: { "fixedColumns": true },
            custom_formatting: [[2, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'stackoverflow/stackoverflow-csharp.csv',
            element: 'stackoverflow-csharp',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            datatables_options: { "fixedColumns": true },
            custom_formatting: [[2, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'stackoverflow/stackoverflow-html.csv',
            element: 'stackoverflow-html',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            datatables_options: { "fixedColumns": true },
            custom_formatting: [[2, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'stackoverflow/stackoverflow-java.csv',
            element: 'stackoverflow-java',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            datatables_options: { "fixedColumns": true },
            custom_formatting: [[2, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'stackoverflow/stackoverflow-javascript.csv',
            element: 'stackoverflow-javascript',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            datatables_options: { "fixedColumns": true },
            custom_formatting: [[2, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'stackoverflow/stackoverflow-Python.csv',
            element: 'stackoverflow-python',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            datatables_options: { "fixedColumns": true },
            custom_formatting: [[2, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'stackoverflow/stackoverflow-rprogramming.csv',
            element: 'stackoverflow-rprogramming',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            datatables_options: { "fixedColumns": true },
            custom_formatting: [[2, format_link]]
          });

          // Twitter
          CsvToHtmlTable.init({
            csv_path: 'twitter/recentpost/twitter_recent_100DaysOfCode.csv',
            element: 'recent-100DaysOfCode',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'twitter/recentpost/twitter_recent_ArtificialIntelligence.csv',
            element: 'recent-ArtificialIntelligence',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'twitter/recentpost/twitter_recent_DataScience.csv',
            element: 'recent-DataScience',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'twitter/recentpost/twitter_recent_DeepLearning.csv',
            element: 'recent-DeepLearning',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'twitter/recentpost/twitter_recent_DEVCommunity.csv',
            element: 'recent-DEVCommunity',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'twitter/recentpost/twitter_recent_MachineLearning.csv',
            element: 'recent-MachineLearning',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'twitter/recentpost/twitter_recent_NeuralNetwork.csv',
            element: 'recent-NeuralNetworks',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });

          // Twitter (Top Posts)
          CsvToHtmlTable.init({
            csv_path: 'twitter/toppost/twitter_top_100DaysOfCode.csv',
            element: 'top-100DaysOfCode',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'twitter/toppost/twitter_top_ArtificialIntelligence.csv',
            element: 'top-ArtificialIntelligence',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'twitter/toppost/twitter_top_DataScience.csv',
            element: 'top-DataScience',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'twitter/toppost/twitter_top_DeepLearning.csv',
            element: 'top-DeepLearning',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'twitter/toppost/twitter_top_DEVCommunity.csv',
            element: 'top-DEVCommunity',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'twitter/toppost/twitter_top_MachineLearning.csv',
            element: 'top-MachineLearning',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });

          CsvToHtmlTable.init({
            csv_path: 'twitter/toppost/twitter_top_NeuralNetwork.csv',
            element: 'top-NeuralNetworks',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });

          // Github
          CsvToHtmlTable.init({
            csv_path: 'github/github-c_programming.csv',
            element: 'github-cprogramming',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });
          CsvToHtmlTable.init({
            csv_path: 'github/github-csharp.csv',
            element: 'github-csharp',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });
          CsvToHtmlTable.init({
            csv_path: 'github/github-html.csv',
            element: 'github-html',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });
          CsvToHtmlTable.init({
            csv_path: 'github/github-java.csv',
            element: 'github-java',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });
          CsvToHtmlTable.init({
            csv_path: 'github/github-javascript.csv',
            element: 'github-javascript',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });
          CsvToHtmlTable.init({
            csv_path: 'github/github-Python.csv',
            element: 'github-python',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });
          CsvToHtmlTable.init({
            csv_path: 'github/github-rprogramming.csv',
            element: 'github-rprogramming',
            allow_download: true,
            csv_options: { separator: ',', delimiter: '"' },
            datatables_options: { "paging": true },
            custom_formatting: [[6, format_link]]
          });
        </script>
```
