Runs a script in Blackboard's VTBE a few seconds after loading the page in order to remove superflous <code>span</code> tags. Requires the [JSHack](https://jshack.net) Building Block to work, but the snippet should be usable elsewhere.

## Methodology

1) Find all VTBE iframes.
```javascript
var iframes = document.querySelectorAll('.mceIframeContainer iframe');        
```
2) For each one, run the <code>fixSpan</code> method.
```javascript
for (var i = 0; i < iframes.length; i++) {
    fixSpan(iframes[i]);
}
```
3) In the <code>fixSpan</code> method, find all <code>span</code> elements.
```javascript
var doc = iframe.contentDocument;
var spans = doc.getElementsByTagName('span');
```
4) For each one, if no attributes exist, replace the element with a new <code>textNode</code> containing only the element's <code>textContent</code>.
```javascript
for (var i = 0; i < spans.length; i++) {
    var span = spans[i];            
    var tag = span.outerHTML.substring(0,6).trim();  
    if (tag === '<span>') {
        console.log('FOUND: <span>' + span.textContent + '</span>');
        span.parentNode.replaceChild(document.createTextNode(span.textContent), span);
    }
}
```
## Why it works
Any extra <code>span</code> elements are **always** the direct parent of the <code>textNode</code> containing the "misspelled" word (or of another extra <code>span</code> element. Any formatting or inline styling would occur outside that scope and would thus be unaffected.
