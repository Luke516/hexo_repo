title: 用JDBC連接mySQL和java
date: 2014-12-22 00:36:20
tags:
- java
category:
- java
---

期末的作業，要我們建立一個資料庫並用java來操作，這裡使用的是mySQL和JDBC
<!--more-->

###安裝mySQL server和JDBC###

假如你的電腦上還沒安裝mySQL，沒關係，安裝的步驟非常簡單。

先到他們的[官方網站](MySQL on Windows)，因為我的作業系統是Windows，所以下載MySQL on Windows裡的MySQL installer。

下載好之後開啟MySQL installer，裡面可以選擇要安裝哪些項目。現在就先安裝mySQL server和Connector/J(java conncetor)吧！

安裝的過程很簡單，假如沒特別需求的話就維持預設設定，安裝完成~


###建立database與使用者###

安裝好之後，電腦裡會出現mySQL的commend line，我們就用它來建立一個資料庫。
命令列指令如下：

建立資料庫：
CREATE DATABASE javabase DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;

建立Table：
CREATE TABLE User (name VARCHAR(20), passwd VARCHAR(20));
(這裡的name和passwd是Table中的欄位)

新增使用者：
CREATE USER 'java'@'localhost' IDENTIFIED BY 'd$7hF_r!9Y';
GRANT ALL ON javabase.* TO 'java'@'localhost' IDENTIFIED BY 'd$7hF_r!9Y';
(這裡的'd$7hF_r!9Y'就是密碼)

###用java來對資料庫進行操作###

接下來就是java的部分了，這邊我使用的是eclipse這個IDE。

要讓我們的java程式能和mySQL連接，需要先加入我們安裝的connector。

在開啟新專案時，我們可以選擇inport external jar，並到我們安裝JDBC的路徑底下，把jar檔加入我們的專案。

接下來就是程式碼的部分了！

####讀取driver####

```java
try {
    System.out.println("Loading driver...");
    Class.forName("com.mysql.jdbc.Driver");
    System.out.println("Driver loaded!");
} catch (ClassNotFoundException e) {
    throw new RuntimeException("Cannot find the driver in the classpath!", e);
}
```

####連接資料庫####

```java
String url = "jdbc:mysql://localhost:3306/javabase";
String username = "java";   	//範例，使用者ID
String password = "d$7hF_r!9Y"	//範例，使用者密碼
Connection con = null;
Statement stat = null; 	//執行,傳入之sql為完整字串 
ResultSet rs = null; 	//結果集 
PreparedStatement pst = null; 

try {
    System.out.println("Connecting database...");
    con = DriverManager.getConnection(url, username, password);
    System.out.println("Database connected!");
} catch (SQLException e) {
    throw new RuntimeException("Cannot connect the database!", e);
} finally {
    System.out.println("Closing the connection.");
    if (connection != null) try { connection.close(); } catch (SQLException ignore) {}
}
```

這裡簡單說明一下各個Class的用途：
Connection 代表和一個database的連結。
Statement 代表MySQL的statement，也就是我們要給mySQL的指令，我們會由Connection產生Statement。	
ResultSet 代表結果，通常會是MySQL中的一個Table，一個Statement通常會產生一個ResultSet。

在上面的程式碼，就是用我們使用者的帳號、密碼，和MySQL server來產生一個Connection，有了這個Connection之後，我們才能進行
接下來的動作~~

####建立table####

```java
try { 
	stat = con.createStatement(); 
	stat.executeUpdate("CREATE TABLE User (id  INTEGER, name VARCHAR(20), passwd  VARCHAR(20))"; ); 
	//這個字串就是傳給mySQL server的指令，這裡是建立一個名叫User的Table
} 
catch(SQLException e) { 
	System.out.println("CreateDB Exception :" + e.toString()); 
} 
```

####插入資料####

```java
public void insertTable( String name,String passwd) {	//輸入name和passwd的資訊
	try { 
		pst = con.prepareStatement("insert into User(id,name,passwd) select ifNULL(max(id),0)+1,?,? FROM User"); 
		//這裡的問號是等下要傳送的參數
		pst.setString(1, name); 	//傳送第一個參數(取代第一個問號)
		pst.setString(2, passwd); 	//傳送第二個參數(取代第二個問號)
		pst.executeUpdate(); 
	} 
	catch(SQLException e) { 
		System.out.println("InsertDB Exception :" + e.toString()); 
	} 
}

```

####輸出資料####

```java
try { 
	stat = con.createStatement(); 
	rs = stat.executeQuery("select * from User "); 
	System.out.println("ID\t\tName\t\tPASSWORD"); 
	while(rs.next()) { 
	    System.out.println(rs.getInt("id")+"\t\t"+ 
	        rs.getString("name")+"\t\t"+rs.getString("passwd")); 
	} 
} 
catch(SQLException e) { 
	System.out.println("DropDB Exception :" + e.toString()); 
} 
finally { 
	Close(); 
} 
```


####關閉連結####

```java
try { 
	if(rs!=null) { 
	    rs.close(); 
	    rs = null; 
	} 
	if(stat!=null) { 
	    stat.close(); 
	    stat = null; 
	} 
	if(pst!=null) { 
	    pst.close(); 
	    pst = null; 
	} 
} 
catch(SQLException e) { 
	System.out.println("Close Exception :" + e.toString()); 
}

```