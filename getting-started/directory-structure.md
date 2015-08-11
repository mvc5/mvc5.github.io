## Directory Structure
<div class="media media-left-sm-collapse">
  <div class="media-left">
    <a href="#">
      <img src="/images/framework-dir.png" width="134" height="250" alt="Directory Structure">
    </a>
  </div>
  <div class="media-body">
    <h4 id="framework-directory">Framework Directory</h4>
    <p>The <a href="https://github.com/mvc5/framework/tree/master/src">src</a> directory contains the PSR-4 composer autoloaded class files.</p>
    <p>The <a href="https://github.com/mvc5/framework/tree/master/config">config</a> directory contains the php array configuration files that can be included and overridden by the application.</p>
    <p>The <a href="https://github.com/mvc5/framework/tree/master/view">view</a> directory contains template files for exceptions, page not found, layout and the home page. They do not need to be used and are configured by the application.</p>    
  </div>
</div>
<div class="media media-left-sm-collapse">
  <div class="media-left">
    <a href="#">
      <img src="/images/application-dir.png" width="134" height="250" alt="Directory Structure">
    </a>
  </div>
  <div class="media-body">
  <h4 id="application-directory">Application Directory</h4>
    <p>The <a href="https://github.com/mvc5/application/tree/master/config">config</a> directory contains the php array configuration files that includes and overrides the default framework configuration. These configuration files can also be made to be environment aware.</p>
    <p>The <a href="https://github.com/mvc5/application/tree/master/public">public</a> directory is the web site document root and contains the main index.php script.</p>
    <p>The <a href="https://github.com/mvc5/application/tree/master/src">src</a> directory contains the application class files and the demo uses the PSR-0 autoloading standard.</p>
    <p>The vendor directory is used for external libraries and contains the framework directory.</p>
    <p>The <a href="https://github.com/mvc5/application/tree/master/view">view</a> directory contains the view model templates.</p>
  </div>
</div>
<style type="text/css">
@media (max-width: 360px) {
  .media-left-sm-collapse .media-left {
    display:none;
  }
  
  .media-left-sm-collapse .media-body {
    width:100%;
  }
}
</style>
