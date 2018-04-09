## Create A Web Page
<h6 class="text-muted">Copy and paste the example code into each new file.</h6>
<p style="margin-top:25px;"><a id="view-model"></a><b>1.</b> Create a new view model file in the <a href="https://github.com/mvc5/mvc5-application/tree/master/src/Home">src/Home</a> directory named <a href="https://github.com/mvc5/mvc5-application/tree/master/src/Home/ViewModel.php">ViewModel.php</a>.</p>
<pre style="line-height:1"><code><?php

namespace Home;

class ViewModel
    extends \Mvc5\ViewModel
{
    /**
     *
     */
    const TEMPLATE = 'home/index';
}</code></pre>
<p>The constant <code>TEMPLATE_NAME</code> can be used as the <a href="https://github.com/mvc5/mvc5/blob/master/src/Template/TemplateModel.php#L14">name</a> or file path of the view model's associated template and is assigned to the template variable within the <a href="https://github.com/mvc5/mvc5/blob/master/src/View/Config/ViewModel.php#L27">constructor</a> when no <a href="https://github.com/mvc5/mvc5/blob/master/src/View/Config/ViewModel.php#L27">constructor</a> arguments are given. Read more about <a href="/overview/#view-models">view models</a>.</p>
<p style="margin-top:25px;"><a id="controller"></a><b>2.</b> Create a new controller file in the <a href="https://github.com/mvc5/mvc5-application/tree/master/src/Home">src/Home</a> directory named <a href="https://github.com/mvc5/mvc5-application/blob/master/src/Home/Controller.php">Controller.php</a>.</p>
<pre style="line-height:1"><code><?php
                                 
namespace Home;

use Mvc5\Http\Request;
use Mvc5\View;

class Controller
{
    /**
     *
     */
    use View\Model;
    
    /**
     *
     */
    const VIEW_MODEL = ViewModel::class;
    
    /**
     * @param Request $request
     * @return ViewModel
     */
    function __invoke(Request $request) : ViewModel
    {
        return $this->model(['msg' => 'Hello World!']);
    }
}</code></pre>
<p>When the above controller is invoked, it <a href="https://github.com/mvc5/mvc5/blob/master/src/Plugins/View.php#L37">returns a copy</a> of the <a href="https://github.com/mvc5/mvc5-application/blob/master/src/Home/ViewModel.php">view model</a> with the <code>msg</code> variable assigned to it. The <a href="https://github.com/mvc5/mvc5-application/blob/master/src/Home/ViewModel.php">view model</a> will become the <a href="https://github.com/mvc5/mvc5/blob/master/src/Response/Dispatch.php#L78">response model</a> and will be <a href="https://github.com/mvc5/mvc5/blob/master/src/View/Engine/PhpEngine.php#L18">rendered</a> prior to <a href="https://github.com/mvc5/mvc5/blob/master/src/Response/Service/Send.php#L66">sending the response</a>.</p>
<p style="margin-top:25px;"><a id="view-template"></a><b>3.</b> Create a new template file in the <a href="https://github.com/mvc5/mvc5-application/tree/master/view/home">view/home</a> directory named <a href="https://github.com/mvc5/mvc5-application/blob/master/view/home/index.phtml">index.phtml</a></p>
<pre style="line-height:1"><code><?php
                                 
  echo '&lt;h1&gt;' . $msg . '&lt;/h1&gt;';

</code></pre>
<p>The variables assigned to a <a href="https://github.com/mvc5/mvc5/blob/master/src/View/ViewModel.php">view model</a> are available within the template as PHP variables, e.g <code>$msg</code> or via the current <a href="https://github.com/mvc5/mvc5-application/tree/master/src/Home/ViewModel.php">view model</a> object, e.g. <code>$this->msg</code>. Read more about <a href="/overview/#rendering-view-models">rendering view models</a>.</p>
