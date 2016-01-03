## Create A Web Page
<h6 class="text-info">Copy and paste the example code into each new file.</h6>
<p style="margin-top:25px;"><a id="view-model"></a><b>1.</b> Create a new view model file in the <a href="https://github.com/mvc5/mvc5-application/tree/master/src/Home">src/Home</a> directory named <a href="https://github.com/mvc5/mvc5-application/tree/master/src/Home/Model.php">Model.php</a>.</p>
<pre style="line-height:1"><code><?php

namespace Home;

use Mvc5\Model\ViewModel;
use Mvc5\Model\Plugin;

class Model
    implements ViewModel
{
    /**
     *
     */
    use Plugin;

    /**
     *
     */
    const TEMPLATE_NAME = __DIR__ . '/../../view/home/index.phtml';
}</code></pre>
<p>The constant <code>TEMPLATE_NAME</code> can be used as the <a href="https://github.com/mvc5/mvc5-application/blob/master/config/template.php#L11">name</a> or file path of the view model's associated template and is assigned to the template variable within the <a href="https://github.com/mvc5/mvc5/blob/master/src/Model/Template/Model.php#L26">constructor</a> when no <a href="https://github.com/mvc5/mvc5/blob/master/src/Model/Template/Model.php#L22">constructor</a> arguments are given. The template name or path can also be set with the <a href="https://github.com/mvc5/mvc5/blob/master/src/Model/Template/Model.php#L35">template</a> method. Read more about <a href="/overview/#view-models">view models</a>.</p>
<p style="margin-top:25px;"><a id="controller"></a><b>2.</b> Create a new controller file in the <a href="https://github.com/mvc5/mvc5-application/tree/master/src/Home">src/Home</a> directory named <a href="https://github.com/mvc5/mvc5-application/blob/master/src/Home/Controller.php">Controller.php</a>.</p>
<pre style="line-height:1"><code><?php

namespace Home;
    
class Controller
{
    /**
     * @var Model
     */
    protected $model;
    
    /**
     * @param Model $model
     */
    public function __construct(Model $model)
    {
        $this->model = $model;
    }

    /**
     * @return Model
     */
    public function __invoke()
    {
        $this->model->vars(['msg' => 'Hello World!']);
        
        return $this->model;
    }
}</code></pre>
<p>When the system instantiates the controller and no model is passed to its constructor, the system will <a href="https://github.com/mvc5/mvc5/blob/master/src/Resolver/Build.php#L151">determine</a> that it is a required parameter and <a href="/overview/#autowiring">autowire</a> the controller with a new instance of the <a href="https://github.com/mvc5/mvc5-application/blob/master/src/Home/Model.php">view model</a>.</p>
<p>In the above, when the controller is invoked, it assigns a variable to the <a href="https://github.com/mvc5/mvc5-application/blob/master/src/Home/Model.php">view model</a> and returns it. The returned <a href="https://github.com/mvc5/mvc5-application/blob/master/src/Home/Model.php">view model</a> will become the <a href="https://github.com/mvc5/mvc5/blob/master/src/Mvc/Mvc.php#L53">response model</a>, which is <a href="https://github.com/mvc5/mvc5/blob/master/src/View/Template/Renderer.php#L36">rendered</a> prior to <a href="https://github.com/mvc5/mvc5/blob/master/src/Response/Send.php#L13">sending the response</a>.</p>
<p style="margin-top:25px;"><a id="view-template"></a><b>3.</b> Create a new template file in the <a href="https://github.com/mvc5/mvc5-application/tree/master/view/home">view/home</a> directory named <a href="https://github.com/mvc5/mvc5-application/blob/master/view/home/index.phtml">index.phtml</a></p>
<pre style="line-height:1"><code><?php
                                 
  echo '&lt;h1&gt;' . $msg . '&lt;/h1&gt;';

</code></pre>
<p>The variables assigned to a <a href="https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php">view model</a> are available within the template as php variables, e.g <code>$msg</code> or via the current object, which is the <a href="https://github.com/mvc5/mvc5/blob/master/src/Model/ViewModel.php">view model</a>, e.g. <code>$this->msg</code>. Read more about <a href="/overview/#rendering-view-models">rendering view models</a>.</p>
<p style="margin-top:25px;"><a id="route"></a><b>4.</b> Create a new route configuration file in the application <a href="https://github.com/mvc5/mvc5-application/tree/master/config">config</a> directory named <a href="https://github.com/mvc5/mvc5-application/blob/master/config/route.php">route.php</a></p>
<pre style="line-height:1"><code><?php

return [
    'name'       => 'home',
    'route'      => '/',
    'controller' => 'Home\Controller'
];</code></pre>
<p>Open the demo application in a web browser. It will display the hello world message on the home page. Read more about <a href="/overview/#routes">routes</a>.</p>
<div class="thumbnail" style="border:none;">
    <img style="margin-left:0;" src="/images/demo-homepage.png" width="435" height="128" title="Demo Home Page">
</div>
