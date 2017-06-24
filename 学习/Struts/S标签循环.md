```xml
<s:iterator var="define" value="#request.defineList" status="status">
	<s:if test="#status.odd==true"><tr></s:if>
    <s:if test="#define.type=='select'">
    	<td><s:property value="#define.name"/></td><td><s:select name="attribute%{#define.property_index}" id="attribute%{#define.property_index}" list="#request.omap['dq']" listKey="o_value" listValue="o_key"></s:select></td>
    </s:if>
  <s:if test="#status.odd!=true"></tr></s:if>
</s:iterator>
````
