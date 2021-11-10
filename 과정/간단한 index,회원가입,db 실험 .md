



# 메인 index화면에서 회원가입 성공- DB까지

## index.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset="UTF-8">
<title>첫번째 페이지</title>
</head>
<body>
 			Hello World
 			<form action="./userJoinAction.jsp" method="post">
 				<input type="text" name="userID">
 				<input type="password" name="userPassword">
 				<input type="submit" value="회원가입">
 			</form>
</body>
</html>
```

![초기 index 페이지](https://user-images.githubusercontent.com/86362202/140951384-25c7dc4a-b161-44ff-b6fb-2802f2197506.png)

## UerDAO

```java
package user;

import java.sql.Connection;

import java.sql.PreparedStatement;

import util.DatabaseUtil;

public class UserDAO {
	
	public int join(String userID, String userPassword) {
		String SQL = "INSERT INTO USER VALUES ( ?, ?)";
		try {
			Connection conn = DatabaseUtil.getConnection();
			PreparedStatement pstmt = conn.prepareStatement(SQL);
			pstmt.setString(1, userID);
			pstmt.setString(2, userPassword);
			return pstmt.executeUpdate();
		} catch (Exception e) {
			e.printStackTrace();
		}
		return -1; 
	}
}

```

## UserDTO

```java
package user;

public class UserDTO {
	String userID;
	String userPassword;
	
	public String getUserID() {
		return userID;
	}
	public void setUserID(String userID) {
		this.userID = userID;
	}
	public String getUserPassword() {
		return userPassword;
	}
	public void setUserPassword(String userPassword) {
		this.userPassword = userPassword;
	}
}

```

## DatabaseUtil ( DB 연결 )

```java
package util;

import java.sql.Connection;
import java.sql.DriverManager;

public class DatabaseUtil {
	
	public static Connection getConnection() {
		try {
			String dbURL = "jdbc:mysql://localhost:3306/TUTORIAL?serverTimezone=UTC";
			String dbID = "root";
			String dbPassword = "0707";
			Class.forName("com.mysql.jdbc.Driver");
			return DriverManager.getConnection(dbURL, dbID, dbPassword);
		} catch (Exception e) {
			e.printStackTrace();
		}
		return null;
	}
}

```

## userJoinAction.jsp (유저 회원가입)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
     <%@ page import="user.UserDTO" %>
    <%@ page import="user.UserDAO" %>
    <%@ page import="java.io.PrintWriter" %>
    <%
    		request.setCharacterEncoding("UTF-8");
    		String userID = null;
    		String userPassword = null;
    		if(request.getParameter("userID") != null) {
    			userID = (String) request.getParameter("userID");
    		}
    		if(request.getParameter("userPassword") != null) {
    			userPassword = (String) request.getParameter("userPassword");
    		}
    		if(userID == null || userPassword == null ) {
    			PrintWriter script = response.getWriter();
    			script.println("<script>");
    			script.println("alert('입력이 안된 사항이 있습니다.');");
    			script.println("history.back();");
    			script.println("</script>");
    			script.close();
    			return;
    		}
    		UserDAO userDAO = new UserDAO();
    		int result = userDAO.join(userID, userPassword);
    		if ( result == 1) {
    			PrintWriter script = response.getWriter();
    			script.println("<script>");
    			script.println("alert('회원 가입에 성공 했습니다.');");
    			script.println("location.href='index.jsp';");
    			script.println("</script>");
    			script.close();
    			return;
    		}
     %>
```



![회원가입한 아이디와 비밀번호들](https://user-images.githubusercontent.com/86362202/140951388-4887100d-baa7-4da3-ba64-602bfb7fb637.png)



![1](https://user-images.githubusercontent.com/86362202/140951379-f5b71c16-1ce2-45b9-9234-d3c81e7e33f5.png)



## 문제점

회원가입란에 입력하면 흰 화면만 떠서 해결 방법을 찾던 중 jdbc connection 버전을 다르게 설치해주니 잘 되었다. 내 2시간..
