# 分布式商城项目

## 1. 模块之间的继承与依赖关系

![模块之间的继承与依赖关系](http://i4.buimg.com/567571/de0956b9fcc14664.jpg) 

## 2. MyBatis 逆向工程

所谓**逆向工程**就是根据数据库表来生成model、mapper代码。

1. 建一个maven项目
2. 在`pom.xml`中添加插件：
3. 新建`generatorConfig.xml`配置文件
4. 执行命令：`mybatis-generator:generate`

**generatorConfig.xml**
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">


<generatorConfiguration>
    <!--数据库驱动jar -->
    <classPathEntry location="xxx/mysql-connector-java-5.1.39.jar" />

    <context id="DB2Tables" targetRuntime="MyBatis3">
        <!--去除注释  -->
        <commentGenerator>
            <property name="suppressAllComments" value="true" />
        </commentGenerator>

        <!--数据库连接 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/mall"
                        userId="root"
                        password="root">
        </jdbcConnection>
        <!--默认false
           Java type resolver will always use java.math.BigDecimal if the database column is of type DECIMAL or NUMERIC.
         -->
        <javaTypeResolver >
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>

        <!--生成实体类 指定包名 以及生成的地址 （可以自定义地址，但是路径不存在不会自动创建  使用Maven生成在target目录下，会自动创建） -->
        <javaModelGenerator targetPackage="com.derker.mall.model" targetProject="MAVEN">
            <property name="enableSubPackages" value="false" />
            <property name="trimStrings" value="true" />
        </javaModelGenerator>
        <!--生成SQLMAP文件 -->
        <sqlMapGenerator targetPackage="com.derker.mall.mapper"  targetProject="MAVEN">
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>
        <!--生成Dao文件 可以配置 type="XMLMAPPER"生成xml的dao实现  context id="DB2Tables" 修改targetRuntime="MyBatis3"  -->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.derker.mall.mapper"  targetProject="MAVEN">
            <property name="enableSubPackages" value="false" />
        </javaClientGenerator>

        <!-- 指定数据库表 -->
        <table schema="" tableName="tb_content"></table>
        <table schema="" tableName="tb_content_category"></table>
        <table schema="" tableName="tb_item"></table>
        <table schema="" tableName="tb_item_cat"></table>
        <table schema="" tableName="tb_item_desc"></table>
        <table schema="" tableName="tb_item_param"></table>
        <table schema="" tableName="tb_item_param_item"></table>
        <table schema="" tableName="tb_order"></table>
        <table schema="" tableName="tb_order_item"></table>
        <table schema="" tableName="tb_order_shipping"></table>
        <table schema="" tableName="tb_user"></table>

    </context>
</generatorConfiguration>
```

**pom.xml**
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.test</groupId>
    <artifactId>myBatis-generator</artifactId>
    <version>1.0-SNAPSHOT</version>

    <build>
        <plugins>
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.3</version>
            </plugin>
        </plugins>
    </build>

</project>
```

## 3. SMM框架整合

