<html>

<body>

<div>
    <section class="ouvJEz"><h1 class="_1RuRku">Odoo Javascript 参考指南</h1><div class="rEsl9f"><div class="_2mYfmT"><a class="_1OhGeD" href="/u/ecd4e741f182" target="_blank" rel="noopener noreferrer"><img class="_13D2Eh" src="https://cdn2.jianshu.io/assets/default_avatar/4-3397163ecdb3855a0a4139c34a695885.jpg" alt=""></a><div style="margin-left: 8px;"><div class="_3U4Smb"><span class="FxYr8x"><a class="_1OhGeD" href="/u/ecd4e741f182" target="_blank" rel="noopener noreferrer">dooms21day</a></span><button data-locale="zh-CN" type="button" class="_3kba3h _1OyPqC _3Mi9q9 _34692-"><span>关注</span></button></div><div class="s-dsoj"><span class="_3tCVn5"><i aria-label="ic-diamond" class="anticon"><svg width="1em" height="1em" fill="currentColor" aria-hidden="true" focusable="false" class="" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><use xlink:href="#ic-diamond"></use></svg></i><span>0.312</span></span><time datetime="2019-01-09T06:36:27.000Z">2019.01.09 14:36:27</time><span>字数 10,824</span><span>阅读 4,897</span></div></div></div></div><article class="_2rhmJa"><p>本文介绍了odoo javascript框架。从代码行的角度来看，这个框架不是一个大的应用程序，但它是非常通用的，因为它基本上是一个将声明性接口描述转换为活动应用程序的机器，能够与数据库中的每个模型和记录交互。甚至可以使用Web客户端修改Web客户端的接口。</p>
<blockquote>
<p>这里有一个有用的html版本的文档：<a href="https://www.odoo.com/documentation/12.0/reference/javascript_api.html" target="_blank" rel="nofollow">Javascript API</a></p>
</blockquote>
<h2>概览</h2>
<p>这个Javascript框架主要设计用于三个地方使用：</p>
<ul>
<li>web客户端：这是一个私有的web应用，可以在其中查看和编辑业务数据。这是一个单页应用程序（永远不会重新加载该页，只在需要时从服务器提取新数据）。</li>
<li>网站：这是Odoo的公共部分。它允许身份不明的用户作为客户端浏览某些内容、购物或执行许多操作。这是一个经典的网站：各种各样的带有控制器的路由和共同协作的Javascript代码。</li>
<li>POS：这是销售点的接口。它是一个特定的但也应用程序。</li>
</ul>
<h2>Web客户端</h2>
<h4>单页应用</h4>
<p>简而言之，webclient实例是整个用户界面的根组件。它的职责是协调所有的子组件，并提供服务，如RPC、本地存储等等。</p>
<p>在运行时，Web客户端是单页应用程序。每次用户执行操作时，它不需要从服务器请求整页。相反，它只请求它所需要的，然后替换/更新视图。此外，它还管理URL：它与Web客户机状态保持同步。</p>
<p>这意味着，当用户在处理odoo时，Web客户机类（和动作管理器）实际上创建并销毁了许多子组件。状态是高度动态的，每个小部件都可以随时销毁。</p>
<h2>Web客户端JS代码概览</h2>
<p>这里，我们在web/static/src/js插件中快速概述了web客户机代码。注意，这是故意不详尽的，我们只涉及最重要的文件/文件夹。</p>
<ul>
<li>boot.js : 这是定义模块系统的文件,它需要首先加载。</li>
<li>core/ : 这是较低级别的构建基块的集合。值得注意的是，它包含类系统、小部件系统、并发实用程序和许多其他类/函数。</li>
<li>chorm/ :在这个文件夹中，我们有大多数大的小部件，它们构成了大部分用户界面。</li>
<li>chrome/abstract_web_client.js and chrome/web_client.js : 这些文件一起定义了WebClient小部件(widget)，它是Web客户机的根小部件（wideget）。</li>
<li>chrome/action_manager.js :  这是将动作（action）转换为小部件(widget)（例如看板或表单视图）的代码。</li>
<li>chrome/search_X.js : 所有这些文件定义了搜索视图（它不是Web客户机视图中的视图，仅从服务器视图）</li>
<li>fields : 这里定义了所有主要字段视图小部件（widget）</li>
<li>views : 这是视图所在的位置</li>
</ul>
<h2>资源管理</h2>
<p>在Odoo中管理资源并不像在其他应用程序中那样简单。其中一个原因是，在其中一些情况中我们有各种各样的状态，但不是所有的资源都是必需的。例如，Web客户端、销售点、网站甚至移动应用程序的需求是不同的。此外，有些资源可能很大，但很少需要。在这种情况下，我们有时希望它们被懒惰地加载。</p>
<p>主要思想是我们用XML定义一组包。捆绑包在这里定义为一组文件（javascript、css、scss）。在odoo中，最重要的包在addons/web/views/webclient_templates.xml文件中定义。看起来是这样的：</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-xml"><code class="  language-xml"><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>template</span> <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>web.assets_common<span class="token punctuation">"</span></span> <span class="token attr-name">name</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>Common Assets (used in backend interface and website)<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>link</span> <span class="token attr-name">rel</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>stylesheet<span class="token punctuation">"</span></span> <span class="token attr-name">type</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>text/css<span class="token punctuation">"</span></span> <span class="token attr-name">href</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>/web/static/lib/jquery.ui/jquery-ui.css<span class="token punctuation">"</span></span><span class="token punctuation">/&gt;</span></span>
    ...
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>script</span> <span class="token attr-name">type</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>text/javascript<span class="token punctuation">"</span></span> <span class="token attr-name">src</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>/web/static/src/js/boot.js<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>script</span><span class="token punctuation">&gt;</span></span>
    ...
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>template</span><span class="token punctuation">&gt;</span></span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<p>然后，可以使用t-call-assets指令将捆绑包中的文件插入到模板中：</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-xml"><code class="  language-xml"><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>t</span> <span class="token attr-name">t-call-assets</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>web.assets_common<span class="token punctuation">"</span></span> <span class="token attr-name">t-js</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>false<span class="token punctuation">"</span></span><span class="token punctuation">/&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>t</span> <span class="token attr-name">t-call-assets</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>web.assets_common<span class="token punctuation">"</span></span> <span class="token attr-name">t-css</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>false<span class="token punctuation">"</span></span><span class="token punctuation">/&gt;</span></span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span></span></code></pre></div>
<p>下面是当服务器使用以下指令呈现模板时发生的情况：</p>
<ul>
<li>包中描述的所有SCSS文件都编译为CSS文件。名为file.scss的文件将编译在名为file.scss.css的文件中。</li>
<li>
<strong>如果我们在debug=assets模式</strong>：<br>
   *  t-js属性设置为false的t-call-assets指令将替换为指向css文件的样式表标记列表。<br>
   * t-css属性设置为false的t-call-assets指令将替换为指向JS文件的脚本标记列表。</li>
<li>
<strong>如果我们不在debug=assets模式</strong><br>
   * CSS文件将被连接并缩小，然后拆分为不超过4096个规则的文件（以绕过IE9的旧限制）。然后，我们根据需要生成尽可能多的样式表标签<br>
   * JS文件被连接并缩小，然后生成一个脚本标记。</li>
