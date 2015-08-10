---
layout: sidebar
title: Getting Started
tagline: Download and explore the demo application
scrollspy: sidebar
---
## Source Code
<p>The Mvc5 Framework has no external dependencies and can be downloaded separately or <a href="#composer-install">installed via composer</a>. However, the best way to explore the Mvc5 Framework is to <a href="#install-demo-application">install the demo application</a>.</p>
<p style="margin-top:20px;">
    <a class="btn btn-default btn-lg" href="https://github.com/mvc5/framework/releases/latest"><span class="glyphicon glyphicon-hand-right"></span> Latest Release</a>
</p>

## Composer Install
<pre><code>$ composer.phar require mvc5/framework</code></pre>
<div class="clearfix"></div>

## Install Demo Application
<ol>
    <li>
        <p>Download the demo application.</p>
        <p style="margin-top:20px;">
            <a class="btn btn-default btn-lg" href="https://github.com/mvc5/application/archive/master.zip"><span class="glyphicon glyphicon-download"></span> Demo Application</a>
        </p>
    </li>
    <li>
        <p>Extract archive and rename directory to <b>mvc5playground</b>.</p>
        <pre><code class="language-php">$ unzip ./application-master.zip<br>$ mv application-master mvc5playground</code></pre>
    </li>
    <li>
        <p>Install composer.</p>
        <pre><code class="language-php">$ cd mvc5playground<br>$ composer.phar install --no-dev</code></pre>
    </li>
    <li>
        <p>Open the demo application in a web browser. The document root should point to the <code>public</code> directory.</p>
        <div class="panel panel-default">
            <div class="panel-body">
                <img src="/images/mvc5playground.png" width="400" height="338" title="Mvc5 Demo Application">
            </div>
        </div>
    </li>
</ol>
