```js
window._img = new Object();
function antiDaolian()
{
    imgs = $("img");
    $.each(imgs,function(i,img){
	    window._img = '<img id=\'img\' src=\''+img.src+'?'+Math.random()+'\' />';
	    ifr = document.createElement('iframe');
	    ifr.src = 'javascript:parent._img;';
	    ifr.frameBorder="0";
	    ifr.scrolling="no";
	    ifr.width="100%";
	    ifr.height="100%";
    	img.parentNode.replaceChild(ifr, img);
    });
}
$(function(){
	antiDaolian();
})
```
