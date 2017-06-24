### 类PermissionTag
```java
public class PermissionTag extends RequestContextAwareTag {
	// 用户标记
	private String userId;
	// 动作标记
	private String actId;
	// 是否隐藏
	private String markId;
	public String getUserId() {
		return userId;
	}
	public void setUserId(String userId) {
		this.userId = userId;
	}
	public String getActId() {
		return actId;
	}
	public void setActId(String actId) {
		this.actId = actId;
	}
	public String getMarkId() {
		return markId;
	}
	public void setMarkId(String markId) {
		this.markId = markId;
	}
	@Override
	public int doStartTagInternal() throws JspException {
		boolean result = false;
		PermissionService permissionService = (PermissionService) this.getRequestContext().getWebApplicationContext().getBean("permissionService");
		SysPermission permission = permissionService.get(1550L);
		try {
			JspWriter out = pageContext.getOut();
			out.write("taglib end time: " + new Date());
		} catch (IOException e) {
		}
		return result ? EVAL_BODY_INCLUDE : SKIP_BODY;// 真：返回EVAL_BODY_INCLUDE（执行标签）；假：返回SKIP_BODY（跳过标签不执行）
	}
}
```

### 约束文件tld： permissionTag.tld
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<taglib xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-jsptaglibrary_2_0.xsd"
	version="2.0">
	<description>permission taglib</description><!-- 描述 -->
	<display-name>permission taglib</display-name>
	<tlib-version>1.0</tlib-version><!-- 版号 -->
	<short-name>permisTag</short-name> <!-- 简单名称 -->
	<uri>http://www.permission.com/</uri> <!-- 引用路径 -->
	<tag>
		<name>permis</name>
		<tag-class>com.dragon.rbac.taglib.PermissionTag</tag-class>
		<body-content>JSP</body-content>
		<attribute>
			<name>userId</name>
			<required>false</required>
			<rtexprvalue>true</rtexprvalue>
		</attribute>
		<attribute>
			<name>actId</name>
			<required>false</required>
			<rtexprvalue>true</rtexprvalue>
		</attribute>
		<attribute>
			<name>markId</name>
			<required>false</required>
			<rtexprvalue>true</rtexprvalue>
		</attribute>
	</tag>
</taglib>
```
### JSP里用法
```jsp
<%@ taglib prefix="permis" uri="http://www.permission.com/"%>
<permis:permis/>
```
