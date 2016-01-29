<h2>Lightweight Rich Text Editor (RTE / WYSIWYG) for jQuery</h2>
<img src='http://lwrte.googlecode.com/files/screenshot.jpg'>
<br>
<p>Do you need <b>really lightweight and unbloated Rich Text Editor (RTE / WYSIWYG) for JQuery</b> and ability to extend it as you want? <b>So you at right place.</b></p>
<p>Why yet another "lightweight" RTE editor? The major reason - i couldn't find RTE editor:<br>
<ol>
<blockquote><li>without any built-in toolbars (you can turn off controls only, but this's other case), </li>
<li>really jQuery plugin, but not wrapper (I'm using jQuery in my projects, that's why best choice - real jQuery plugin),</li>
<li>easy to extend (for example - file uploads).</li>
<li><b>different toolbars</b> for design mode and source mode</li>
</ol></blockquote>

I tried to make this Rich Text Editor as simple as possible to be easily changed according your needs. <br />
The file "jquery.rte.js" is <b>only 10kb uncompressed</b>. This has all that your editor probably need, but has no any built-in toolbars - you can extend it easily as you want. <br />
<br />
I also provided basic toolbar (<b>actually it has almost same features as other WYSIWYG editors</b>), <b><u>include Image/File uploads</u></b> (you will have to configure your server script for uploads or update "uploader.php" from example).<br>
<br /><br />
This editor is compatible with major browsers (IE6, Firefox 2, Opera 9, Safari 3.03) and degrade gracefully in a textarea for others.</p>
<p>P.S.: Tested with jQuery 1.2.6 and 1.3</p>
<br>
<br>
<hr><br>
<br>
<br>
<b>Usage:</b>
<pre><code><br>
$('.rte').rte({<br>
css: [['default.css']],<br>
controls_rte: rte_toolbar,<br>
controls_html: html_toolbar<br>
});<br>
</code></pre>
OR<br>
<pre><code><br>
$('.rte').rte({<br>
css: [['default.css']],<br>
base_url: 'http://mysite.com',<br>
frame_class: 'frameBody',<br>
width: 350,<br>
height: 400,<br>
controls_rte: rte_toolbar,<br>
controls_html: html_toolbar<br>
});<br>
</code></pre>
<b>Plugin has no any built-in toolbars, so you have to provide it. Lucky, I created one (with almost any feature that you probably will need, include image uploads and file attachments), so you can use it :)</b>
<br><br>
Plugin has only RTE/WYSIWYG editor's API, but no any controls/toobars. In that way you can use own specific version of editor for projects.<br>
<br><br>
Check Wiki for documentation and API.