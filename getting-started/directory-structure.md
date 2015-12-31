## Directory Structure
<div class="media media-left-sm-collapse">
  <div class="media-left">
    <a href="#">
      <img src="/images/framework-dir.png" width="94" height="250" alt="Directory Structure">
    </a>
  </div>
  <div class="media-body">
    <h4 id="mvc5-directory">Mvc5</h4>
    <p>The <a href="https://github.com/mvc5/mvc5/tree/master/src">src</a> directory contains the <a href="http://www.php-fig.org/psr/psr-4/">PSR-4</a> composer autoloaded class files.</p>
    <p>The <a href="https://github.com/mvc5/mvc5/tree/master/config">config</a> directory contains the php array configuration files that can be included and overridden by the application.</p>
    <p>The <a href="https://github.com/mvc5/mvc5/tree/master/view">view</a> directory contains template files for exceptions, page not found, layout and the home page. They do not need to be used and are <a href="https://github.com/mvc5/mvc5/blob/master/config/template.php">configured</a> by the application.</p>    
  </div>
</div>
<div class="media media-left-sm-collapse">
  <div class="media-left">
    <a href="#">
      <img src="/images/application-dir.png" width="97" height="250" alt="Directory Structure">
    </a>
  </div>
  <div class="media-body">
  <h4 id="application-directory">Application</h4>
    <p>The <a href="https://github.com/mvc5/mvc5-application/tree/master/config">config</a> directory contains the php array configuration files that includes and overrides the default framework configuration. These configuration files can also be <a href="/overview/#environment-aware">environment aware</a>.</p>
    <p>The <a href="https://github.com/mvc5/mvc5-application/tree/master/public">public</a> directory is the web site document root and contains the main <a href="https://github.com/mvc5/mvc5-application/blob/master/public/index.php">index.php</a> script.</p>
    <p>The <a href="https://github.com/mvc5/mvc5-application/tree/master/src">src</a> directory contains the application class files and uses the <a href="http://www.php-fig.org/psr/psr-4/">PSR-4</a> autoloading standard.</p>
    <p>The vendor directory is used for external libraries including the <a href="#mvc5-directory">mvc5</a> directory.</p>
    <p>The <a href="https://github.com/mvc5/mvc5-application/tree/master/view">view</a> directory contains the view model templates.</p>
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
