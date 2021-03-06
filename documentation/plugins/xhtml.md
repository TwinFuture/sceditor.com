---
layout: doc
title: XHTML - Plugin
permalink: /documentation/plugins/xhtml/
categories:
    - Docs
---

## XHTML Plugin <a id="xhtml"></a>

This plugin will convert the output of SCEditor into valid XHTML.

<div class="alert alert-block alert-error">
	<h4>Warning!</h4>
	<p>Never trust the output from this plugin. Like all client side code the output can be altered or forged to produce XSS attacks.</p>
	<p>Always make sure to filter the XHTML, as well as the data from all other user inputs, server side through a library like <a href="http://htmlpurifier.org/">htmlpurifier (PHP)</a> or  <a href="https://www.owasp.org/index.php/OWASP_Java_HTML_Sanitizer">OWASP Java HTML Sanitizer</a> to prevent XSS attacks.</p>
</div>


### Initialise <a id="initialise"></a>

To enable the XHTML plugin just add `xhtml` to the `plugins` option. e.g.

<pre class="prettyprint linenums">
&lt;script&gt;
$(function() {
	$("textarea").sceditor({
		plugins: "xhtml",
		style: "minified/jquery.sceditor.default.min.css"
	});
});
&lt;/script&gt;
</pre>


### Options <a id="options"></a>

All of the options below should be set before creating an instance of the editor. If they are set after they will not take effect until source mode is toggled.


### converters <a id="converters"></a>

`$.sceditor.plugins.xhtml.converters` *Array*

An `array of` converter `objects` used to convert HTML into valid XHTML or HTML5.

A converter object must be in the following format:

<pre class="prettyprint linenums">
{
	// Object with all the tags to match
	tags: {
		// Matches any &lt;tagname&gt; that has the following attributes
		"tagname": {
			// Will match if tagname has the attribute attributename with
			// one of the specified values from the array
			attributename: [
				'an array', 'of values or', 'null to match', 'all values'
			],

			// Will match if it has the attribute anotherattribute regardless
			// of the attributes value
			anotherattribute: null
		},

		// Matches any &lt;anothertagname&gt; tag
		'anothertagname': null
		
	},
	conv: function(node) {
		// convert node here
	}
}
</pre>

To match any tag use `*` for the name.

For example:

<pre class="prettyprint linenums">
// Push this converter onto the end of the converters array
$.sceditor.plugins.xhtml.converters.push({
	tags: {
		// Match any tag with the attribute border
		"*": {
			border: null
		}
		
	},
	conv: function(node) {
		var $node = $(node);

		// Change border attribute to CSS
		$node.css('border-size', $node.attr('border')).removeAttr('border');
	}
});
</pre>

The above will match any tag with the attibute `border` and convert the `border` attribute into `border-size` CSS.


### allowedAttribs <a id="allowedAttribs"></a>

`$.sceditor.plugins.xhtml.allowedAttribs` *Object*

Acts as a whitelist of allowed attributes. If this is null or empty then all attributes will be allowed unless blacklisted.

The allowed attributes can be specified on a tag by tag basis or for all tags by using `*` as the tag name.

`allowedAttribs` should be in the following format format:

<pre class="prettyprint linenums">
$.sceditor.plugins.xhtml.disallowedAttribs['tagname']['attributename'] = [
	'array', 'of', 'valid', 'values', 'or', 'null', 'to', 'allow', 'all', 'values'
];
</pre>

For example:

<pre class="prettyprint linenums">
$.sceditor.plugins.xhtml.disallowedAttribs = {
	// The * sign matches every tags so this will allow the id
	// and class attributes on all tags
	'*': {
		id: null,
		'class': null
	},
	// This will allow the href attribute on the a tag
	a: {
		href: null
	},
	// This will allow the face attribute on the font tag
	// with a value of 'Arial' or 'Times New Roman'
	font: {
		face: ['Arial', 'Times New Roman']
	}
};
</pre>

Acts as a blacklist of disallowed tags. If null or empty then all tags will be allowed.


### disallowedAttribs <a id="disallowedAttribs"></a>

`$.sceditor.plugins.xhtml.disallowedAttribs` *Object*

<span class="label label-important">Important:</span> This will only be used if allowedAttribs is empty.

Acts as a blacklist of disallowed attributes. If null or empty then all attributes will be allowed.

The format of `disallowedAttribs` is the same as `allowedAttribs` above except instead of allowing attributes, this will blacklist them.


### allowedTags <a id="allowedTags"></a>

`$.sceditor.plugins.xhtml.allowedTags` *Array* 

Acts as a whitelist of allowed tags. If null or empty then all tags will be allowed unless blacklisted.

<pre class="prettyprint linenums">
$.sceditor.plugins.xhtml.allowedTags = ['a', 'strong', 'p', 'span', 'div'];
</pre>


### disallowedTags <a id="disallowedTags"></a>

`$.sceditor.plugins.xhtml.disallowedTags` *Array*

<span class="label label-important">Important:</span> This will only be used if allowedTags is empty.

Acts as a blacklist of disallowed tags. If null or empty then all tags will be allowed.

<pre class="prettyprint linenums">
$.sceditor.plugins.xhtml.disallowedTags = ['a', 'strong', 'p', 'span', 'div'];
</pre>

