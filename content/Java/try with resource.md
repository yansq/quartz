# try with resource

在 Java 1.7 版本中引入，用于简化资源关闭的操作。

原始的 try-catch 写法：
```java
public void normalTry() throws Exception {
    String sql = "This is a sql statement with parameter";
    Connection conn = null;
    PreparedStatement ps = null;
    ResultSet rs = null;
    try {
        // Any methodology to get connection.
        conn = getConnection();
        ps = conn.prepareStatement(sql);
        ps.setString(1, "This is parameter");
        rs = ps.executeQuery();
        while (rs.next()) {
            rs.getString(1);
        }
    } finally {
        try {
            rs.close();
            ps.close();
            conn.close();
        } catch (Exception e) {
            throw e;
        }
    }
}
```

使用 try-with-resource 写法：
```java
public void tryWithResource() throws Exception {
    String sql = "This is a sql statement";
    try (Connection conn = getConnection(); 
        PreparedStatement ps = conn.prepareStatement(sql)) {
        ps.setString(1, "This is parameter");
        try (ResultSet rs = ps.executeQuery(sql);) {
            while (rs.next()) {
                rs.getString(1);
            }
        }
    }
}
```

