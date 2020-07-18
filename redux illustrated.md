---


---

<hr>
<hr>
<h1 id="redux-illustrated-in-a-react-app">Redux Illustrated in a React app</h1>
<p>Hi! I’m Lossushi. This is a tutorial on <strong>Redux</strong> and a little on <strong>React</strong>. We will see the process of an app and <strong>how</strong> to structure it for best efficiency. In other words, <strong>Why</strong> and <strong>When</strong> should I use redux?</p>
<h3 id="table-of-contents">table of contents</h3>
<ul>
<li>React
<ul>
<li><a href="#what-are-we-building">What are we building?</a></li>
<li><a href="#why-are-we-building-such-a-weird-house">Why are we building such a weird house?</a></li>
<li><a href="#memoization">Memoization</a></li>
</ul>
</li>
<li>Redux and React-Redux
<ul>
<li><a href="#and-then-comes-redux">And then comes Redux</a></li>
<li><a href="#adding-redux-and-react-redux">Adding Redux and React-Redux</a></li>
<li><a href="#last-tweak-for-efficiency">Last tweak for efficiency</a></li>
</ul>
</li>
<li><a href="#a-few-last-words">A few last words</a></li>
<li><a href="#links-to-code">Just show me the full code</a></li>
<li><a href="#follow-me">Follow me</a></li>
</ul>
<p>Before getting to Redux, let’s code the app in React and analyze how it works.</p>
<h2 id="what-are-we-building">What are we building?</h2>
<p>A very simple app made of three components. A main one: House, and two nested components: Switch and Lamp.</p>
<p>As some of you may have already guessed, Switch and Lamp will share some changing data (namely turning the light on and off) and since these components are both nested we need to lift the state up to their closest common ancestor, House.</p>
<p>For those who need a reminder, here is the link to the React documentation on <a href="https://reactjs.org/docs/lifting-state-up.html">lifting up the state</a>.</p>
<ul>
<li>House : holding the state
<ul>
<li>Switch : receiving the action as props</li>
<li>Lamp : receiving the state variable as props</li>
</ul>
</li>
</ul>
<blockquote>
<p>Note : We will be using React Hooks and its useState() to hold the state in the House component. We could use class components but there is really no need and function components are so much shorter to code.</p>
</blockquote>
<p>Let’s do that :</p>
<p>Four files : one for each components and a CSS file .</p>
<p>House.js</p>
<pre class="  language-javascript"><code class="prism  language-javascript"><span class="token keyword">import</span> React<span class="token punctuation">,</span> <span class="token punctuation">{</span> useState <span class="token punctuation">}</span> <span class="token keyword">from</span> <span class="token string">"react"</span><span class="token punctuation">;</span>
<span class="token keyword">function</span> <span class="token function">House</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token keyword">const</span> <span class="token punctuation">[</span>onOff<span class="token punctuation">,</span> setOnOff<span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token function">useState</span><span class="token punctuation">(</span><span class="token boolean">false</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token keyword">return</span> <span class="token punctuation">(</span>
    <span class="token operator">&lt;</span>div<span class="token operator">&gt;</span>
      <span class="token operator">&lt;</span>Switch setOnOff<span class="token operator">=</span><span class="token punctuation">{</span>setOnOff<span class="token punctuation">}</span> <span class="token operator">/</span><span class="token operator">&gt;</span>
      <span class="token operator">&lt;</span>Lamp onOff<span class="token operator">=</span><span class="token punctuation">{</span>onOff<span class="token punctuation">}</span> <span class="token operator">/</span><span class="token operator">&gt;</span>
    <span class="token operator">&lt;</span><span class="token operator">/</span>div<span class="token operator">&gt;</span>
  <span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<p>Switch.js</p>
<pre class="  language-javascript"><code class="prism  language-javascript"><span class="token keyword">function</span> <span class="token function">Switch</span><span class="token punctuation">(</span><span class="token punctuation">{</span> setOnOff <span class="token punctuation">}</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token keyword">return</span> <span class="token punctuation">(</span>
    <span class="token operator">&lt;</span>button onClick<span class="token operator">=</span><span class="token punctuation">{</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token function">setOnOff</span><span class="token punctuation">(</span><span class="token punctuation">(</span>currentState<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token operator">!</span>currentState<span class="token punctuation">)</span><span class="token punctuation">}</span><span class="token operator">&gt;</span>
      On<span class="token operator">/</span>Off
    <span class="token operator">&lt;</span><span class="token operator">/</span>button<span class="token operator">&gt;</span>
  <span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<p>Lamp.js</p>
<pre class="  language-javascript"><code class="prism  language-javascript"><span class="token keyword">function</span> <span class="token function">Lamp</span><span class="token punctuation">(</span><span class="token punctuation">{</span> onOff <span class="token punctuation">}</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token keyword">return</span> <span class="token operator">&lt;</span>div className<span class="token operator">=</span><span class="token punctuation">{</span>onOff <span class="token operator">?</span> <span class="token string">"lamp on"</span> <span class="token punctuation">:</span> <span class="token string">"lamp off"</span><span class="token punctuation">}</span> <span class="token operator">/</span><span class="token operator">&gt;</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<p>style.css</p>
<pre class="  language-css"><code class="prism  language-css"><span class="token selector"><span class="token class">.lamp</span> </span><span class="token punctuation">{</span>
  <span class="token property">width</span><span class="token punctuation">:</span> <span class="token number">100</span>px<span class="token punctuation">;</span>
  <span class="token property">height</span><span class="token punctuation">:</span> <span class="token number">100</span>px<span class="token punctuation">;</span>
  <span class="token property">border-radius</span><span class="token punctuation">:</span> <span class="token number">50%</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
<span class="token selector"><span class="token class">.on</span> </span><span class="token punctuation">{</span>
  <span class="token property">background</span><span class="token punctuation">:</span> <span class="token hexcode">#ff0</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
<span class="token selector"><span class="token class">.off</span> </span><span class="token punctuation">{</span>
  <span class="token property">background</span><span class="token punctuation">:</span> <span class="token function">rgba</span><span class="token punctuation">(</span><span class="token number">165</span>, <span class="token number">165</span>, <span class="token number">0</span>, <span class="token number">0.705</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<p><em>Really simple !</em><br>
Find this code on codesandbox :<a href="https://codesandbox.io/s/redux-illustrated-react-only-version-rgcrs?file=/src/Home.js">The react only version</a></p>
<h2 id="why-are-we-building-such-a-weird-house">Why are we building such a weird house?</h2>
<p><em>One would usually want the switch in the same room as the lamp.</em></p>
<p>We want to observe what happens when we click on the Switch’s button. Specifically which component is rendered and in what order .</p>
<p>To help with that we can add the following to each component and check our console.</p>
<pre class="  language-javascript"><code class="prism  language-javascript">console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">"rendering [component's name]"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<p><strong>Reminder</strong> : (and that goes for <code>setState()</code> in class components as well) When a variable in the state changes react re-renders.</p>
<p>Here is a diagram of our app to help visualize the process :</p>
<div class="mermaid"><svg xmlns="http://www.w3.org/2000/svg" id="mermaid-svg-bUqnuZ4141fY3IrO" width="100%" viewBox="0 0 325.68333435058594 240.27830505371094"><g transform="translate(-12, -12)"><g class="output"><g class="clusters"></g><g class="edgePaths"><g class="edgePath"><path class="path" d="M152.01542066896815,95.97290739382169L86.96666717529297,159.20331573486328L86.96666717529297,197.5616455078125" marker-end="url(#arrowhead996)"></path><defs><marker id="arrowhead996" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath"></path></marker></defs></g><g class="edgePath"><path class="path" d="M202.75958239252392,95.97290890996635L266.80833435058594,159.20331573486328L266.80833435058594,197.5616455078125" marker-end="url(#arrowhead997)"></path><defs><marker id="arrowhead997" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath"></path></marker></defs></g></g><g class="edgeLabels"><g class="edgeLabel" transform="translate(86.96666717529297,159.20331573486328)"><g transform="translate(-21.71666717529297,-13.358329772949219)" class="label"><div xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel">action</span></div></g></g><g class="edgeLabel" transform="translate(266.80833435058594,159.20331573486328)"><g transform="translate(-46.65833282470703,-13.358329772949219)" class="label"><div xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel">state variable</span></div></g></g></g><g class="nodes"><g class="node" id="A" transform="translate(176.88750076293945,70.42249298095703)"><polygon points="50.42249450683594,0 100.84498901367188,-50.42249450683594 50.42249450683594,-100.84498901367188 0,-50.42249450683594" rx="5" ry="5" transform="translate(-50.42249450683594,50.42249450683594)"></polygon><g class="label" transform="translate(0,0)"><g transform="translate(-22.666664123535156,-13.358329772949219)"><div xmlns="http://www.w3.org/1999/xhtml">House</div></g></g></g><g class="node" id="B" transform="translate(86.96666717529297,220.91997528076172)"><rect rx="0" ry="0" x="-66.96666717529297" y="-23.35832977294922" width="133.93333435058594" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-56.96666717529297,-13.358329772949219)"><div xmlns="http://www.w3.org/1999/xhtml">Room A : Switch</div></g></g></g><g class="node" id="C" transform="translate(266.80833435058594,220.91997528076172)"><rect rx="0" ry="0" x="-62.875" y="-23.35832977294922" width="125.75" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-52.875,-13.358329772949219)"><div xmlns="http://www.w3.org/1999/xhtml">Room B : Lamp</div></g></g></g></g></g></g></svg></div>
<p>I click on my button in my Switch component. The ‘action’ that was passed down as a props sends the change of the state back up to the House component thus House is re-rendered.<br>
Since Switch and Lamp are nested in House they also end up being re-rendered.<br>
The whole app has been re-rendered.</p>
<p><em><strong>Now that is not efficient at all!</strong></em><br>
Imagine the Switch component is holding some expensive data, it will have to re-render it even though the change of state affects only the holder of the state (House) and where the variable is used (Lamp).</p>
<p>Before wowing you with the efficiency of Redux, I would like to point out that it is possible to avoid Switch from being re-rendered.</p>
<h2 id="memoization">Memoization</h2>
<p><em>This is not a mistype. Here’s the idea :</em></p>
<p>Since the props does not change (here our setOnOff function), React can skip rendering the Switch, and reuse the last rendered result. (Only the variable in the state changes which is in both House and Lamp).</p>
<p>Here is the doc on memoization : <a href="https://reactjs.org/docs/react-api.html#reactmemo">React.memo</a> for function components and <a href="https://reactjs.org/docs/react-api.html#reactpurecomponent">React.PureComponent</a> for class components.</p>
<p>That’s how the new code looks like :</p>
<pre class="  language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> Switch <span class="token operator">=</span> React<span class="token punctuation">.</span><span class="token function">memo</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token punctuation">{</span> setOnOff <span class="token punctuation">}</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  <span class="token keyword">return</span> <span class="token punctuation">(</span>
    <span class="token operator">&lt;</span>button onClick<span class="token operator">=</span><span class="token punctuation">{</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token function">setOnOff</span><span class="token punctuation">(</span>currentState <span class="token operator">=&gt;</span> <span class="token operator">!</span>currentState<span class="token punctuation">)</span><span class="token punctuation">}</span><span class="token operator">&gt;</span>
    On<span class="token operator">/</span>Off
    <span class="token operator">&lt;</span><span class="token operator">/</span>button<span class="token operator">&gt;</span>
  <span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<p>Code : <a href="https://codesandbox.io/s/redux-illustrated-memo-version-oe18j?file=/src/Home.js">The react.memo version</a></p>
<p>Now when the app is first rendered we will see logged :</p>
<pre><code>rendering House
rendering Switch 
rendering Lamp
</code></pre>
<p>then at every click only :</p>
<pre><code>rendering House
rendering Lamp
</code></pre>
<p><em>Well that is more efficient than the previous version but what if the expensive data is in House instead of Switch we would still have a slow app.</em></p>
<h2 id="and-then-comes-redux">And then comes Redux</h2>
<p><em><strong>The following is the analogy not to miss !!!</strong></em></p>
<p>Before we  get technical, I would like you to imagine the state which is held in House as an electric panel. We have seen this, every time we hit the switch the current (data in our case) goes up to the panel (the state, causing re-render) and down to the lamp.</p>
<p>What if we could take the electric panel out of the House and store it in the shed or the garage, well anywhere outside the House really. Quite literally that is exactly what Redux allows us to do.</p>
<p>Here is the diagram of what our app will look like :</p>
<div class="mermaid"><svg xmlns="http://www.w3.org/2000/svg" id="mermaid-svg-MmlGddwtjmNgHx5J" width="100%" viewBox="0 0 344.7891616821289 240.27830505371094"><g transform="translate(-12, -12)"><g class="output"><g class="clusters"></g><g class="edgePaths"><g class="edgePath"><path class="path" d="M92.20623539334551,105.22622211819902L66.10833358764648,159.20331573486328L79.07226981096237,197.5616455078125" marker-end="url(#arrowhead1009)"></path><defs><marker id="arrowhead1009" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath"></path></marker></defs></g><g class="edgePath"><path class="path" d="M134.96825716746923,94.70173413502101L207.29874801635742,159.20331573486328L256.1600438936209,197.5616455078125" marker-end="url(#arrowhead1010)"></path><defs><marker id="arrowhead1010" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath"></path></marker></defs></g><g class="edgePath"><path class="path" d="M238.88417840683513,93.78082275390625L165.58208084106445,159.20331573486328L116.72078496380098,197.5616455078125" marker-end="url(#arrowhead1011)"></path><defs><marker id="arrowhead1011" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath"></path></marker></defs></g><g class="edgePath"><path class="path" d="M276.7813656692508,93.78082275390625L309.6224937438965,159.20331573486328L294.8872176242364,197.5616455078125" marker-end="url(#arrowhead1012)"></path><defs><marker id="arrowhead1012" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath"></path></marker></defs></g></g><g class="edgeLabels"><g class="edgeLabel" transform=""><g transform="translate(0,0)" class="label"><div xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></g></g><g class="edgeLabel" transform=""><g transform="translate(0,0)" class="label"><div xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></g></g><g class="edgeLabel" transform="translate(165.58208084106445,159.20331573486328)"><g transform="translate(-21.71666717529297,-13.358329772949219)" class="label"><div xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel">action</span></div></g></g><g class="edgeLabel" transform="translate(309.6224937438965,159.20331573486328)"><g transform="translate(-27.416664123535156,-13.358329772949219)" class="label"><div xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel">variable</span></div></g></g></g><g class="nodes"><g class="node" id="A" transform="translate(107.82500076293945,70.42249298095703)"><polygon points="50.42249450683594,0 100.84498901367188,-50.42249450683594 50.42249450683594,-100.84498901367188 0,-50.42249450683594" rx="5" ry="5" transform="translate(-50.42249450683594,50.42249450683594)"></polygon><g class="label" transform="translate(0,0)"><g transform="translate(-22.666664123535156,-13.358329772949219)"><div xmlns="http://www.w3.org/1999/xhtml">House</div></g></g></g><g class="node" id="B" transform="translate(86.96666717529297,220.91997528076172)"><rect rx="0" ry="0" x="-66.96666717529297" y="-23.35832977294922" width="133.93333435058594" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-56.96666717529297,-13.358329772949219)"><div xmlns="http://www.w3.org/1999/xhtml">Room A : Switch</div></g></g></g><g class="node" id="C" transform="translate(285.9141616821289,220.91997528076172)"><rect rx="0" ry="0" x="-62.875" y="-23.35832977294922" width="125.75" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-52.875,-13.358329772949219)"><div xmlns="http://www.w3.org/1999/xhtml">Room B : Lamp</div></g></g></g><g class="node" id="D" transform="translate(265.0558280944824,70.42249298095703)"><rect rx="5" ry="5" x="-56.80833435058594" y="-23.35832977294922" width="113.61666870117188" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-46.80833435058594,-13.358329772949219)"><div xmlns="http://www.w3.org/1999/xhtml">Redux's store</div></g></g></g></g></g></g></svg></div>
<p>With this architecture in our code, every time we switch on and off, it goes straight to the store then to the lamp. We have cut the middle man thus House will not re-render which also means we do not need to use memoization on Switch anymore.</p>
<p><strong>That’s sounds awesome ! Why haven’t we done that from the start ?!</strong></p>
<p>While it <strong>is awesome</strong>, it is also lengthy to code. Which is why I wanted you to properly understand the mechanics, then <strong>you will know when use Redux</strong>.</p>
<h2 id="adding-redux-and-react-redux">Adding Redux and React-Redux</h2>
<p>A little explanation on how to add Redux to your app.<br>
Redux is made of :</p>
<ul>
<li>a store</li>
<li>action(s)</li>
<li>reducer(s)</li>
<li>dispatch</li>
<li>display</li>
</ul>
<p><em>Don’t be intimidated by the list it is actually quite easy !</em></p>
<p>Think of it this way :<br>
<em>Here comes another analogy. I love analogies !</em> 😄</p>
<pre class="  language-javascript"><code class="prism  language-javascript"><span class="token keyword">let</span> store <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>			<span class="token comment">// store : global scope variable</span>
<span class="token keyword">function</span> <span class="token function">add</span><span class="token punctuation">(</span>param<span class="token punctuation">)</span><span class="token punctuation">{</span>		<span class="token comment">// action : description</span>
  store <span class="token operator">+=</span> param<span class="token punctuation">;</span>		<span class="token comment">// reducer : compute</span>
  <span class="token keyword">return</span> console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>store<span class="token punctuation">)</span><span class="token punctuation">;</span>	<span class="token comment">// display : show result (inside Lamp)</span>
<span class="token punctuation">}</span>
<span class="token function">add</span><span class="token punctuation">(</span><span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span>				<span class="token comment">// dispatch : invoke function (inside Switch)</span>
</code></pre>
<p>The new files :</p>
<p>Index.js</p>
<blockquote>
<p>First we “initialize” the store at a global scope so that the whole app has access to it.<br>
If I were to use our house analogy we simply need to extend a wire from the electrical panel to any room in the house to have electricity.</p>
</blockquote>
<pre class="  language-javascript"><code class="prism  language-javascript"><span class="token keyword">import</span> <span class="token punctuation">{</span> Provider <span class="token punctuation">}</span> <span class="token keyword">from</span> <span class="token string">"react-redux"</span><span class="token punctuation">;</span>
<span class="token keyword">import</span> <span class="token punctuation">{</span> createStore <span class="token punctuation">}</span> <span class="token keyword">from</span> <span class="token string">"redux"</span><span class="token punctuation">;</span>
<span class="token keyword">import</span> <span class="token punctuation">{</span> reducer <span class="token punctuation">}</span> <span class="token keyword">from</span> <span class="token string">"./Redux/Reducer"</span><span class="token punctuation">;</span>
</code></pre><p><span class="token keyword">const</span> store <span class="token operator">=</span> <span class="token function">createStore</span><span class="token punctuation">(</span>reducer<span class="token punctuation">)</span><span class="token punctuation">;</span><br>
<span class="token keyword">const</span> rootElement <span class="token operator">=</span> document<span class="token punctuation">.</span><span class="token function">getElementById</span><span class="token punctuation">(</span><span class="token string">“root”</span><span class="token punctuation">)</span><span class="token punctuation">;</span><br>
ReactDOM<span class="token punctuation">.</span><span class="token function">render</span><span class="token punctuation">(</span><br>
<span class="token operator">&lt;</span>Provider store<span class="token operator">=</span><span class="token punctuation">{</span>store<span class="token punctuation">}</span><span class="token operator">&gt;</span><br>
<span class="token operator">&lt;</span>Home <span class="token operator">/</span><span class="token operator">&gt;</span><br>
<span class="token operator">&lt;</span><span class="token operator">/</span>Provider<span class="token operator">&gt;</span><span class="token punctuation">,</span><br>
rootElement<br>
<span class="token punctuation">)</span><span class="token punctuation">;</span><br>
</p>
<p>ActionType.js</p>
<blockquote>
<p>It not a necessity to have an actionType.js file but saving your action’s names in contant variables is prone to less bugs or mistakes.</p>
</blockquote>
<pre class="  language-javascript"><code class="prism  language-javascript"><span class="token keyword">export</span> <span class="token keyword">const</span> TOGGLE <span class="token operator">=</span> <span class="token string">"TOGGLE"</span><span class="token punctuation">;</span>
</code></pre>
<p>Action.js</p>
<blockquote>
<p>Like I said above this is the description of the action. It is a simple javascript function that returns an object.</p>
</blockquote>
<pre class="  language-javascript"><code class="prism  language-javascript"><span class="token keyword">import</span> <span class="token punctuation">{</span> TOGGLE <span class="token punctuation">}</span> <span class="token keyword">from</span> <span class="token string">"../ActionType"</span><span class="token punctuation">;</span>
</code></pre><p><span class="token keyword">export</span> <span class="token keyword">const</span> <span class="token function-variable function">setOnOff</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span><br>
<span class="token keyword">return</span> <span class="token punctuation">{</span><br>
type<span class="token punctuation">:</span> TOGGLE<br>
<span class="token punctuation">}</span><span class="token punctuation">;</span><br>
<span class="token punctuation">}</span><span class="token punctuation">;</span><br>
</p>
<p>Reducer.js</p>
<blockquote>
<p>Given the action it updates the state. (The computation part). Again this is simple javascript.</p>
</blockquote>
<pre class="  language-javascript"><code class="prism  language-javascript"><span class="token keyword">import</span> <span class="token punctuation">{</span> TOGGLE <span class="token punctuation">}</span> <span class="token keyword">from</span> <span class="token string">"../ActionType"</span><span class="token punctuation">;</span>
</code></pre><p><span class="token keyword">const</span> initialState <span class="token operator">=</span> <span class="token punctuation">{</span><br>
onOff<span class="token punctuation">:</span> <span class="token boolean">false</span><span class="token punctuation">,</span><br>
user<span class="token punctuation">:</span> <span class="token string">“Bob”</span><br>
<span class="token punctuation">}</span><span class="token punctuation">;</span></p>
<p><span class="token keyword">export</span> <span class="token keyword">const</span> <span class="token function-variable function">reducer</span> <span class="token operator">=</span> <span class="token punctuation">(</span>state <span class="token operator">=</span> initialState<span class="token punctuation">,</span> action<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span><br>
<span class="token keyword">switch</span> <span class="token punctuation">(</span>action<span class="token punctuation">.</span>type<span class="token punctuation">)</span> <span class="token punctuation">{</span><br>
<span class="token keyword">case</span> TOGGLE<span class="token punctuation">:</span><br>
<span class="token keyword">return</span> <span class="token punctuation">{</span> <span class="token operator">…</span>state<span class="token punctuation">,</span> onOff<span class="token punctuation">:</span> <span class="token operator">!</span>state<span class="token punctuation">.</span>onOff <span class="token punctuation">}</span><span class="token punctuation">;</span><br>
<span class="token keyword">default</span><span class="token punctuation">:</span><br>
<span class="token keyword">return</span> state<span class="token punctuation">;</span><br>
<span class="token punctuation">}</span><br>
<span class="token punctuation">}</span><span class="token punctuation">;</span><br>
</p>
<p>The updated files :</p>
<p>Home.js</p>
<blockquote>
<p>We clean the house from the old wiring.</p>
</blockquote>
<pre class="  language-javascript"><code class="prism  language-javascript"><span class="token keyword">function</span> <span class="token function">House</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">"rendering Home"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">return</span> <span class="token punctuation">(</span>
  <span class="token operator">&lt;</span>div<span class="token operator">&gt;</span>
    <span class="token operator">&lt;</span>Switch <span class="token operator">/</span><span class="token operator">&gt;</span>
    <span class="token operator">&lt;</span>Lamp <span class="token operator">/</span><span class="token operator">&gt;</span>
  <span class="token operator">&lt;</span><span class="token operator">/</span>div<span class="token operator">&gt;</span>
  <span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<p>Switch.js</p>
<blockquote>
<p>Dispatch : with the help of the react-redux function connect() actions are mapped to the props. It is react-redux way of passing down the action like we did in the Home.js file : <code>&lt;Switch setOnOff={setOnOff} /&gt;</code></p>
</blockquote>
<pre class="  language-javascript"><code class="prism  language-javascript"><span class="token keyword">import</span> <span class="token punctuation">{</span> setOnOff <span class="token punctuation">}</span> <span class="token keyword">from</span> <span class="token string">"./Redux/Action"</span><span class="token punctuation">;</span>
<span class="token keyword">import</span> <span class="token punctuation">{</span> connect <span class="token punctuation">}</span> <span class="token keyword">from</span> <span class="token string">"react-redux"</span><span class="token punctuation">;</span>
</code></pre><p><span class="token keyword">function</span> <span class="token function">Switch</span><span class="token punctuation">(</span><span class="token punctuation">{</span> setOnOff <span class="token punctuation">}</span><span class="token punctuation">)</span> <span class="token punctuation">{</span><br>
console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">“rendering Switch”</span><span class="token punctuation">)</span><span class="token punctuation">;</span><br>
<span class="token keyword">return</span> <span class="token operator">&lt;</span>button  onClick<span class="token operator">=</span><span class="token punctuation">{</span>setOnOff<span class="token punctuation">}</span><span class="token operator">&gt;</span>On<span class="token operator">/</span>Off<span class="token operator">&lt;</span><span class="token operator">/</span>button<span class="token operator">&gt;</span><span class="token punctuation">;</span><br>
<span class="token punctuation">}</span></p>
<p><span class="token keyword">export</span> <span class="token keyword">default</span> <span class="token function">connect</span><span class="token punctuation">(</span><span class="token keyword">null</span><span class="token punctuation">,</span> <span class="token punctuation">{</span> setOnOff <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">(</span>Switch<span class="token punctuation">)</span><span class="token punctuation">;</span><br>
</p>
<p>Lamp.js</p>
<blockquote>
<p>Display : basically the same as dispatch for mapping the state to props (<code>&lt;Lamp onOff={onOff} /&gt;</code>).</p>
</blockquote>
<pre class="  language-javascript"><code class="prism  language-javascript"><span class="token keyword">function</span> <span class="token function">Lamp</span><span class="token punctuation">(</span><span class="token punctuation">{</span> onOff<span class="token punctuation">,</span> user <span class="token punctuation">}</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">"rendering Lamp"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token keyword">return</span> <span class="token punctuation">(</span>
    <span class="token operator">&lt;</span>div<span class="token operator">&gt;</span>
      <span class="token operator">&lt;</span>p<span class="token operator">&gt;</span><span class="token punctuation">{</span> user <span class="token punctuation">}</span> is turning on and off the <span class="token keyword">switch</span><span class="token operator">&lt;</span><span class="token operator">/</span>p<span class="token operator">&gt;</span>
      <span class="token operator">&lt;</span>div className<span class="token operator">=</span><span class="token punctuation">{</span>onOff <span class="token operator">?</span> <span class="token string">"lamp on"</span> <span class="token punctuation">:</span> <span class="token string">"lamp off"</span><span class="token punctuation">}</span> <span class="token operator">/</span><span class="token operator">&gt;</span>
    <span class="token operator">&lt;</span><span class="token operator">/</span>div<span class="token operator">&gt;</span>
  <span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre><p><span class="token keyword">export</span> <span class="token keyword">default</span> <span class="token function">connect</span><span class="token punctuation">(</span>state <span class="token operator">=&gt;</span> state<span class="token punctuation">)</span><span class="token punctuation">(</span>Lamp<span class="token punctuation">)</span><span class="token punctuation">;</span><br>
</p>
<p>Code : <a href="https://codesandbox.io/s/redux-illustrated-redux-version-l75ld?file=/src/Home.js">The redux version</a></p>
<h2 id="last-tweak-for-efficiency">Last tweak for efficiency</h2>
<p>This last part is really when the state has lots of data. Especially data unrelated to the current component. So we need to filter the state. Technically this has nothing to with Redux but this article addresses the issue of efficiency so let’ get to it.</p>
<p><em>It will be very short since our state has so little data.</em></p>
<p>For a more complex example check the <a href="https://react-redux.js.org/introduction/basic-tutorial#basic-tutorial">React-Redux basic tutorial</a>.</p>
<p>The new file :<br>
Selector.js</p>
<pre class="  language-javascript"><code class="prism  language-javascript"><span class="token keyword">export</span> <span class="token keyword">const</span> <span class="token function-variable function">getOnOff</span> <span class="token operator">=</span> store <span class="token operator">=&gt;</span> store<span class="token punctuation">.</span>onOff<span class="token punctuation">;</span>
</code></pre>
<p>The updated file :<br>
Lamp.js</p>
<pre class="  language-javascript"><code class="prism  language-javascript"><span class="token keyword">function</span> <span class="token function">Lamp</span><span class="token punctuation">(</span><span class="token punctuation">{</span> onOff<span class="token punctuation">,</span> user <span class="token punctuation">}</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">"rendering Lamp"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token keyword">return</span> <span class="token punctuation">(</span>
    <span class="token operator">&lt;</span>div<span class="token operator">&gt;</span>
      <span class="token operator">&lt;</span>p<span class="token operator">&gt;</span><span class="token punctuation">{</span>user <span class="token operator">||</span> <span class="token string">"Default user"</span><span class="token punctuation">}</span> is turning on and off the <span class="token keyword">switch</span><span class="token operator">&lt;</span><span class="token operator">/</span>p<span class="token operator">&gt;</span>
      <span class="token operator">&lt;</span>div className<span class="token operator">=</span><span class="token punctuation">{</span>onOff <span class="token operator">?</span> <span class="token string">"lamp on"</span> <span class="token punctuation">:</span> <span class="token string">"lamp off"</span><span class="token punctuation">}</span> <span class="token operator">/</span><span class="token operator">&gt;</span>
    <span class="token operator">&lt;</span><span class="token operator">/</span>div<span class="token operator">&gt;</span>
  <span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre><p><span class="token keyword">const</span> <span class="token function-variable function">mapStateToProps</span> <span class="token operator">=</span> state <span class="token operator">=&gt;</span> <span class="token punctuation">{</span><br>
console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">“mapping store.state to props”</span><span class="token punctuation">)</span><span class="token punctuation">;</span><br>
<span class="token keyword">return</span> <span class="token punctuation">{</span> onOff<span class="token punctuation">:</span> <span class="token function">getOnOff</span><span class="token punctuation">(</span>state<span class="token punctuation">)</span> <span class="token punctuation">}</span><span class="token punctuation">;</span><br>
<span class="token punctuation">}</span><span class="token punctuation">;</span></p>
<p><span class="token keyword">export</span> <span class="token keyword">default</span> <span class="token function">connect</span><span class="token punctuation">(</span>mapStateToProps<span class="token punctuation">)</span><span class="token punctuation">(</span>Lamp<span class="token punctuation">)</span><span class="token punctuation">;</span><br>
</p>
<p>Code : <a href="https://codesandbox.io/s/redux-illustrated-redux-version-with-selector-f9sgw?file=/src/Lamp.js">The redux version with a selector</a></p>
<p>Like I said the selector is a simple javascript filter (not to be confused with the method which is probably why it was named selector).<br>
Nonetheless a simple function returning the targeted data.<br>
Which is then mapped to the props in the Lamp component.</p>
<blockquote>
<p>Notice that the data store.state.user is no longer recognized. It was not passed down to the props of Lamp.</p>
</blockquote>
<h2 id="a-few-last-words">A few last words</h2>
<p>As you can see it was a lot to add, so make sure it is worthwhile for your app. Think twice on the structure of your code, the architecture. What will be re-rendered on the change of the state? Is the data in those components expensive thus slowing down my app? Can I use memoization to speed things along?</p>
<p>As a rule of thumb, the bigger the app the more chance the need for Redux, but a developer is an engineer, I think it is better to understand the why and the when.<br>
So be smart in structuring your app.</p>
<p>I hope this article will have helped you in your future projects.<br>
Happy coding !</p>
<h2 id="links-to-code">Links to Code</h2>
<ul>
<li><a href="https://codesandbox.io/s/redux-illustrated-react-only-version-rgcrs?file=/src/Home.js">The react only version</a></li>
<li><a href="https://codesandbox.io/s/redux-illustrated-memo-version-oe18j?file=/src/Home.js">The react.memo version</a></li>
<li><a href="https://codesandbox.io/s/redux-illustrated-redux-version-l75ld?file=/src/Home.js">The redux version</a></li>
<li><a href="https://codesandbox.io/s/redux-illustrated-redux-version-with-selector-f9sgw?file=/src/Lamp.js">The redux version with a selector</a></li>
</ul>
<h2 id="follow-me">Follow me</h2>
<ul>
<li><a href="https://twitter.com/lossushi">twitter</a></li>
<li><a href="https://github.com/gitSushi">github</a></li>
<li><a href="https://codepen.io/gitsushi/">codepen</a></li>
</ul>

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY2NTQyOTAzOV19
-->