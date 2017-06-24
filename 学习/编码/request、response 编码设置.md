1. request.setCharacterEncoding（）是设置从request中取得的值或从数据库中取出的值指定后可以通过getParameter()则直接获得正确的字符串，如果不指定，则默认使用iso8859-1编码。
值得注意的是在执行setCharacterEncoding()之前，不能执行任何getParameter()。而且，该指定只对POST方法有效，对GET方法无效。
分析原因，应该是在执行第一个getParameter()的时候，java将会按照编码分析所有的提交内容，而后续的getParameter()不再进行分析，所以setCharacterEncoding()无效。而对于GET方法提交表单是，提交的内容在URL中，一开始就已经按照编码分析提交内容，setCharacterEncoding()自然就无效。
get需在Tomcat的server.xml中的:
URIEncoding="GBK" />) 加入URIEncoding="GBK",解决get请求乱码问题

2. response.setContentType("text/xml;charset=GBK")是设置页面中为中文编码
前者是设置动态文字（参数，数据库），后者设置页面静态文字
response.setContentType指定 HTTP 响应的编码,同时指定了浏览器显示的编码. response.setCharacterEncoding设置HTTP 响应的编码,如果之前使用response.setContentType设置了编码格式,则使用response.setCharacterEncoding指定的编码格式覆盖之前的设置.
与response.setContentType相同的是,调用此方法,必须在getWriter执行之前或者response被提交之前
response.setContentType("text/html; charset=utf-8"); html
response.setContentType("text/plain; charset=utf-8"); 文本
response.setContentType("text/javascript; charset=utf-8"); json数据
response.setContentType("application/xml; charset=utf-8");  xml数据
