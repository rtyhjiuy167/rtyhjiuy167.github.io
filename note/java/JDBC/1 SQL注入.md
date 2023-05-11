```sql
Create table users(
	userid bigint PRIMARY KEY,
	username varchar(20),
	password varchar(20)
);

insert into users values(1,123456,654321);
```

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class SQLInject {
    public static void main(String[] args) throws Exception {
        Connect();
    }

    public static Connection Connect() throws Exception {

        String url = "jdbc:postgresql://127.0.0.1:5432/test"; // jdbc:连接哪种数据库://url:端口//数据库名
        String username = "jdbc_user"; // 数据库用户名用户名
        String password = "123456"; // 用户密码
        Connection conn = DriverManager.getConnection(url, username, password);

        String name = "haha";
        String pwd= "' or '1' = '1";

        String sql = "select * from users where username = '"+name+"' and password = '"+pwd+"'";
        Statement stmt = conn.createStatement();

        ResultSet rs = stmt.executeQuery(sql);

        if(rs.next()){
            System.out.println("登录成功");
        }

        // 关闭连接
        rs.close();
        stmt.close();
        conn.close();
        return conn;
    }
}
```

执行的SQL语句：select * from users where username = 'haha' and password = '' or '1' = '1' ;

可使用PreparedStatement来防止SQL注入：

```java
import java.sql.*;

public class SQLInject {
    public static void main(String[] args) throws Exception {
        Connect();
    }

    public static Connection Connect() throws Exception {

        String url = "jdbc:postgresql://127.0.0.1:5432/test?userServerPrepStmts=true"; // userServerPrepStmts=true开启预编译
        String username = "jdbc_user"; // 数据库用户名用户名
        String password = "123456"; // 用户密码
        Connection conn = DriverManager.getConnection(url, username, password);

        String name = "haha";
        String pwd= "' or '1' = '1";

        String sql = "select * from users where username = ? and password = ?";

        //获取pstmt对象
        PreparedStatement pstmt =conn.prepareStatement(sql);

        // 设置 ？ 的值
        // 第一参数表示第几个 第二参数 内容
        pstmt.setString(1,name);
        pstmt.setString(2,pwd);

        // 通过 pstmt 来执行语句 此时不需要传入 sql 了
        ResultSet rs = pstmt.executeQuery();

        if(rs.next()){
            System.out.println("登录成功");
        }else{
            System.out.println("登录失败");
        }

        // 关闭连接
        rs.close();
        pstmt.close();
        conn.close();
        return conn;
    }
}
```

使用PreparedStatement会对sql中的特殊字符进行转义。使用userServerPrepStmts=true能进行预编译，能提高性能。