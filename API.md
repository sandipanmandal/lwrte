# Introduction #

So you decided to create own specific toolbar for your project? Let's do it, but first of all, you will have to study a core api a bit. It's unbloated and will not require a lot of your time.


# Details #
First of all, let's try to understand idiology a bit. lwRTE has only text-data in time(depends on current mode). Most popular editors has both (formatted text and html-source) and trying to sync it time from time, but in our case - the only text in memory. When you initialize rich text editor for your textarea, new iframe created with text from that textarea. After that, textarea removed from memory and DOM. When you switch into source mode, then new textarea created with text from iframe. After that, iframe removed from memory and DOM.

**There is the only editor (iframe or textarea) in time, with only copy of your text!**

Just remember - for source mode we have a textarea as editor, for design mode (rich text editor) we have iframe as editor.

  * source mode - textarea as editor (to get DOM instance: **this.textarea**)
  * design mode - iframe as editor (to get DOM instance: **this.iframe**)


## Usage ##
```
$(document).ready(function() {
	var arr = $('.rte1').rte({
		css: ['default.css'],
		controls_rte: rte_toolbar,
		controls_html: html_toolbar
	});

	$('.rte2').rte({
		css: ['default.css', 'styles.css'],
		width: 450,
		height: 200,
		controls_rte: rte_toolbar,
		controls_html: html_toolbar
	}, arr);

	//arr is array of RTEm you can use api as you want.

});
```

As you can see, you can use array that returned as you want. Both methods to access is possinble - by index (for anonimous textareas) or by ID.
Ex:
**`arr[0].get_content()`** or **`arr['txt1'].get_content()`**


## Functions ##

### get\_content() ###
Use it if you want **to get content** of current editor. It's the only right function to get content. Works fine for both mode - design and source.

### set\_content(content) ###
Use it if you want **to set content** of current editor. It's the only right function to set content. Works fine for both mode - design and source.

### get\_selected\_text() ###
Use it if you want to get selected text without html tags. **Works for design mode only.**

### get\_selected\_html() ###
Use it if you want to get selected text with html tags. **Works for design mode only.**

### selection\_replace\_with(html) ###
Use it to replace selection with 'html'. **Works for design mode only.**

### editor\_cmd(command, args) ###
Executes built-in 'command' with 'args' for editor in design mode. 'command' is browser specific command for design mode. **Works for design mode only.**

More about that commands here:<br>
<blockquote><a href='http://msdn.microsoft.com/en-us/library/ms533049(VS.85).aspx'>http://msdn.microsoft.com/en-us/library/ms533049(VS.85).aspx</a><br>
<a href='http://www.mozilla.org/editor/midas-spec.html'>http://www.mozilla.org/editor/midas-spec.html</a><br></blockquote>

<h3>enable_design_mode()</h3>
<b>Turn on Design Mode</b>. Design Mode - mode where you can execute special commands to make changes into DOM. Use <b>editor_cmd</b> function for that. This function does: creates new iframe, copies text from textarea, removes textarea from DOM, turn on design mode for iframe.<br>
<br>
<h3>disable_design_mode(submit)</h3>
<b>Turn off Design Mode</b>. Actually this function does:<br>
<br>
<ul><li>if 'submit' is false, then creates new textarea, copies text from iframe, removes iframe from DOM.<br>
</li><li>if 'submit' is true, then creates new 'hidden' input, copies text from iframe and submit data.</li></ul>

<h3>create_toolbar(controls)</h3>
This function creates toolbar from provided controls(creates, without insertion into the DOM). You can use it later with function <b>activate_toolbar</b>. Read more about controls at "Toolbars" section.<br>
<br>
<h3>create_panel(title, width)</h3>
If you want to create additional panel(window) for toolbar control (I.e. when user clicks on control), then it's right function. It creates invisible additional panel and inserting it into the DOM. You need to make it visible manually (you can use 'show', 'fadeIn' and other jQuery effect funсtions). You can set specific 'title' and 'width' of that panel.<br>
<br>
<h3>get_toolbar()</h3>
That function returns jQuery object with current active toolbar ('.rte-toolbar') for editor. Remember, we have different toolbars for source mode and design mode!<br>
<br>
<h3>activate_toolbar(editor, tb)</h3>
That function activates toolbar 'tb' for editor. Actually it does next: remove current toolbar from DOM for 'editor' and insert 'tb' into DOM.<br>
<br>
<h3>toolbar_click(obj, control)</h3>
This is callback function for any toolbar control. Toolbar consists of number of controls (buttons and etc). Read more about toolbars in documentation for it.<br>
This callback does next:<br>
<ul><li>If 'control' has own callback(<b>exec</b> parameter of control), than it executes.<br>
</li><li>If 'control' hasn't callback, but have 'command' and current mode is design mode, then it executes that 'command'. 'obj' is DOM object that was clicked/changed. You can read more about control's callback at documentation for Toolbars, section <b>exec</b>.</li></ul>

<h3>get_selection_range()</h3>
This function returns range object (browser specific) for current selection. <b>Works for design mode only.</b>

<h3>get_selected_element()</h3>
This functions locate DOM object at mouse cursor. You can use that node for <b>set_selected_controls</b> function. <b>Works for design mode only.</b>

<h3>set_selected_controls(node, controls)</h3>
This function marks toolbar's controls (works with 'controls' that you provided) as active for specific node (node is DOM object where mouse cursor is placed right now). <b>Works for design mode only.</b>

