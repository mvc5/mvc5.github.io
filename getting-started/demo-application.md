## Demo Application
<p>Install the demo <a href="https://github.com/mvc5/mvc5-application">mvc5-application</a>.</p>

<pre><code class="language-php">composer.phar create-project mvc5/mvc5-application mvc5playground</code></pre>
<p style="margin-top:20px;">The document root is the <a href="https://github.com/mvc5/mvc5-application/tree/master/public"><code>public</code></a> directory and in a development environment, error reporting should be enabled in the php.ini file.</p>
<pre><code class="language-php">php -S localhost:8000 -t mvc5playground/public</code></pre>
<div class="thumbnail" style="border:none;">
    <img style="margin-left:0;" src="/images/mvc5playground.png" width="480" height="320" title="Mvc5 Demo Application">
</div>

## Docker Project
<p class="pt-3">Alternatively, install the <a href="https://github.com/devosc/docker" title="PHP Docker Project">PHP Docker Project</a> and run <code>docker-up -a</code>. The demo <a href="https://github.com/mvc5/mvc5-application">mvc5-application</a> project contains a <a href="https://github.com/mvc5/mvc5-application/blob/master/docker-compose.yml">Compose</a> file that configures the version of PHP to use and the development tools to install. <a href="https://traefik.io/" title="Traefik">Traefik</a> is used as a reverse proxy so that multiple projects can run at the same time. Shared services, such as Adminer, MailHog, MariaDB and PostgreSQL, can also be started at the same time as the project container. Other commands are available to provide a convenient way to work with a PHP project and Docker, e.g. <code>docker-app</code>, <code>docker-phpunit</code> and <code>docker-xdebug</code>.</p>
