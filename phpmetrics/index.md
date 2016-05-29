---
layout: default
title: PhpMetrics Report
---
<style type="text/css">{% include_relative phpmetrics.css %}</style>
<div class="phpmetrics container">
    <div class="row">
        <div class="well accessibility-box">
            <label>
                <input type="checkbox" onclick="toggleAccessibility()"> I'm colorblind
            </label>
        </div>

        <div class="menu-box">
            <ul id="menu" class="menu" >
                <li id="link-overview" class="active"><a >Overview</a></li>
                <li id="link-score"><a >Evaluation</a></li>
                <li id="link-relations"><a  >Relations map</a></li>
                <li id="link-repartition"><a  >Repartition</a></li>
                <li id="link-explore"><a >Explore</a></li>
                
                <li id="link-help"><a  >Help</a></li>
            </ul>
        </div>


    </div>


    <div class="tab-content row">


        <!-- General details -->
        <div class="row tab active" id="overview">
            <div class="col-3">
                <h3>Evaluation</h3>
                <small><a id="btn-save-maintainability" download="phpmetric-maintainability.svg">Download (as SVG)</a> | <a onclick="return zoom('svg-maintainability', updateMaintainabilityChart);">zoom</a></small>
                <div id="svg-maintainability"><svg /></div>

                <div class="well ">

                    <h4>Information</h4>
                    <p>
                        Each file is symbolized by a circle.
                        Size of the circle represents the Cyclomatic complexity.
                        Color of the circle represents the Maintainability Index.
                    </p>
                    <p>Large red circles will be probably hard to maintain.</p>
                </div>

            </div>
            <div class=" col-3">

                <h3 id="title-custom">Custom chart</h3>
                <small><a id="btn-save-custom" download="phpmetric-custom.svg">Download (as SVG)</a> | <a onclick="return zoom('svg-custom', updateCustomChart);">zoom</a></small>
                <div id="svg-custom"><svg /></div>

                <div class="well">
                    <h4>Configuration</h4>
                    <p>
                        Select metrics you want to display in chart.
                    </p>
                    <table class="table table-condensed table-bordered">
                        <tr>
                            <td>X Axis</td>
                            <td>Y Axis</td>
                            <td>Diameter</td>
                        </tr>
                        <tr>
                            <td>
                                <select id="xAxis" class="form-control" onchange="xAxis = this.value; updateCustomChart();">
                                                                            <option value="afferentCoupling">Afferent Coupling </option>
                                                                            <option value="bugs">Bugs </option>
                                                                            <option value="commentWeight">Comment Weight </option>
                                                                            <option value="cyclomaticComplexity">Cyclomatic Complexity </option>
                                                                            <option value="dc">Dc </option>
                                                                            <option value="difficulty">Difficulty </option>
                                                                            <option value="efferentCoupling">Efferent Coupling </option>
                                                                            <option value="effort">Effort </option>
                                                                            <option value="filename">Filename </option>
                                                                            <option value="instability">Instability </option>
                                                                            <option value="intelligentContent">Intelligent Content </option>
                                                                            <option value="lcom">Lcom </option>
                                                                            <option value="length">Length </option>
                                                                            <option value="loc">Loc </option>
                                                                            <option value="logicalLoc">Logical Loc </option>
                                                                            <option value="maintainabilityIndex">Maintainability Index </option>
                                                                            <option value="maintainabilityIndexWithoutComment">Maintainability Index Without Comment </option>
                                                                            <option value="myerDistance">Myer Distance </option>
                                                                            <option value="myerInterval">Myer Interval </option>
                                                                            <option value="name">Name </option>
                                                                            <option value="noc">Noc </option>
                                                                            <option value="noc-anon">Noc-anon </option>
                                                                            <option value="noca">Noca </option>
                                                                            <option value="nocc">Nocc </option>
                                                                            <option value="noi">Noi </option>
                                                                            <option value="nom">Nom </option>
                                                                            <option value="operators">Operators </option>
                                                                            <option value="rdc">Rdc </option>
                                                                            <option value="rsc">Rsc </option>
                                                                            <option value="rsysc">Rsysc </option>
                                                                            <option value="sc">Sc </option>
                                                                            <option value="sysc">Sysc </option>
                                                                            <option value="time">Time </option>
                                                                            <option value="vocabulary">Vocabulary </option>
                                                                            <option value="volume">Volume </option>
                                                                    </select>
                            </td>
                            <td>
                                <select id="yAxis" class="form-control" onchange="yAxis = this.value; updateCustomChart();">
                                                                            <option value="afferentCoupling">Afferent Coupling </option>
                                                                            <option value="bugs">Bugs </option>
                                                                            <option value="commentWeight">Comment Weight </option>
                                                                            <option value="cyclomaticComplexity">Cyclomatic Complexity </option>
                                                                            <option value="dc">Dc </option>
                                                                            <option value="difficulty">Difficulty </option>
                                                                            <option value="efferentCoupling">Efferent Coupling </option>
                                                                            <option value="effort">Effort </option>
                                                                            <option value="filename">Filename </option>
                                                                            <option value="instability">Instability </option>
                                                                            <option value="intelligentContent">Intelligent Content </option>
                                                                            <option value="lcom">Lcom </option>
                                                                            <option value="length">Length </option>
                                                                            <option value="loc">Loc </option>
                                                                            <option value="logicalLoc">Logical Loc </option>
                                                                            <option value="maintainabilityIndex">Maintainability Index </option>
                                                                            <option value="maintainabilityIndexWithoutComment">Maintainability Index Without Comment </option>
                                                                            <option value="myerDistance">Myer Distance </option>
                                                                            <option value="myerInterval">Myer Interval </option>
                                                                            <option value="name">Name </option>
                                                                            <option value="noc">Noc </option>
                                                                            <option value="noc-anon">Noc-anon </option>
                                                                            <option value="noca">Noca </option>
                                                                            <option value="nocc">Nocc </option>
                                                                            <option value="noi">Noi </option>
                                                                            <option value="nom">Nom </option>
                                                                            <option value="operators">Operators </option>
                                                                            <option value="rdc">Rdc </option>
                                                                            <option value="rsc">Rsc </option>
                                                                            <option value="rsysc">Rsysc </option>
                                                                            <option value="sc">Sc </option>
                                                                            <option value="sysc">Sysc </option>
                                                                            <option value="time">Time </option>
                                                                            <option value="vocabulary">Vocabulary </option>
                                                                            <option value="volume">Volume </option>
                                                                    </select>
                            </td>
                            <td>
                                <select id="rAxis" class="form-control" onchange="rAxis = this.value; updateCustomChart();">
                                                                            <option value="afferentCoupling">Afferent Coupling </option>
                                                                            <option value="bugs">Bugs </option>
                                                                            <option value="commentWeight">Comment Weight </option>
                                                                            <option value="cyclomaticComplexity">Cyclomatic Complexity </option>
                                                                            <option value="dc">Dc </option>
                                                                            <option value="difficulty">Difficulty </option>
                                                                            <option value="efferentCoupling">Efferent Coupling </option>
                                                                            <option value="effort">Effort </option>
                                                                            <option value="filename">Filename </option>
                                                                            <option value="instability">Instability </option>
                                                                            <option value="intelligentContent">Intelligent Content </option>
                                                                            <option value="lcom">Lcom </option>
                                                                            <option value="length">Length </option>
                                                                            <option value="loc">Loc </option>
                                                                            <option value="logicalLoc">Logical Loc </option>
                                                                            <option value="maintainabilityIndex">Maintainability Index </option>
                                                                            <option value="maintainabilityIndexWithoutComment">Maintainability Index Without Comment </option>
                                                                            <option value="myerDistance">Myer Distance </option>
                                                                            <option value="myerInterval">Myer Interval </option>
                                                                            <option value="name">Name </option>
                                                                            <option value="noc">Noc </option>
                                                                            <option value="noc-anon">Noc-anon </option>
                                                                            <option value="noca">Noca </option>
                                                                            <option value="nocc">Nocc </option>
                                                                            <option value="noi">Noi </option>
                                                                            <option value="nom">Nom </option>
                                                                            <option value="operators">Operators </option>
                                                                            <option value="rdc">Rdc </option>
                                                                            <option value="rsc">Rsc </option>
                                                                            <option value="rsysc">Rsysc </option>
                                                                            <option value="sc">Sc </option>
                                                                            <option value="sysc">Sysc </option>
                                                                            <option value="time">Time </option>
                                                                            <option value="vocabulary">Vocabulary </option>
                                                                            <option value="volume">Volume </option>
                                                                    </select>
                            </td>
                        </tr>
                    </table>
                </div>

            </div>
            <div class="col-3">
                <h3>Abstractness / Instability</h3>
                <small><a id="btn-save-abstractness" download="phpmetric-abstractness.svg">Download (as SVG)</a> | <a onclick="return zoom('svg-abstractness', updateAbstractnessChart);">zoom</a></small>
                <div id="svg-abstractness"><svg /></div>
            </div>
        </div>
        <!-- end: General details -->

        <!-- Score -->
        <div class="tab" id="score">
            <div class="col-sm-12">
                <h3>Score</h3>

                <div class="col-6">
                    <div id="chart-score"></div>
                </div>

                <div class="col-6">
                    <p>
                        This score is not absolute. This chart is a comparison of your project relative to a representative average of recent PHP projects.
                    </p>
                    <p>
                        Each score is calculated from <acronym title="Cyclomatic complexity, Halstead metrics, Maintainability Index, Comment weight, Difficulty, Logical lines of code...">various criterias</acronym> from 203 files in your projects.
                        Your score is a note between 0 (poor) and 100 (excellent).
                    </p>
                    <table class="table">
                        <thead>
                            <tr>
                                <th>Factor</th>
                                <th>Score</th>
                            </tr>
                        </thead>
                        <tbody>
                                                    <tr>
                                <td>Maintainability</td>
                                <td>100 / 100</td>
                            </tr>
                                                    <tr>
                                <td>Accessibility for new developers</td>
                                <td>91.19 / 100</td>
                            </tr>
                                                    <tr>
                                <td>Simplicity of algorithms</td>
                                <td>55 / 100</td>
                            </tr>
                                                    <tr>
                                <td>Volume</td>
                                <td>100 / 100</td>
                            </tr>
                                                    <tr>
                                <td>Reducing bug&#039;s probability</td>
                                <td>95.08 / 100</td>
                            </tr>
                                                </tbody>
                    </table>
                    <p>
                        <strong>This score does not replace the judgement of a human.</strong>
                    </p>
                </div>
            </div>
        </div>
        <!-- end: Score -->

        <!-- Relations -->
        <div class="tab" id="relations">
            <h3>Relations</h3>
            <div class="col-sm-12">
                <div class="well">
                    <p>
                        Class uses another when it calls, constructs, types hint, extends or implements it.
                    </p>
                    <ul>
                        <li><span class="badge relation-source">Used by</span> : this class is used by hovered element.</li>
                        <li><span class="badge relation-target">Uses</span> : this class uses hovered element.</li>
                    </ul>

                </div>
                <small><a id="btn-save-relations" download="phpmetric-relations.svg">Download (as SVG)</a></small>
                <div class="svg-main" id="svg-relations"><svg /></div>

            </div>
        </div>
        <!-- end: Relations -->


        <!-- Tabular datas -->
        <div class="tab" id="explore">
            <div class="col-sm-12">
                <h3>Explore</h3>
                <p>
                    <select id="select-display-tabular">
                        <option value="all" selected="selected">Display metrics</option>
                        <option value="essential">Essential</option>
                        <option value="all">All</option>
                    </select>
                </p>
                <div id="chart-tableview"></div>
            </div>
        </div>
        <!-- end: Tabular datas -->


        <!-- Volume -->
        <div class="tab" id="repartition">
            <h3>Repartition</h3>
            <div class="col-3">
                <table class="table table-striped">
                    <tbody>
                        <tr>
                            <td>Files</td>
                            <td>203</td>
                        </tr>
                        <tr>
                            <td>Lines of code</td>
                            <td>
                                9195
                                                                    <span class="more">(63 by class,
                                        35 by method)</span>
                                                            </td>
                        </tr>
                        <tr>
                            <td>Logical lines of code</td>
                            <td>
                                1634
                                                                    <span class="more">(11 by class,
                                        6 by method)</span>
                                                            </td>
                        </tr>
                        <tr>
                            <td>Classes</td>
                            <td>145
                                                                    <div>
                                        <span class="more">
                                            47 interfaces (32 %)
                                        </span>
                                    </div>
                                    <div>
                                        <span class="more">
                                            97 concrete classes (67 %)
                                        </span>
                                    </div>
                                    <div>
                                        <span class="more">
                                            47 abstract classes (32 %)
                                        </span>
                                    </div>
                                    <div>
                                        <span class="more">
                                            1 anonymous classes (1 %)
                                        </span>
                                    </div>
                                                            </td>
                        </tr>

                        <tr>
                            <td>Methods</td>
                            <td>
                                261
                                                                    <span class="more">(1.8 by class )</span>
                                                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <div class="col-3">
                <table class="table table-striped">
                    <tbody>
                    <tr>
                        <td>Relative system complexity</td>
                        <td>1.42</td>
                    </tr>
                    <tr>
                        <td>Relative data complexity</td>
                        <td>0.49</td>
                    </tr>
                    <tr>
                        <td>Relative structure complexity</td>
                        <td>0.93</td>
                    </tr>
                    </tbody>
                </table>
            </div>
            <div class="col-3">
                <table class="table table-striped">
                    <tbody>
                    <tr>
                        <td>Lack of cohesion of methods</td>
                        <td>0.33</td>
                    </tr>
                    <tr>
                        <td>Efferent Coupling</td>
                        <td>0.56</td>
                    </tr>
                    <tr>
                        <td>Afferent Coupling</td>
                        <td>0.16</td>
                    </tr>
                    <tr>
                        <td>Abstractness</td>
                        <td>0.33</td>
                    </tr>
                    </tbody>
                </table>
            </div>
        </div>
        <!-- end: Volume -->

        <!-- Help -->
        <div class="tab" id="help">
            <div class="col-sm-12">
                <h3>Help</h3>
                <ul>
                    <li>You're using PhpMetrics v1.10.0</li>
                    <li>Online help is available on the <a href="http://www.phpmetrics.org/documentation/index.html">PhpMetrics website</a>.</li>
                    <li>Sources of PhpMetrics are available on <a href="https://github.com/PhpMetrics/PhpMetrics">Github</a>.</li>
                    <li>PhpMetrics is created by <a href="http://lepine.pro">Jean-François Lépine</a>.</li>
                </ul>
            </div>
        </div>
        <!-- end: Help -->

        <!-- Extensions blocks -->
        
        <!-- end: Extensions -->

    </div>


    <div class="row footer">
        <div class="container">
            Powered by <a href="http://www.phpmetrics.org">PhpMetrics</a> v1.10.0 - Copyright Jean-François Lépine
        </div>
    </div>

</div>
{% include_relative phpmetrics.js.html %}