</ul>
<p>请注意，资源文件是缓存的，因此从理论上讲，浏览器应该只加载它们一次。</p>
<h2>主包</h2>
<p>当odoo服务器启动时，它检查包中每个文件的时间戳，如果需要，它将创建/重新创建相应的包。</p>
<p>以下是大多数开发人员需要知道的一些重要包：</p>
<ul>
<li>web.assets_common : 此包包含Web客户端、网站以及销售点（POS）所共有的大多数资源。这应该包含用于Odoo框架的较低级别的构建块。注意，它包含boot.js文件，它定义了odoo模块系统。</li>
<li>web.assets_backend :这个包包含特定于Web客户端的代码（特别是Web客户端/动作管理器/视图）</li>
<li>web.assets_frontend :这个包是关于所有特定于公共网站的：电子商务、论坛、博客、事件管理…</li>
</ul>
<h2>在一个资源包里添加文件</h2>
<p>将位于addons/web中的文件添加到bundle的正确方法很简单：只需将脚本或样式表标记添加到文件webclient_templates.xml中的bundle即可。但是当我们使用不同的插件（addon）时，我们需要从该插件添加一个文件。在这种情况下，应分三步进行：</p>
<ol>
<li>添加一个 assets.xml 文件到views/文件夹</li>
<li>添加字符'views/assets.xml' 到manifest文件的键'data'的值里</li>
<li>创建所需包的继承视图，并使用xpath表达式添加文件。例如：</li>
</ol>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-xml"><code class="  language-xml"><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>template</span> <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>assets_backend<span class="token punctuation">"</span></span> <span class="token attr-name">name</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>helpdesk assets<span class="token punctuation">"</span></span> <span class="token attr-name">inherit_id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>web.assets_backend<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>xpath</span> <span class="token attr-name">expr</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>//script[last()]<span class="token punctuation">"</span></span> <span class="token attr-name">position</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>after<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>link</span> <span class="token attr-name">rel</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>stylesheet<span class="token punctuation">"</span></span> <span class="token attr-name">type</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>text/scss<span class="token punctuation">"</span></span> <span class="token attr-name">href</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>/helpdesk/static/src/scss/helpdesk.scss<span class="token punctuation">"</span></span><span class="token punctuation">/&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>script</span> <span class="token attr-name">type</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>text/javascript<span class="token punctuation">"</span></span> <span class="token attr-name">src</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>/helpdesk/static/src/js/helpdesk_dashboard.js<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>script</span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>xpath</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>template</span><span class="token punctuation">&gt;</span></span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<blockquote>
<p>请注意，当用户加载odoo web客户端时，包中的所有文件都会立即加载。这意味着每次通过网络传输文件（浏览器缓存处于活动状态时除外）。在某些情况下，最好使用Lazyload的一些资产。例如，如果一个小部件需要一个大的库，而这个小部件不是体验的核心部分，那么在实际创建小部件时，最好只加载库。widget类实际上已经为这个用例内置了支持。(查阅<a href="https://www.odoo.com/documentation/12.0/reference/javascript_reference.html#reference-javascript-reference-qweb" target="_blank" rel="nofollow">QWeb模板引擎</a>部分)</p>
</blockquote>
<h2>如果文件没有加载/更新应该怎么办</h2>
<p>文件可能无法正确加载有许多不同的原因。您可以尝试以下几点来解决此问题：</p>
<ul>
<li>一旦服务器启动，它就不知道资源文件是否已被修改。因此，您可以简单地重新启动服务器来重新生成资源。</li>
<li>检查控制台（在开发工具中，通常用F12打开），确保没有明显的错误</li>
<li>尝试在文件的开头添加console.log（在任何模块定义之前），这样您就可以查看文件是否已加载。</li>
<li>在用户界面中，在调试模式下（在此处插入链接到调试模式），有一个选项可以强制服务器更新其资源文件。</li>
<li>使用debug=assets模式。这实际上会绕过资源包（请注意，它实际上并不能解决问题，服务器仍然使用过时的包）</li>
<li>最后，对于开发人员来说，最方便的方法是使用--dev=all选项启动服务器。这将激活文件监视程序选项，必要时将自动使资源无效。请注意，如果操作系统是Windows，它就不能很好地工作。</li>
<li>记住刷新页面！</li>
<li>或者保存代码文件…</li>
</ul>
<blockquote>
<p>重新创建资源文件后，需要刷新页面，重新加载正确的文件（如果不起作用，文件可能被缓存了）。</p>
</blockquote>
<h2>Javascript模块系统</h2>
<p>一旦我们能够将我们的javascript文件加载到浏览器中，我们就需要确保以正确的顺序加载它们。为了实现这一点，odoo定义了一个小模块系统（位于addons/web/static/src/js/boot.js文件中，需要首先加载该文件）。</p>
<p>在AMD的启发下，odoo模块系统通过在全局odoo对象上定义函数define来工作。然后我们通过调用该函数来定义每个javascript模块。在odoo框架中，模块是一段将尽快执行的代码。它有一个名称，可能还有一些依赖项。当它的依赖项被加载时，模块也将被加载。模块的值就是定义模块的函数的返回值。</p>
<p>一个例子，看起来像这样：</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-php"><code class="  language-php"><span class="token comment">// in file a.js</span>
odoo<span class="token punctuation">.</span><span class="token function">define</span><span class="token punctuation">(</span><span class="token single-quoted-string string">'module.A'</span><span class="token punctuation">,</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token keyword">require</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token double-quoted-string string">"use strict"</span><span class="token punctuation">;</span>

    <span class="token keyword">var</span> <span class="token constant">A</span> <span class="token operator">=</span> <span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">;</span>
    
    <span class="token keyword">return</span> <span class="token constant">A</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token comment">// in file b.js</span>
odoo<span class="token punctuation">.</span><span class="token function">define</span><span class="token punctuation">(</span><span class="token single-quoted-string string">'module.B'</span><span class="token punctuation">,</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token keyword">require</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token double-quoted-string string">"use strict"</span><span class="token punctuation">;</span>

    <span class="token keyword">var</span> <span class="token constant">A</span> <span class="token operator">=</span> <span class="token keyword">require</span><span class="token punctuation">(</span><span class="token single-quoted-string string">'module.A'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    
    <span class="token keyword">var</span> <span class="token constant">B</span> <span class="token operator">=</span> <span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">;</span> <span class="token comment">// something that involves A</span>
    
    <span class="token keyword">return</span> <span class="token constant">B</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<p>定义模块的另一种方法是在第二个参数中明确地给出依赖项列表。</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-jsx"><code class="  language-jsx">odoo<span class="token punctuation">.</span><span class="token function">define</span><span class="token punctuation">(</span><span class="token string">'module.Something'</span><span class="token punctuation">,</span> <span class="token punctuation">[</span><span class="token string">'module.A'</span><span class="token punctuation">,</span> <span class="token string">'module.B'</span><span class="token punctuation">]</span><span class="token punctuation">,</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token parameter">require</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token string">"use strict"</span><span class="token punctuation">;</span>

    <span class="token keyword">var</span> <span class="token constant">A</span> <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'module.A'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token keyword">var</span> <span class="token constant">B</span> <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'module.B'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    
    <span class="token comment">// some code</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<p>如果某些依赖项丢失/未就绪，那么模块将不会被加载。几秒钟后控制台中将出现警告。</p>
<p>请注意，不支持循环依赖项。这是有道理的，但这意味着需要谨慎。</p>
<h2>定义一个模块</h2>
<p>odoo.define 方法给了三个参数：</p>
<ul>
<li>moduleName: javascript模块的名称。它应该是一个唯一的字符串。惯例是在odoo插件（addon）的名字后面加上一个具体的描述。例如，“web.widget”描述在web插件中定义的模块，该模块导出一个widget类（因为第一个字母大写）。<br>
如果名称不唯一，将引发异常并显示在控制台中</li>
<li>dependencies : 第二个参数是可选的。如果给定，它应该是一个字符串列表，每个字符串对应一个JavaScript模块。这描述了在执行模块之前需要加载的依赖项。如果这里没有明确地给出依赖项，那么模块系统将通过调用ToString从函数中提取它们，然后使用regexp查找所有Require语句。</li>
<li>最后一个参数是定义模块的函数。它的返回值是模块的值，可以传递给其他需要它的模块。注意，异步模块有一个小的异常，请参见下一节。<br>
如果发生错误，将在控制台中记录（在调试模式下）：</li>
<li>Missing dependencies: 这些模块不会出现在页面中。可能是javascript文件不在页面中或模块名称错误</li>
<li>Failed Modules : 一个Javascript错误被检测到</li>
<li>Rejected modules ：块返回拒绝的延迟。它（及其相关模块）未加载。</li>
<li>Rejected linked modules: 依赖被拒绝模块的模块</li>
<li>Non loaded modules : 模块依赖了一个缺失/失败的模块</li>
</ul>
<h2>异步模块</h2>
<p>模块可能需要在准备就绪之前执行一些工作。例如，它可以做一个RPC来加载一些数据。在这种情况下，模块只需返回一个deferred（promise）。在这种情况下，模块系统只需等待deferred完成，然后注册模块。</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-jsx"><code class="  language-jsx">odoo<span class="token punctuation">.</span><span class="token function">define</span><span class="token punctuation">(</span><span class="token string">'module.Something'</span><span class="token punctuation">,</span> <span class="token punctuation">[</span><span class="token string">'web.ajax'</span><span class="token punctuation">]</span><span class="token punctuation">,</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token parameter">require</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token string">"use strict"</span><span class="token punctuation">;</span>

    <span class="token keyword">var</span> ajax <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'web.ajax'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    
    <span class="token keyword">return</span> ajax<span class="token punctuation">.</span><span class="token function">rpc</span><span class="token punctuation">(</span><span class="token operator">...</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">then</span><span class="token punctuation">(</span><span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token parameter">result</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token comment">// some code here</span>
        <span class="token keyword">return</span> something<span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<h2>最好的练习</h2>
<ul>
<li>记住模块名的约定：插件名加上模块名后缀</li>
<li>在模块顶部声明所有依赖项。此外，它们应该按模块名称的字母顺序排序。这样更容易理解您的模块。</li>
<li>在末尾声明所有导出的值</li>
<li>尽量避免从一个模块导出过多的内容。通常最好在一个（小/更小）模块中简单地导出一件事情。</li>
<li>异步模块可以用来简化一些用例。例如，web.dom_ready模块返回一个deferred ，当dom实际就绪时，这个deferred 将被解决。因此，另一个需要dom的模块可以在某个地方简单地有一个require（“web.dom_ready”）语句，并且只有当dom准备好时才会执行代码。</li>
<li>尽量避免在一个文件中定义多个模块。这在短期内可能很方便，但实际上很难维护。</li>
</ul>
<h2>类系统</h2>
<p>Odoo是在ECMAScript 6类可用之前开发的。在ECMAScript 5中，定义类的标准方法是定义一个函数并在其原型对象上添加方法。这很好，但是当我们想要使用继承、混合时，它稍微复杂一些。<br>
出于这些原因，Odoo决定使用自己的类系统，这是受到约翰·雷西格的启发。基类位于web.class文件class.js中。</p>
<h2>创建一个子类</h2>
<p>让我们讨论如何创建类。主要机制是使用extend方法（这或多或少相当于ES6类中的extend）。</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-jsx"><code class="  language-jsx"><span class="token keyword">var</span> Class <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'web.Class'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">var</span> Animal <span class="token operator">=</span> Class<span class="token punctuation">.</span><span class="token function">extend</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
    <span class="token function-variable function">init</span><span class="token punctuation">:</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span>x <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span>hunger <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
    <span class="token function-variable function">move</span><span class="token punctuation">:</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span>x <span class="token operator">=</span> <span class="token keyword">this</span><span class="token punctuation">.</span>x <span class="token operator">+</span> <span class="token number">1</span><span class="token punctuation">;</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span>hunger <span class="token operator">=</span> <span class="token keyword">this</span><span class="token punctuation">.</span>hunger <span class="token operator">+</span> <span class="token number">1</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
    <span class="token function-variable function">eat</span><span class="token punctuation">:</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span>hunger <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>

<p>在本例中，init函数是构造函数。它将在创建实例时调用。通过使用new关键字创建实例。</p>
<h2>继承</h2>
<p>可以方便地继承现有的类。这只需在超类上使用extend方法即可完成。当调用一个方法时，框架会秘密地将一个特殊的方法_super重新绑定到当前调用的方法中。这允许我们在需要调用父方法时使用this._super。</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-jsx"><code class="  language-jsx"><span class="token keyword">var</span> Animal <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'web.Animal'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">var</span> Dog <span class="token operator">=</span> Animal<span class="token punctuation">.</span><span class="token function">extend</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
    <span class="token function-variable function">move</span><span class="token punctuation">:</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">bark</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">_super</span><span class="token punctuation">.</span><span class="token function">apply</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">,</span> arguments<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
    <span class="token function-variable function">bark</span><span class="token punctuation">:</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'woof'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">var</span> dog <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Dog</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
dog<span class="token punctuation">.</span><span class="token function">move</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<h2>混合</h2>
<p>Odoo类系统不支持多重继承，但是对于那些需要共享某些行为的情况，我们有一个混合系统：extend方法实际上可以接受任意数量的参数，并将它们组合到新的类中。</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-jsx"><code class="  language-jsx"><span class="token keyword">var</span> Animal <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'web.Animal'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">var</span> DanceMixin <span class="token operator">=</span> <span class="token punctuation">{</span>
    <span class="token function-variable function">dance</span><span class="token punctuation">:</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'dancing...'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span>

<span class="token keyword">var</span> Hamster <span class="token operator">=</span> Animal<span class="token punctuation">.</span><span class="token function">extend</span><span class="token punctuation">(</span>DanceMixin<span class="token punctuation">,</span> <span class="token punctuation">{</span>
    <span class="token function-variable function">sleep</span><span class="token punctuation">:</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'sleeping'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<p>在这个例子中，Hamter 类是Animal的子类，但是它也混合了DanceMixin.</p>
<h2>修改现有的类</h2>
<p>这并不常见，但有时我们需要在适当的位置修改另一个类。目标是有一个机制来改变一个类和所有未来/现在的实例。这是通过使用include方法完成的：</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-jsx"><code class="  language-jsx"><span class="token keyword">var</span> Hamster <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'web.Hamster'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

Hamster<span class="token punctuation">.</span><span class="token function">include</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
    <span class="token function-variable function">sleep</span><span class="token punctuation">:</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">_super</span><span class="token punctuation">.</span><span class="token function">apply</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">,</span> arguments<span class="token punctuation">)</span><span class="token punctuation">;</span>
        console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'zzzz'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<p>这显然是一个危险的操作，应该小心操作。但是，按照odoo的结构，有时需要在一个插件中修改在另一个插件中定义的widget/class的行为。请注意，它将修改类的所有实例，即使它们已经创建。</p>
<h2>小部件（Widget）</h2>
<p>widget类实际上是用户界面的一个重要构建块。几乎用户界面中的所有内容都在小部件（widget）的控制之下。widget类在widget.js中的module web.widget中定义。<br>
简而言之，widget类提供的特性包括：</p>
<ul>
<li>小部件之间的父/子关系（PropertiesMixin）</li>
<li>**具有安全功能的广泛生命周期管理 **（e.g. 在销毁父级期间自动销毁子窗口小部件）</li>
<li>自动渲染qweb模板</li>
<li>帮助与外部环境交互的各种实用功能。<br>
一个计数的小部件例子：</li>
</ul>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-jsx"><code class="  language-jsx"><span class="token keyword">var</span> Widget <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'web.Widget'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">var</span> Counter <span class="token operator">=</span> Widget<span class="token punctuation">.</span><span class="token function">extend</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
    template<span class="token punctuation">:</span> <span class="token string">'some.template'</span><span class="token punctuation">,</span>
    events<span class="token punctuation">:</span> <span class="token punctuation">{</span>
        <span class="token string">'click button'</span><span class="token punctuation">:</span> <span class="token string">'_onClick'</span><span class="token punctuation">,</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
    <span class="token function-variable function">init</span><span class="token punctuation">:</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token parameter">parent<span class="token punctuation">,</span> value</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">_super</span><span class="token punctuation">(</span>parent<span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span>count <span class="token operator">=</span> value<span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
    <span class="token function-variable function">_onClick</span><span class="token punctuation">:</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span>count<span class="token operator">++</span><span class="token punctuation">;</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">$</span><span class="token punctuation">(</span><span class="token string">'.val'</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">text</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">.</span>count<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<p>对于本例，假设模板some.template（并且正确加载：模板位于一个文件中，该文件在模块清单中的qweb键中正确定义）如下：</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-xml"><code class="  language-xml"><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>div</span> <span class="token attr-name">t-name</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>some.template<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>span</span> <span class="token attr-name">class</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>val<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>t</span> <span class="token attr-name">t-esc</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>widget.count<span class="token punctuation">"</span></span><span class="token punctuation">/&gt;</span></span><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>span</span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>button</span><span class="token punctuation">&gt;</span></span>Increment<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>button</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>div</span><span class="token punctuation">&gt;</span></span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span></span></code></pre></div>
<p>这个例子说明了小部件类的一些特性，包括事件系统、模板系统、带有初始父参数的构造函数。</p>
<h2>小部件的生命周期</h2>
<p>与许多组件系统一样，widget类有一个定义良好的生命周期。通常的生命周期如下：调用init，然后willStart，然后rendering，然后start，最后destroy。</p>
<p><code>Widget.init(parent)</code><br>
这是构造函数。init方法应该初始化小部件的基本状态。它是同步的，可以被重写以从小部件的创建者/父对象获取更多参数。</p>
<blockquote>
<p>Arguments : parent（<code>Widget（）</code>）–新的widget的父级，用于处理自动销毁和事件传播。对于没有父级的小部件，可以为<code>null</code>。</p>
</blockquote>
<p><code>Widget.willStart()</code><br>
当一个小部件被创建并被附加到DOM的过程中，框架将调用这个方法一次。willstart方法是一个钩子，它应该返回一个deferred。JS框架将等待这个deferred完成，然后再继续渲染步骤。注意，此时小部件没有dom根元素。willstart钩子主要用于执行一些异步工作，例如从服务器获取数据。</p>
<p><code>[Rendering]()</code><br>
此步骤由框架自动完成。框架会检查小部件上是否定义了template键。如果定义了，那么它将在呈现上下文中使用绑定到小部件的widget键呈现该模板（请参见上面的示例：我们在QWeb模板中使用widget.count来读取小部件的值）。如果没有定义模板，则读取 tagName 键并创建相应的DOM元素。渲染完成后，我们将结果设置为小部件的$el属性。在此之后，我们将自动绑定events和custom_events键中的所有事件。</p>
<p><code>Widget.start()</code><br>
渲染完成后，框架将自动调用Start方法。这对于执行一些特殊的后期渲染工作很有用。例如，设置库。<br>
必须返回deferred以指示其工作何时完成。</p>
<blockquote>
<p>Returns: deferred 对象</p>
</blockquote>
<p><code>Widget.destroy()</code><br>
这始终是小部件生命周期中的最后一步。当小部件被销毁时，我们基本上执行所有必要的清理操作：从组件树中删除小部件，取消绑定所有事件，…<br>
当小部件的父级被销毁时自动调用，如果小部件没有父级，或者如果它被删除但父级仍然存在，则必须显式调用。</p>
<p>请注意，不必调用willstart和start方法。可以创建一个小部件（将调用init方法），然后销毁（destroy方法），而不需要附加到DOM。如果是这种情况，将不会调用will start和start。</p>
<h2>Widget API</h2>
<p><code>Widget.tagName</code><br>
如果小部件没有定义模板，则使用。默认为DIV，将用作标记名来创建要设置为小部件的dom根的dom元素。可以使用以下属性进一步自定义生成的dom根目录：</p>
<p><code>Widget.id</code><br>
用于在生成的dom根上生成id属性。请注意，这是很少需要的，如果一个小部件可以多次使用，这可能不是一个好主意。</p>
<p><code>Widget.className</code><br>
用于在生成的dom根上生成class属性。请注意，它实际上可以包含多个css类：“some-class other-class”</p>
<p><code>Widget.attributes</code><br>
属性名到属性值的映射（对象文本）。这些k:v对中的每一个都将被设置为生成的dom根上的dom属性。</p>
<p><code>Widget.el</code><br>
将原始dom元素设置为小部件的根（仅在start lifecyle方法之后可用）</p>
<p><code>Widget.$el</code><br>
jquery封装的el，（仅在Start Lifecyle方法之后可用）</p>
<p><code>Widget.template</code><br>
应设置为QWeb模板的名称。如果设置了，模板将在小部件初始化之后但在其启动之前呈现。模板生成的根元素将被设置为小部件的dom根。</p>
<p><code>Widget.xmlDependencies</code><br>
呈现小部件之前需要加载的XML文件的路径列表。这不会导致加载已加载的任何内容。如果您想延迟加载模板，或者想要在网站和Web客户机界面之间共享一个小部件，这很有用。</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-csharp"><code class="  language-csharp"><span class="token keyword">var</span> EditorMenuBar <span class="token operator">=</span> Widget<span class="token punctuation">.</span><span class="token function">extend</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
    xmlDependencies<span class="token punctuation">:</span> <span class="token punctuation">[</span><span class="token string">'/web_editor/static/src/xml/editor.xml'</span><span class="token punctuation">]</span><span class="token punctuation">,</span>
    <span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span></span></code></pre></div>
<p><code>Widget.events</code><br>
事件是事件选择器（由空格分隔的事件名称和可选CSS选择器）到回调的映射。回调可以是小部件方法或函数对象的名称。在这两种情况下，这都将设置为小部件：</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-events"><code class="events: :  language-events">    'click p.oe_some_class a': 'some_method',
    'change input': function (e) {
        e.stopPropagation();
    }
},
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<p>选择器用于jquery的事件委托，回调只对与选择器匹配的dom根的后代触发。如果选择器被省略（只指定了一个事件名），那么事件将直接设置在小部件的dom根上。<br>
注意：不鼓励使用内联函数，将来可能会删除它。</p>
<p><code>Widget.custom_events</code></p>
<blockquote>
<p>returns: true 如果小部件正在或者已经被销毁，否则false</p>
</blockquote>
<p><code>Widget.$(selector)</code><br>
将指定为参数的CSS选择器应用于小部件的dom根:<br>
<code>this.$(selector);</code><br>
功能上与以下相同：<br>
<code>this.$el.find(selector);</code></p>
<blockquote>
<p>arguments: selector(string)-CSS选择器<br>
return:jQuery 对象</p>
</blockquote>
<blockquote>
<p>这个助手方法类似于<code>Backbone.View.$</code></p>
</blockquote>
<p><code>Widget.setElement(element)</code><br>
将小部件的dom根重新设置为提供的元素，还处理重新设置dom根的各种别名以及取消设置和重新设置委托事件。</p>
<blockquote>
<p>arguments: element(Element) -一个DOM元素或者jQuery对象设置为小部件的根DOM</p>
</blockquote>
<h2>在DOM中插入一个小部件</h2>
<p><code>Widget.appendTo(element)</code><br>
渲染小部件并将它作为子元素插入到目标元素后面，使用<code>.appentTo()</code></p>
<p><code>Widget.prependTo(element)</code><br>
渲染小部件并将它作为子元素插入到目标元素前面，使用<code>.prependTo()</code></p>
<p><code>Widget.insertAfter(element)</code><br>
渲染小部件并将它作为目标元素的前一个同级插入，使用<code>.insertAfter()</code></p>
<p><code>Widget.insertBefore(element)</code><br>
渲染小部件并将其作为目标的后一个同级插入，使用<code>.insertBefore()</code></p>
<p>所有这些方法都接受相应jquery方法接受的任何内容（css选择器、dom节点或jquery对象）。他们都会返回一个 deferred，并承担三个任务：</p>
<ul>
<li>
<strong>通过以下方式呈现小部件的根元素：</strong><br>
<code>renderElement()</code>
</li>
<li>
<strong>使用jquery在DOM中插入小部件的根元素</strong><br>
匹配的方法</li>
<li>启动小部件并返回启动结果</li>
</ul>
<h2>小部件指南</h2>
<ul>
<li>在一般应用和模块中中，标识 （id 属性）应该避免使用，ID限制了组件的可重用性，并使代码更加脆弱。大多数情况下，它们可以替换为Nothing、Classes或保留对dom节点或jquery元素的引用。<br>
如果ID是绝对必要的（因为第三方库需要一个），则应使用_.uniqueId（）部分生成ID，例如：<br>
<code>this.id = _.uniqueId('my-widget-');</code>
</li>
<li>避免使用可预测/通用的CSS类名。类名称（如“content”或“navigation”）可能与所需的含义/语义匹配，但其他开发人员可能也有相同的需求，从而造成命名冲突和意外行为。通用类名的前缀应该是它们所属组件的名称（创建“非正式”名称空间，就像在C或Objective-C中那样）。</li>
<li>应避免使用全局选择器。因为一个组件可以在一个页面中多次使用（ODoo中的一个例子是仪表板），所以查询应该限制在给定组件的范围内。未筛选的选择（如<code>$(selector)</code>或<code>document.querySelectorAll（selector）</code>）通常会导致意外或错误的行为。odoo web的widget（）有一个提供dom根（<img class="math-inline" src="https://math.jianshu.com/math?formula=el%EF%BC%89%E7%9A%84%E5%B1%9E%E6%80%A7%EF%BC%8C%E4%BB%A5%E5%8F%8A%E7%9B%B4%E6%8E%A5%E9%80%89%E6%8B%A9%E8%8A%82%E7%82%B9%E7%9A%84%E5%BF%AB%E6%8D%B7%E6%96%B9%E5%BC%8F(" alt="el）的属性，以及直接选择节点的快捷方式(" mathimg="1">_().)</li>
<li>更一般地说，不要假设您的组件拥有或控制任何超出其个人$el的东西（因此，避免使用对父部件的引用）。</li>
<li>HTML模板/渲染应该使用QWeb，除非非常简单。</li>
<li>所有交互组件（向屏幕显示信息或截取DOM事件的组件）必须继承自widget（），并正确实现和使用其API和生命周期。</li>
</ul>
<h2>Qweb模板引擎</h2>
<p>Web客户端使用QWeb模板引擎来呈现小部件（除非它们重写renderelement方法来执行其他操作）。QWebJS模板引擎基于XML，主要与Python实现兼容。</p>
<p>现在，让我们解释如何加载模板。每当Web客户端启动时，都会对/web/web client/qweb路由进行RPC。然后，服务器将返回在每个已安装模块的数据文件中定义的所有模板的列表。正确的文件列在每个模块清单的QWeb条目中。</p>
<p>在启动第一个小部件之前，Web客户机将等待加载该模板列表。</p>
<p>这个机制可以很好地满足我们的需求，但有时我们希望懒加载模板。例如，假设我们有一个很少使用的小部件。在这种情况下，我们可能不希望将其模板加载到主文件中，以便使Web客户机稍微轻一些。在这种情况下，我们可以使用小部件的xmlpendencies键：</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-jsx"><code class="  language-jsx"><span class="token keyword">var</span> Widget <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'web.Widget'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">var</span> Counter <span class="token operator">=</span> Widget<span class="token punctuation">.</span><span class="token function">extend</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
    template<span class="token punctuation">:</span> <span class="token string">'some.template'</span><span class="token punctuation">,</span>
    xmlDependencies<span class="token punctuation">:</span> <span class="token punctuation">[</span><span class="token string">'/myaddon/path/to/my/file.xml'</span><span class="token punctuation">]</span><span class="token punctuation">,</span>

    <span class="token operator">...</span>

<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<p>有了这个，计数器小部件将以willstart方法加载xmlpendencies文件，这样在执行呈现时模板就可以准备好了。</p>
<h2>事件系统</h2>
<p>目前，odoo支持两个事件系统：一个允许添加侦听器和触发事件的简单系统，以及一个更完整的系统，它还可以使事件“冒泡”。</p>
<p>这两个事件系统都在文件mixins.js的eventspatchemixin中实现。这个mixin包含在widget类中。</p>
<h2>基础事件系统</h2>
<p>这是历史上第一个事件系统。它实现了一个简单的总线模式。我们有4种主要方法：</p>
<ul>
<li>on : 在一个事件上注册监听器</li>
<li>off: 移除事件的监听器</li>
<li>once: 注册一个只使用一次的监听器</li>
<li>trigger：跟踪一个事件。这会调用所有监听器。<br>
一下是一个怎么使用事件系统的例子：</li>
</ul>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-kotlin"><code class="  language-kotlin"><span class="token keyword">var</span> Widget <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'web.Widget'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">var</span> Counter <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'myModule.Counter'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">var</span> MyWidget <span class="token operator">=</span> Widget<span class="token punctuation">.</span><span class="token function">extend</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
    start<span class="token operator">:</span> <span class="token function">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span>counter <span class="token operator">=</span> new <span class="token function">Counter</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span>counter<span class="token punctuation">.</span><span class="token function">on</span><span class="token punctuation">(</span><span class="token string">'valuechange'</span><span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">.</span>_onValueChange<span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token keyword">var</span> def <span class="token operator">=</span> <span class="token keyword">this</span><span class="token punctuation">.</span>counter<span class="token punctuation">.</span><span class="token function">appendTo</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">.</span>$el<span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token keyword">return</span> $<span class="token punctuation">.</span><span class="token function">when</span><span class="token punctuation">(</span>def<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">.</span>_super<span class="token punctuation">.</span><span class="token function">apply</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">,</span> arguments<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
    _onValueChange<span class="token operator">:</span> <span class="token function">function</span> <span class="token punctuation">(</span><span class="token keyword">val</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token comment">// do something with val</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token comment">// in Counter widget, we need to call the trigger method:</span>

<span class="token operator">..</span><span class="token punctuation">.</span> <span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">trigger</span><span class="token punctuation">(</span><span class="token string">'valuechange'</span><span class="token punctuation">,</span> someValue<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<blockquote>
<p><img class="math-inline" src="https://math.jianshu.com/math?formula=%5Ccolor%7B%23FFC125%7D%7B%E8%AD%A6%E5%91%8A%7D" alt="\color{#FFC125}{警告}" mathimg="1"><br>
不鼓励使用此事件系统，我们计划用扩展事件系统中的trigger-up方法替换每个trigger方法。</p>
</blockquote>
<h2>扩展的事件系统</h2>
<p>自定义事件小部件是一个更高级的系统，它模拟DOM事件API。每当一个事件被触发时，它将“冒泡”组件树，直到它到达根小部件，或者停止。</p>
<ul>
<li>trigger_up:这是一种方法，它将创建一个小的odooEvent并将其分派到组件树中。请注意，它将从触发事件的组件开始</li>
<li>custom_events:这相当于事件字典，但是对于odoo事件来说。<br>
OdoEvent类非常简单。它有三个公共属性：target（触发事件的小部件）、name（事件名称）和data（有效负载）。它还有两种方法：stopPropagation 和 is_stopped.。<br>
上一个示例可以更新为使用自定义事件系统：</li>
</ul>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-jsx"><code class="  language-jsx"><span class="token keyword">var</span> Widget <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'web.Widget'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">var</span> Counter <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'myModule.Counter'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">var</span> MyWidget <span class="token operator">=</span> Widget<span class="token punctuation">.</span><span class="token function">extend</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
    custom_events<span class="token punctuation">:</span> <span class="token punctuation">{</span>
        valuechange<span class="token punctuation">:</span> <span class="token string">'_onValueChange'</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
    <span class="token function-variable function">start</span><span class="token punctuation">:</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span>counter <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Counter</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token keyword">var</span> def <span class="token operator">=</span> <span class="token keyword">this</span><span class="token punctuation">.</span>counter<span class="token punctuation">.</span><span class="token function">appendTo</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">.</span>$el<span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token keyword">return</span> $<span class="token punctuation">.</span><span class="token function">when</span><span class="token punctuation">(</span>def<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">_super</span><span class="token punctuation">.</span><span class="token function">apply</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">,</span> arguments<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
    <span class="token function-variable function">_onValueChange</span><span class="token punctuation">:</span> <span class="token keyword">function</span><span class="token punctuation">(</span><span class="token parameter">event</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token comment">// do something with event.data.val</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token comment">// in Counter widget, we need to call the trigger_up method:</span>

<span class="token operator">...</span> <span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">trigger_up</span><span class="token punctuation">(</span><span class="token string">'valuechange'</span><span class="token punctuation">,</span> <span class="token punctuation">{</span>value<span class="token punctuation">:</span> someValue<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<h2>注册</h2>
<p>Odoo生态系统的一个常见需求是从外部扩展/更改基本系统的行为（通过安装应用程序，即不同的模块）。例如，可能需要在某些视图中添加新的小部件类型。在这种情况下，以及其他许多情况下，通常的过程是创建所需的组件，然后将其添加到注册表（注册步骤），以使Web客户机的其余部分知道它的存在。<br>
一下是一些在系统中可用的注册：</p>
<ul>
<li>字段注册表（由“web.field_registry”导出）。字段注册表包含Web客户端已知的所有字段小部件。每当视图（通常是表单或列表/看板）需要字段小部件时，这就是它将要查找的地方。典型的用例如下所示：</li>
</ul>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-csharp"><code class="  language-csharp"><span class="token keyword">var</span> fieldRegistry <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'web.field_registry'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">var</span> FieldPad <span class="token operator">=</span> <span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">;</span>

fieldRegistry<span class="token punctuation">.</span><span class="token keyword">add</span><span class="token punctuation">(</span><span class="token string">'pad'</span><span class="token punctuation">,</span> FieldPad<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<p>注意，每个值都应该是AbstractField的子类</p>
<ul>
<li>视图注册表：此注册表包含Web客户端已知的所有JS视图</li>
</ul>
<p>（尤其是视图管理器）。此注册表的每个值都应该是AbstractView的子类</p>
<ul>
<li>动作注册表：我们跟踪此注册表中的所有客户端动作。这个是动作管理器在需要创建客户端操作时查找的位置。在版本11中，每个值应该只是小部件的一个子类。但是，在版本12中，值必须是abstractAction。</li>
</ul>
<h2>小部件之间的通信</h2>
<p>有许多组件之间的通信方式</p>
<ul>
<li><p><strong>从父级到它的子级</strong><br>
一个简单的例子。父不见可以简单的调用子部件方法：<br>
<code>this.someWidget.update(someInfo);</code></p></li>
<li><p><strong>从一个小部件到它的父/某个祖先</strong><br>
在这种情况下，小部件的工作只是通知其环境发生了什么事情。由于我们不希望小部件具有对其父部件的引用（这将使小部件与其父部件的实现相结合），因此继续操作的最佳方法通常是使用触发器trigger_up方法触发一个事件，该事件将冒泡到组件树中：<br>
<code>this.trigger_up('open_record', { record: record, id: id});</code><br>
此事件将在小部件上触发，然后将冒泡并最终被某些上游小部件捕获：</p></li>
</ul>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-csharp"><code class="  language-csharp"><span class="token keyword">var</span> SomeAncestor <span class="token operator">=</span> Widget<span class="token punctuation">.</span><span class="token function">extend</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
    custom_events<span class="token punctuation">:</span> <span class="token punctuation">{</span>
        <span class="token string">'open_record'</span><span class="token punctuation">:</span> <span class="token string">'_onOpenRecord'</span><span class="token punctuation">,</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
    _onOpenRecord<span class="token punctuation">:</span> function <span class="token punctuation">(</span><span class="token keyword">event</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">var</span> record <span class="token operator">=</span> <span class="token keyword">event</span><span class="token punctuation">.</span>data<span class="token punctuation">.</span>record<span class="token punctuation">;</span>
        <span class="token keyword">var</span> id <span class="token operator">=</span> <span class="token keyword">event</span><span class="token punctuation">.</span>data<span class="token punctuation">.</span>id<span class="token punctuation">;</span>
        <span class="token comment">// do something with the event.</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<ul>
<li>
<strong>交叉组件</strong><br>
通过总线可以实现跨组件通信。这不是首选的通信形式，因为它有使代码难以维护的缺点。但是，它具有分离组件的优势。在这种情况下，这只是通过触发和监听总线上的事件来完成的。例如：</li>
</ul>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-jsx"><code class="  language-jsx"><span class="token comment">// in WidgetA</span>
<span class="token keyword">var</span> core <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'web.core'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">var</span> WidgetA <span class="token operator">=</span> Widget<span class="token punctuation">.</span><span class="token function">extend</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
    <span class="token operator">...</span>
    <span class="token function-variable function">start</span><span class="token punctuation">:</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        core<span class="token punctuation">.</span>bus<span class="token punctuation">.</span><span class="token function">on</span><span class="token punctuation">(</span><span class="token string">'barcode_scanned'</span><span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">.</span>_onBarcodeScanned<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token comment">// in WidgetB</span>
<span class="token keyword">var</span> WidgetB <span class="token operator">=</span> Widget<span class="token punctuation">.</span><span class="token function">extend</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
    <span class="token operator">...</span>
    <span class="token function-variable function">someFunction</span><span class="token punctuation">:</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token parameter">barcode</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        core<span class="token punctuation">.</span>bus<span class="token punctuation">.</span><span class="token function">trigger</span><span class="token punctuation">(</span><span class="token string">'barcode_scanned'</span><span class="token punctuation">,</span> barcode<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<p>在本例中，我们使用web.core导出的总线，但这不是必需的。可以为特定目的创建总线。</p>
<h2>服务services</h2>
<p>在11.0版中，我们引入了服务的概念。主要的想法是给子组件一种受控制的方式来访问它们的环境，这种方式允许框架进行足够的控制，并且是可测试的。<br>
服务系统围绕三个理念进行组织：services、service providers和widget。它的工作方式是小部件触发（使用trigger_up）事件，这些事件冒泡到服务提供者，服务提供者将要求服务执行任务，然后可能返回一个答案。</p>
<h2>服务service</h2>
<p>服务是AbstractService类的实例。它基本上只有一个名称和一些方法。它的工作是执行一些工作，通常是一些依赖于环境的工作。<br>
例如，我们有Ajax服务（任务是执行RPC）、本地存储（与浏览器本地存储交互）和许多其他服务。<br>
以下是有关如何实现Ajax服务的简化示例：</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-jsx"><code class="  language-jsx"><span class="token keyword">var</span> AbstractService <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'web.AbstractService'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">var</span> AjaxService <span class="token operator">=</span> AbstractService<span class="token punctuation">.</span><span class="token function">extend</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
    name<span class="token punctuation">:</span> <span class="token string">'ajax'</span><span class="token punctuation">,</span>
    <span class="token function-variable function">rpc</span><span class="token punctuation">:</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token parameter"><span class="token operator">...</span></span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">return</span> <span class="token operator">...</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<p>这个服务被叫做‘ajax’而且定义了一个方法，rpc.</p>
<h2>服务提供者Service Provider</h2>
<p>为了使服务正常工作，有必要让一个服务提供者准备好分派定制事件。在后端（Web客户端），这是由主Web客户端实例完成的。请注意，服务提供程序的代码来自ServiceProviderMin。</p>
<h2>部件widget</h2>
<p>小部件是请求服务的部分。为了做到这一点，它只需触发一个事件调用服务（通常通过使用helper函数调用）。此事件将冒泡并将意图传达给系统的其余部分。<br>
在实践中，有些函数被频繁地调用，以至于我们有一些助手函数使它们更容易使用。例如，_rpc方法是帮助生成rpc的助手。</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-jsx"><code class="  language-jsx"><span class="token keyword">var</span> SomeWidget <span class="token operator">=</span> Widget<span class="token punctuation">.</span><span class="token function">extend</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
    <span class="token function-variable function">_getActivityModelViewID</span><span class="token punctuation">:</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token parameter">model</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">return</span> <span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">_rpc</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
            model<span class="token punctuation">:</span> model<span class="token punctuation">,</span>
            method<span class="token punctuation">:</span> <span class="token string">'get_activity_view_id'</span>
        <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<blockquote>
<p><img class="math-inline" src="https://math.jianshu.com/math?formula=%5Ccolor%7B%23FFC125%7D%7B%E8%AD%A6%E5%91%8A%7D" alt="\color{#FFC125}{警告}" mathimg="1"><br>
如果一个小部件被销毁，它将从主组件树中分离出来，并且没有父组件。在这种情况下，事件不会冒泡，这意味着工作不会完成。这通常正是我们从一个被破坏的小部件中想要的。</p>
</blockquote>
<h2>RPCs</h2>
<p>RPC功能由Ajax服务提供。但大多数人可能只会与_rpc助手进行交互。<br>
在处理odoo时，通常有两个用例：一个需要在（python）模型上调用方法（这需要通过控制器call_kw），或者一个需要直接调用控制器（在某些路由上可用）。</p>
<ul>
<li>在python模型中调用方法</li>
</ul>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-css"><code class="  language-css"><span class="token selector">return this._rpc(</span><span class="token punctuation">{</span>
    <span class="token property">model</span><span class="token punctuation">:</span> <span class="token string">'some.model'</span><span class="token punctuation">,</span>
    <span class="token property">method</span><span class="token punctuation">:</span> <span class="token string">'some_method'</span><span class="token punctuation">,</span>
    <span class="token property">args</span><span class="token punctuation">:</span> [some<span class="token punctuation">,</span> args]<span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<ul>
<li>直接调用控制器</li>
</ul>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-css"><code class="  language-css"><span class="token selector">return this._rpc(</span><span class="token punctuation">{</span>
    <span class="token selector">route: '/some/route/',
    params:</span> <span class="token punctuation">{</span> <span class="token property">some</span><span class="token punctuation">:</span> kwargs<span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span></span></code></pre></div>
<h2>通知</h2>
<p>odoo框架有一种标准的方式来向用户传递各种信息：通知，它显示在用户界面的右上角。<br>
通知有两种类型：</p>
<ul>
<li>notification: 有助于显示一些反馈。例如，每当用户取消订阅某个频道时。</li>
<li>warning:用于显示一些重要/紧急信息。通常是系统中的大多数（可恢复的）错误。</li>
</ul>
<p>此外，通知还可以用于向用户询问问题，而不会干扰其工作流。想象一下，通过VoIP接收到的一个电话呼叫：可以显示一个带有两个按钮的粘性通知：接受和拒绝。</p>
<h2>通知系统</h2>
<p>Odoo中的通知系统设计有以下组件：</p>
<ul>
<li>a Notification widget:这是一个简单的小部件，用于创建和显示所需的信息。</li>
<li>a NotificationService:一种服务，其职责是在请求完成时（使用custom_event）创建和销毁通知。请注意，Web客户端是一个服务提供者。</li>
<li>ServiceMixin中的两个助手功能：do_notify和do_warn</li>
</ul>
<h2>显示通知</h2>
<p>显示通知的最常见方法是使用来自ServiceMixin的两种方法：</p>
<ul>
<li><p><strong>do_notify(title, message, sticky, className):</strong><br>
显示一个通知类型的通知：<br>
&nbsp;&nbsp; * title:string. 将会在顶部显示为标题<br>
&nbsp;&nbsp; * message:string. 通知的内容<br>
&nbsp;&nbsp; *  sticky : boolean,optional. 如果为真，这个通知将会一直保留直到用户解除。否则，通知将会在一段很短的时间之后自动关闭。<br>
&nbsp;&nbsp; *  calssname:string,optional.这是一个css类的名字，将会自动添加到通知中。这对样式有用，即使不鼓励使用它。</p></li>
<li><p><strong>do_warn(title, message, sticky, className):</strong><br>
显示一个警告类型的通知。<br>
&nbsp;&nbsp; * title:string.将会在顶部显示为标题<br>
&nbsp;&nbsp; * message：string.通知的内容<br>
&nbsp;&nbsp; * sticky : boolean,optional. 如果为真，这个通知将会一直保留直到用户解除。否则，通知将会在一段很短的时间之后自动关闭。<br>
&nbsp;&nbsp; *  calssname:string,optional.这是一个css类的名字，将会自动添加到通知中。这对样式有用，即使不鼓励使用它。<br>
这里有两个如何使用这两个方法的例子：</p></li>
</ul>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-cpp"><code class="  language-cpp"><span class="token comment">// note that we call _t on the text to make sure it is properly translated.</span>
<span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">do_notify</span><span class="token punctuation">(</span><span class="token function">_t</span><span class="token punctuation">(</span><span class="token string">"Success"</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token function">_t</span><span class="token punctuation">(</span><span class="token string">"Your signature request has been sent."</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">do_warn</span><span class="token punctuation">(</span><span class="token function">_t</span><span class="token punctuation">(</span><span class="token string">"Error"</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token function">_t</span><span class="token punctuation">(</span><span class="token string">"Filter name is required."</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span></span></code></pre></div>
<h2>任务栏Systray</h2>
<p>Systray是界面菜单栏的右侧部分，Web客户端在其中显示一些小部件，如消息菜单。<br>
当菜单创建SystrayMenu时，它将查找所有已注册的小部件，并将它们作为子小部件添加到适当的位置。<br>
目前没有针对Systray小工具的特定API。它们应该是简单的小部件，并且可以像使用trigger-up方法的其他小部件一样与环境通信。</p>
<h2>添加一个新得任务栏项目</h2>
<p>没有Systray注册表。添加小部件的正确方法是将其添加到类变量systraymenu.items中。</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-jsx"><code class="  language-jsx"><span class="token keyword">var</span> SystrayMenu <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'web.SystrayMenu'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">var</span> MySystrayWidget <span class="token operator">=</span> Widget<span class="token punctuation">.</span><span class="token function">extend</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
    <span class="token operator">...</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

SystrayMenu<span class="token punctuation">.</span>Items<span class="token punctuation">.</span><span class="token function">push</span><span class="token punctuation">(</span>MySystrayWidget<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<h2>排序Ordering</h2>
<p>在将小部件添加到自己之前，Systray菜单将按Sequence属性对项目进行排序。如果原型上不存在该属性，则将使用50。因此，要将Systray项目定位在靠后，可以设置一个非常高的序列号（反之，将其放在靠前的是一个较低的序列号）。<br>
<code>MySystrayWidget.prototype.sequence = 100;</code></p>
<h2>翻译管理</h2>
<p>有些翻译是在服务器端进行的（基本上是由服务器呈现或处理的所有文本字符串），但是静态文件中有需要翻译的字符串。它目前的工作方式如下：</p>
<ul>
<li>每个可翻译字符串都带有特殊的函数_t（可在js模块web.core中找到）</li>
<li>服务器使用这些字符串生成正确的PO文件。</li>
<li>每当加载Web客户机时，它将调用route/web/web client/translations，它返回所有可翻译术语的列表</li>
<li>在运行时，每当调用函数时，它都会在该列表中查找以查找转换，如果找不到转换，则返回它或原始字符串。</li>
</ul>
<p>请注意，在<a href="https://www.odoo.com/documentation/12.0/reference/translations.html" target="_blank" rel="nofollow">文档翻译</a>模块中，从服务器的角度对翻译进行了更详细的解释。<br>
javascript中的翻译有两个重要功能：_t和_lt。区别在于_lt是以懒惰的方式进行的。</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-jsx"><code class="  language-jsx"><span class="token keyword">var</span> core <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'web.core'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">var</span> _t <span class="token operator">=</span> core<span class="token punctuation">.</span>_t<span class="token punctuation">;</span>
<span class="token keyword">var</span> _lt <span class="token operator">=</span> core<span class="token punctuation">.</span>_lt<span class="token punctuation">;</span>

<span class="token keyword">var</span> SomeWidget <span class="token operator">=</span> Widget<span class="token punctuation">.</span><span class="token function">extend</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
    exampleString<span class="token punctuation">:</span> <span class="token function">_lt</span><span class="token punctuation">(</span><span class="token string">'this should be translated'</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
    <span class="token operator">...</span>
    <span class="token function-variable function">someMethod</span><span class="token punctuation">:</span> <span class="token keyword">function</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">var</span> str <span class="token operator">=</span> <span class="token function">_t</span><span class="token punctuation">(</span><span class="token string">'some text'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token operator">...</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<p>在本例中，由于在加载模块时翻译尚未准备就绪，因此必须使用_lt。<br>
注意，翻译功能需要注意。参数中给定的字符串不应是动态的。</p>
<h2>会话Session</h2>
<p>Web客户端提供了一个特定的模块，其中包含一些特定于用户当前会话的信息。一些显著的键是：</p>
<ul>
<li>uid:当前用户的id（来自于表 res.users 的ID）</li>
<li>user_name: 用户的名字，字符串类型</li>
<li>用户上下文（context [用户id,语言和时区]）</li>
<li>partner_id: 与当前用户关联的合作伙伴的ID</li>
<li>db:当前使用的数据库名字</li>
</ul>
<h2>添加信息到会话中</h2>
<p>加载/web路由后，服务器将在模板中插入一些会话信息和脚本标记。信息将从模型ir.http的方法session_info中读取。因此，如果要添加特定信息，可以通过重写session_info方法并将其添加到字典中来完成。</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-python"><code class="  language-python"><span class="token keyword">from</span> odoo <span class="token keyword">import</span> models
<span class="token keyword">from</span> odoo<span class="token punctuation">.</span>http <span class="token keyword">import</span> request


<span class="token keyword">class</span> <span class="token class-name">IrHttp</span><span class="token punctuation">(</span>models<span class="token punctuation">.</span>AbstractModel<span class="token punctuation">)</span><span class="token punctuation">:</span>
    _inherit <span class="token operator">=</span> <span class="token string">'ir.http'</span>

    <span class="token keyword">def</span> <span class="token function">session_info</span><span class="token punctuation">(</span>self<span class="token punctuation">)</span><span class="token punctuation">:</span>
        result <span class="token operator">=</span> <span class="token builtin">super</span><span class="token punctuation">(</span>IrHttp<span class="token punctuation">,</span> self<span class="token punctuation">)</span><span class="token punctuation">.</span>session_info<span class="token punctuation">(</span><span class="token punctuation">)</span>
        result<span class="token punctuation">[</span><span class="token string">'some_key'</span><span class="token punctuation">]</span> <span class="token operator">=</span> get_some_value_from_db<span class="token punctuation">(</span><span class="token punctuation">)</span>
        <span class="token keyword">return</span> result
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></code></pre></div>
<p>现在，通过在会话中读取该值，可以在javascript中获取该值：</p>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-jsx"><code class="  language-jsx"><span class="token keyword">var</span> session <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'web.session'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">var</span> myValue <span class="token operator">=</span> session<span class="token punctuation">.</span>some_key<span class="token punctuation">;</span>
<span class="token operator">...</span>
<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span></span></code></pre></div>
<p>请注意，此机制旨在减少Web客户端准备就绪所需的通信量。它更适合于计算成本较低的数据（缓慢的session_info调用将延迟为每个人加载Web客户端），以及在初始化过程早期需要的数据。</p>
<h2>视图</h2>
<p>“视图”一词有多种含义。本节是关于视图的JavaScript代码的设计，而不是Arch的结构或其他任何内容。<br>
2017年，Odoo用新架构替换了先前的视图代码。主要需要将呈现逻辑与模型逻辑分开。<br>
视图（一般意义上）现在用4个部分描述：视图、控制器、渲染器和模型。这4个部分的API在AbstractView、AbstractController、AbstractRenderer和AbstractModel类中进行了描述。</p>
<br>
<div class="image-package">
<div class="image-container" style="max-width: 655px; max-height: 173px; background-color: transparent; --darkreader-inline-bgcolor:transparent;" data-darkreader-inline-bgcolor="">
<div class="image-container-fill" style="padding-bottom: 26.41%;"></div>
<div class="image-view" data-width="655" data-height="173"><img data-original-src="//upload-images.jianshu.io/upload_images/8241736-274816349836a0b2.png" data-original-width="655" data-original-height="173" data-original-format="image/png" data-original-filesize="11142" data-image-index="0" style="cursor: zoom-in;" class="" src="//upload-images.jianshu.io/upload_images/8241736-274816349836a0b2.png?imageMogr2/auto-orient/strip|imageView2/2/w/655/format/webp"></div>
</div>
<div class="image-caption">视图结构.png</div>
</div>
<ul>
<li>视图是工厂。它的工作是获取一组字段、arch、上下文和其他一些参数，然后构造一个控制器/渲染器/模型三元组。<br>
视图的作用是用正确的信息正确地设置MVC模式的每一部分。通常，它必须处理arch字符串，并提取视图中彼此所需的数据。<br>
请注意，视图是一个类，而不是一个小部件。一旦它的工作完成，它就可以被丢弃。</li>
<li>渲染器有一个作业：表示在DOM元素中查看的数据。每个视图都可以以不同的方式呈现数据。此外，它应该监听适当的用户操作，并在必要时通知其父级（Controller）。<br>
渲染器是MVC模式中的V.</li>
<li>模型：它的工作是获取并保持视图的状态。通常，它以某种方式表示数据库中的一组记录。该模型是“业务数据”的所有者。它是MVC模式中的M.</li>
<li>Controller：它的工作是协调渲染器和模型。此外，它是Web客户端其余部分的主要入口点。例如，当用户在搜索视图中更改某些内容时，将使用适当的信息调用控制器的更新方法。<br>
它是MVC模式中的C.</li>
</ul>
<blockquote>
<p>视图的JS代码已设计为可在视图管理器/操作管理器的上下文之外使用。它们可以用于客户端操作，也可以显示在公共网站上（对资源进行一些处理）</p>
</blockquote>
<h2>字段部件</h2>
<p>该AbstractField类是在一个视图中的所有控件的基类，用于支持他们所有的视图（目前为：表格，列表，看板）。<br>
v11字段小部件与先前版本之间存在许多差异。让我们提一下最重要的一些：</p>
<ul>
<li>小部件在所有视图之间共享（表单/列表/看板）。无需再复制实现。请注意，可以为视图设置特定版本的窗口小部件，方法是在视图注册表中为视图名称添加前缀：list.many2one将优先于many2one选择。</li>
<li>小部件不再是字段值的所有者。它们仅表示数据并与视图的其余部分进行通信。</li>
<li>小部件不再需要能够在编辑和只读模式之间切换。现在，当需要进行此类更改时，窗口小部件将被销毁并再次重新呈现。这不是问题，因为他们无论如何都不拥有自己的价值</li>
<li>字段小部件可以在视图之外使用。他们的API略显笨拙，但它们的设计是独立的。</li>
</ul>
<h2>装饰与列表视图一样，字段小部件对装饰具有简单的支持。装饰的目标是有一种简单的方法来根据记录当前状态指定文本颜色。例如，</h2>
<div class="_2Uzcx_"><button class="VJbwyy" type="button" aria-label="复制代码"><i aria-label="icon: copy" class="anticon anticon-copy"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="copy" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M832 64H296c-4.4 0-8 3.6-8 8v56c0 4.4 3.6 8 8 8h496v688c0 4.4 3.6 8 8 8h56c4.4 0 8-3.6 8-8V96c0-17.7-14.3-32-32-32zM704 192H192c-17.7 0-32 14.3-32 32v530.7c0 8.5 3.4 16.6 9.4 22.6l173.3 173.3c2.2 2.2 4.7 4 7.4 5.5v1.9h4.2c3.5 1.3 7.2 2 11 2H704c17.7 0 32-14.3 32-32V224c0-17.7-14.3-32-32-32zM350 856.2L263.9 770H350v86.2zM664 888H414V746c0-22.1-17.9-40-40-40H232V264h432v624z"></path></svg></i></button><pre class="line-numbers  language-xml"><code class="  language-xml"><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>field</span>  <span class="token attr-name">name</span> <span class="token attr-value"><span class="token punctuation">=</span> “state”</span>  <span class="token attr-name">decoration-danger</span> <span class="token attr-value"><span class="token punctuation">=</span> “amount＆lt;</span> <span class="token attr-name">10000”</span> <span class="token punctuation">/&gt;</span></span>
<span aria-hidden="true" class="line-numbers-rows"><span></span></span></code></pre></div>
<p>有效的装饰名字有：</p>
<ul>
<li>decoration-bf</li>
<li>decoration-it</li>
<li>decoration-danger</li>
<li>decoration-info</li>
<li>decoration-muted</li>
<li>decoration-primary</li>
<li>decoration-success</li>
<li>decoration-warning<br>
每个装饰decoration-X将映射到css类text-X，这是一个标准的bootstrap css类（text-it和text-bf除外，它们由odoo处理并分别对应于斜体和粗体）。请注意，decoration属性的值应该是一个有效的python表达式，它将使用记录作为评估上下文进行评估。</li>
</ul>
</article><div></div><div class="_1kCBjS"><div class="_18vaTa"><div class="_3BUZPB"><div class="_2Bo4Th" role="button" tabindex="-1" aria-label="给文章点赞"><i aria-label="ic-like" class="anticon"><svg width="1em" height="1em" fill="currentColor" aria-hidden="true" focusable="false" class="" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><use xlink:href="#ic-like"></use></svg></i></div><span class="_1LOh_5" role="button" tabindex="-1" aria-label="查看点赞列表">5人点赞<i aria-label="icon: right" class="anticon anticon-right"><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="right" width="1em" height="1em" fill="currentColor" aria-hidden="true" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><path d="M765.7 486.8L314.9 134.7A7.97 7.97 0 0 0 302 141v77.3c0 4.9 2.3 9.6 6.1 12.6l360 281.1-360 281.1c-3.9 3-6.1 7.7-6.1 12.6V883c0 6.7 7.7 10.4 12.9 6.3l450.8-352.1a31.96 31.96 0 0 0 0-50.4z"></path></svg></i></span></div><div class="_3BUZPB"><div class="_2Bo4Th" role="button" tabindex="-1"><i aria-label="ic-dislike" class="anticon"><svg width="1em" height="1em" fill="currentColor" aria-hidden="true" focusable="false" class="" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><use xlink:href="#ic-dislike"></use></svg></i></div></div></div><div class="_18vaTa"><a class="_3BUZPB _1x1ok9 _1OhGeD" href="/nb/23046377" target="_blank" rel="noopener noreferrer"><i aria-label="ic-notebook" class="anticon"><svg width="1em" height="1em" fill="currentColor" aria-hidden="true" focusable="false" class="" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><use xlink:href="#ic-notebook"></use></svg></i><span>odoo随记</span></a><div class="_3BUZPB ant-dropdown-trigger"><div class="_2Bo4Th"><i aria-label="ic-others" class="anticon"><svg width="1em" height="1em" fill="currentColor" aria-hidden="true" focusable="false" class="" data-darkreader-inline-fill="" style="--darkreader-inline-fill:currentColor;"><use xlink:href="#ic-others"></use></svg></i></div></div></div></div><div class="_19DgIp" style="margin-top:24px;margin-bottom:24px"></div><div class="_13lIbp"><div class="_191KSt">"小礼物走一走，来简书关注我"</div><button type="button" class="_1OyPqC _3Mi9q9 _2WY0RL _1YbC5u"><span>赞赏支持</span></button><span class="_3zdmIj">还没有人赞赏，支持一下</span></div><div class="d0hShY"><a class="_1OhGeD" href="/u/ecd4e741f182" target="_blank" rel="noopener noreferrer"><img class="_27NmgV" src="https://cdn2.jianshu.io/assets/default_avatar/4-3397163ecdb3855a0a4139c34a695885.jpg" alt="  "></a><div class="Uz-vZq"><div class="Cqpr1X"><a class="HC3FFO _1OhGeD" href="/u/ecd4e741f182" title="dooms21day" target="_blank" rel="noopener noreferrer">dooms21day</a><span class="_2WEj6j" title=""></span></div><div class="lJvI3S"><span>总资产1 (约0.10元)</span><span>共写了2.1W字</span><span>获得7个赞</span><span>共20个粉丝</span></div></div><button data-locale="zh-CN" type="button" class="_1OyPqC _3Mi9q9"><span>关注</span></button></div></section>
</div>



</body>

</html>