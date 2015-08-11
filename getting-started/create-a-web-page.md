## Create A Web Page
<p class="text-info">Copy and paste the example code into each new file.</p>
<p style="margin-top:25px;"><a id="view-model"></a><b>1.</b> Create a new view model file in the <a href="https://github.com/mvc5/application/tree/master/src/Home">src/Home</a> directory named <a href="https://github.com/mvc5/application/tree/master/src/Home/Model.php">Model.php</a>.</p>
<pre style="line-height:1"><code><?php

namespace Home;

use Mvc5\View\Model\Base;
use Mvc5\View\Model\Plugin;
use Mvc5\View\Model\ViewModel;
use Mvc5\View\ViewPlugin;

class Model
    implements Plugin, ViewModel
{
    /**
     *
     */
    use Base;
    use ViewPlugin;

    /**
     *
     */
    const TEMPLATE_NAME = __DIR__ . '/../../view/home/index.phtml';
}</code></pre>
<p>The constant <code>TEMPLATE_NAME</code> can be used as the name or file path of the view model's associated template and it is assigned to the template variable within the <a href="https://github.com/mvc5/framework/blob/master/src/View/Model/Base.php#L21">constructor</a> when no <a href="https://github.com/mvc5/framework/blob/master/src/View/Model/Base.php#L21">constructor</a> argument is given. The template name or path can also be set with the <a href="https://github.com/mvc5/framework/blob/master/src/View/Model/Base.php#L66">template</a> method. Read more about <a href="/overview/#view-model-plugins">view model plugins</a>.</p>
<p style="margin-top:25px;"><a id="controller"></a><b>2.</b> Create a new controller file in the <a href="https://github.com/mvc5/application/tree/master/src/Home">src/Home</a> directory named <a href="https://github.com/mvc5/application/blob/master/src/Home/Controller.php">Controller.php</a>.</p>
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
        $this->model->vars(['date' => date('m/d/Y')]);
        
        return $this->model;
    }
}</code></pre>
<p>When the system instantiates the controller and no model is passed to the constructor, the system will determine that it is a required parameter and will use <a href="/overview/#constructor-autowiring">constructor autowiring</a> to instantiate a new instance of the view model and pass it to the controller.</p>
<p>In the above example, when the controller is invoked, it assigns the current date to the view model and returns it. The returned view model will then be the response model and is rendered prior to sending the response. Read more about the <a href="/overview/#model-view-controller">model view controller</a>.</p>
<p style="margin-top:25px;"><a id="view-template"></a><b>3.</b> Create a new template file in the <a href="https://github.com/mvc5/application/tree/master/view/home">view/home</a> directory named <a href="https://github.com/mvc5/application/blob/master/view/home/index.phtml">index.phtml</a></p>
<pre style="line-height:1"><code><?php
                                 
  echo '&lt;h1&gt;' . $date . '&lt;/h1&gt;';

</code></pre>
<p>The variables assigned to a view model are available within the template as php variables, e.g <code>$date</code> or via the current object which is the view model, e.g. <code>$this->date</code>. Read more about <a href="/overview/#rendering-view-models">rendering view models</a>.</p>
<p style="margin-top:25px;"><a id="route"></a><b>4.</b> Create a new route configuration file in the application <a href="https://github.com/mvc5/application/tree/master/config">config</a> directory named <a href="https://github.com/mvc5/application/blob/master/config/route.php">route.php</a></p>
<pre style="line-height:1"><code><?php

return [
    'name'       => 'home',
    'route'      => '/',
    'controller' => 'Home\Controller'
];</code></pre>
<p>The above example is for the site home page, open the demo application in a web browser, it should show today's date. Read more about <a href="/overview/#routes">routes</a>.</p>
<div class="thumbnail" style="border:none;">
    <img style="margin-left:0;" src="/images/demo-home.png" width="400" height="113" title="Demo Home Page">
</div>
