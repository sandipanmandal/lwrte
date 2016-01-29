# Introduction #
### So....it's "TOOLBARS" time :) ###

The idea of that RTE plugin for jQuery is - split core and toolbars functionality, but without any bloating core-api. So core can do only basic functions (read about it in API section).

Let's look for basic usage of plugin (turn any textarea with class 'rte' into WYSIWYG):
```
	$('.rte').rte({
		controls_rte: rte_toolbar,
		controls_html: html_toolbar
	});
```
As you can see there are 2 options: **controls\_rte** and **controls\_html**. Later you will know how to create it. Just remember, plugin has no any bult-in toolbars or controls by default. If you will use it without any options:
```
	$('.rte').rte();
```
you will get next:<br />
<img src='http://lwrte.googlecode.com/files/empty1.jpg'> and <img src='http://lwrte.googlecode.com/files/empty2.jpg'>

The only controls enable\disable as you see. It has built-in callback, but actually you can overwrite it (inherit, by other words. It's very useful for additional text-processing between switching of modes or before submit).<br>
<br>
Ok, now you are really ready to create own controls.<br>
<br>
<h1>Creating Own Controls</h1>
Controls is javascript object actually, that has specific format. Format is simple, and probably suit for any task that you will want. Controls is the thing that you provide at startup/initialize step as 'controls_rte' and 'controls_html' inside of options. Look for example above.<br>
<br>
<h2>Format of toolbar</h2>
<pre>
{<br>
*control_name1* : {...},<br>
*control_name2* : {...},<br>
*control_nameN* : {...}<br>
}<br>
</pre>

<h2>Format of control</h2>
<pre>
*control_name*: {<br>
*command:* string<br>
*separator:* boolean<br>
*init:* function<br>
*exec:* function<br>
*args:* string or array<br>
*select:* string<br>
*tag_cmp:* function<br>
*hint*: string<br>
*tags:* array<br>
}<br>
</pre>

Where <b>contol_name</b> is name of your control. It's used as className for CSS-styling. This must be unique for each toolbar. Shortly: only one control with 'control_name' per toolbar.<br>
<br>
<h3>Example 1: separator</h3>
<pre><code>	s1: {separator: true}<br>
</code></pre>
<h3>Example 2: basic control</h3>
<pre><code>	bold: {command: 'bold', tags:['b', 'strong']}<br>
</code></pre>
<h3>Example 3: advanced control</h3>
<pre><code>	bold: {<br>
		exec: function() {<br>
			var self = this;<br>
			alert("Control clicked!");<br>
			self.editor_cmd('bold');<br>
		},<br>
		hint: "New version of bold",<br>
		tags: ['b', 'strong']<br>
	}<br>
</code></pre>

Let's talk about each thing.<br>
<br>
<h2>command (string)</h2>
It's command for browser's design mode editor. Some commands need additional arguments (for example, name of font), some don't. It's browser specific feature.<br>
<br>
<h3>Example:</h3>
<pre><code>command: 'formatBlock'<br>
</code></pre>

You can read more about that commands here:<br>
<blockquote><a href='http://msdn.microsoft.com/en-us/library/ms533049(VS.85).aspx'>http://msdn.microsoft.com/en-us/library/ms533049(VS.85).aspx</a><br>
<a href='http://www.mozilla.org/editor/midas-spec.html'>http://www.mozilla.org/editor/midas-spec.html</a><br></blockquote>

<h2>separator (boolean)</h2>
Just set it true, if you want to add new separator inside of your toolbar. Other parameters are omitted.<br>
<h3>Example:</h3>
<pre><code>separator: true<br>
</code></pre>

<h2>init( function(rte) )</h2>
Well, this is function that will be called before adding that control into the toolbar. So additional manipulations can be done. For example, dependence on 'args' of that control. Remember 'this' reference at that function is control, but not rte!<br>
<h3>Example 1: Init</h3>
<pre><code>init: function(rte) {<br>
	alert("Control setup");<br>
	this.exec = function() { alert('on fly callback.') };<br>
}<br>
</code></pre>

<h3>Example 2: Init and args</h3>
<pre><code>init: function(rte) {<br>
	if(this.args == 'arg1') {<br>
		this.exec = function() { alert('exec1.') };<br>
	} else {<br>
		this.exec = function() { alert('exec2.') };<br>
	}<br>
<br>
	alert(this.exec);<br>
}<br>
</code></pre>

<h2>exec ( function(args) )</h2>
Well, this is a heart of you own specific control. Here you must place any functionality that you want from that control. To get instance of lwRTE object (to use core functions), you can use 'this' reference. If you provided 'args' for that control, than you can get it as argument 'args'. If contol is select, than last argument will be 'select' object, so you will be able to get index of selected item.<br>
<br>
<h3>Example 1: Hello world</h3>
<pre><code>exec: function(args) {<br>
	alert("Hello world from that control");<br>
}<br>
</code></pre>
<h3>Example 2: Get selected text</h3>
<pre><code>exec: function(args) {<br>
	var self = this;<br>
	alert("You have selected:" + self.get_selected_text());<br>
}<br>
</code></pre>
<h3>Example 3: Replace something with something</h3>
<pre><code>exec: function(args) {<br>
	var self = this;<br>
	var text = self.get_content();<br>
	text = text.replace( /&lt;BR&gt;/gi, '[BR]' );<br>
	self.set_content(text);<br>
}<br>
</code></pre>
<h3>Example 4: Execute command for browser's design mode editor</h3>
<pre><code>exec: function(args) {<br>
	var self = this;<br>
	self.editor_cmd('insertImage', 'uploads/screenshot.jpg');<br>
}<br>
</code></pre>

<h3>Example 5: callback for select control</h3>
<pre><code>exec: function(args) {<br>
	if(args) {<br>
		try {<br>
			var css = args.options[args.selectedIndex].value<br>
			var self = this;<br>
			var html = self.get_selected_text();<br>
			html = '&lt;span class="' + css + '"&gt;' + html + '&lt;/span&gt;';<br>
			self.selection_replace_with(html);<br>
			args.selectedIndex = 0;<br>
		} catch(e) {<br>
		}<br>
	}<br>
}<br>
</code></pre>

Just look at 'jquery.rte.tb' inside of full package and you will see more complex, real functionality like file uploads, color selections and other.<br>
<br>
<h2>args (string or array)</h2>
Just additional arguments for 'command'. Read more about it at 'command' section (Check URLs for specification)<br>
<br>
<h2>select (string)</h2>
Sometimes you need not a common control, but a selection from few values, like font selection, for example. In that case you can provide a html-code for that selection here.<br>
<h3>Example:</h3>
<pre><code>select: "<br>
&lt;select&gt;\<br>
	&lt;option value=""&gt;-&lt;/option&gt;\<br>
	&lt;option value="1"&gt;1 (8pt)&lt;/option&gt;\<br>
	&lt;option value="2"&gt;2 (10pt)&lt;/option&gt;\<br>
	&lt;option value="3"&gt;3 (12pt)&lt;/options&gt;\<br>
	&lt;option value="4"&gt;4 (14pt)&lt;/option&gt;\<br>
	&lt;option value="5"&gt;5 (16pt)&lt;/options&gt;\<br>
	&lt;option value="6"&gt;6 (18pt)&lt;/option&gt;\<br>
	&lt;option value="7"&gt;7 (20pt)&lt;/options&gt;\<br>
&lt;/select&gt;\<br>
"<br>
</code></pre>

<h2>tag_cmp ( function(node, tag) )</h2>
This is callback function to compare DOM node and tag from <b>tags</b> parameter. It's using by core (<b>set_selected_controls</b>) to mark active controls on toolbar. Read more about it in API section. Shortly: <b>node</b> - is DOM object, <b>tag</b> - is value from <b>tags</b> array (read more below).<br>
<br>
<h3>Example:</h3>
<pre><code>tag_cmp:<br>
	function(node, tag) {<br>
		tag = tag.replace(/&lt;([^&gt;]*)&gt;/, '$1');<br>
		return (tag.toLowerCase() == node.nodeName.toLowerCase());<br>
	}<br>
</code></pre>

<h2>hint (string)</h2>
You can add extended hint for control that will be shown (core will set 'title' for common controls).<br>
<br>
<h3>Example:</h3>
<pre><code>hint: 'Make you text bold'<br>
</code></pre>

<h2>tags (array)</h2>
This array is using by core (<b>set_selected_controls</b>) to mark active controls. Using with<br>
<b>tag_cmp</b> callback and core(<b>set_selected_controls</b>).<br>
<br>
<h3>Example:</h3>
<pre><code>tags: ['b', 'strong']<br>
</code></pre>

<h1>How to extend enable/disable control?</h1>
Ok, for example, you need some additional text processing  before submit and want to implement it easily? Not a problem. Just add any parameter you want (from documentation above) for <b>enabled</b> and <b>disable</b> controls. <b>enable</b> is using on source mode toolbar and <b>disable</b> is using on design mode toolbar. Don't care about turning on/off design mode, core will do rest things.<br>
<br>
<h3>Example:</h3>
<pre><code>var rte_toolbar = {<br>
	disable: {<br>
		exec: function() { alert("Press OK to turn OFF design mode!"); }<br>
	}<br>
};<br>
<br>
var html_toolbar = {<br>
	enable: {<br>
		exec: function() { alert("Press OK to turn ON design mode!"); }<br>
	}<br>
};<br>
</code></pre>
<br>
<br>
<hr><br>
<br>
<br>
<b>Check source of basic toolbar to get more examples of controls</b>

