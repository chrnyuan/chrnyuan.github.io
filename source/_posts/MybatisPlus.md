---
title: MybatisPlus
date: 2019-08-21 15:59:25
tags: [MybatisPlus]
---
# Mybatis-Plus 代码生成器

# 1:什么是Mybatis-Plus？
### MybatisPlus是一个开源的ORM框架，是对mybatis框架的增强，宣传只做增强不做修改，强大的CURD,无侵入，内置代码生成，sql性能解析,内置分页。
# 2：使用Mybatis-Plus的好处？

### 开发速度大大加快，使用Mybatis-Plus生成器可以生成 mapper、xml、service 、serviceImpl、controller,还有强大的条件构造器，单表查询nosql。

<!--more-->
## 开始配置Mybatis-PLus代码生成器
## 创建一个springboot项目在pom文件中添加依赖
```
		<!--Mysql 连接驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
		<!-- mybatisPlus 代码生成 -->
		<dependency>
    		<groupId>com.baomidou</groupId>
    		<artifactId>mybatis-plus-generator</artifactId>
    		<version>3.1.2</version>
		</dependency>
		<!-- 使用freemarer模板引擎 -->
		<dependency>
    		<groupId>org.freemarker</groupId>
    		<artifactId>freemarker</artifactId>
    		<version>2.3.28</version>
		</dependency>


```
## 创建一个类CodeGenerator，直接执行main方法就可以，代码如下
```
public class CodeGenerator {

	 /**
	  * <p>
	  * 读取控制台内容
	  * </p>
	  */
	 public static String scanner(String tip) {
	     Scanner scanner = new Scanner(System.in);
	     StringBuilder help = new StringBuilder();
	     help.append("请输入" + tip + "：");
	     System.out.println(help.toString());
	     if (scanner.hasNext()) {
	         String ipt = scanner.next();
	         if (StringUtils.isNotEmpty(ipt)) {
	             return ipt;
	         }
	     }
	     throw new MybatisPlusException("请输入正确的" + tip + "！");
	 }

	 public static void main(String[] args) {
	     // 代码生成器
	     AutoGenerator mpg = new AutoGenerator();
	     
	     /**
	      * 使用了非默认模板引擎
	      */
	     // set freemarker engine
	     mpg.setTemplateEngine(new FreemarkerTemplateEngine());
	     
	     // 全局配置
	     GlobalConfig gc = new GlobalConfig();
	     final String projectPath = System.getProperty("user.dir");
	     gc.setOutputDir(projectPath + "/src/main/java");
	     gc.setAuthor("zyk");//作者
	     gc.setOpen(false);
	     // gc.setSwagger2(true); 实体属性 Swagger2 注解
	     mpg.setGlobalConfig(gc);

	     // 数据源配置
	     DataSourceConfig dsc = new DataSourceConfig();
	     dsc.setUrl("jdbc:mysql://localhost:3306/database?useUnicode=true&useSSL=false&characterEncoding=utf8&serverTimezone=UTC");
	     // dsc.setSchemaName("public");
	     dsc.setDriverName("com.mysql.cj.jdbc.Driver");
	     dsc.setUsername("root");
	     dsc.setPassword("密码");
	     mpg.setDataSource(dsc);

	     // 包配置
	     final PackageConfig pc = new PackageConfig();
	     pc.setModuleName(scanner("模块名"));//模块名
	     pc.setParent("com.xxx.demo");//项目名
	     mpg.setPackageInfo(pc);

	     // 自定义配置
	     InjectionConfig cfg = new InjectionConfig() {
	         @Override
	         public void initMap() {
	             // to do nothing
	         }
	     };

	     // 如果模板引擎是 freemarker
	     String templatePath = "/templates/mapper.xml.ftl";
	     // 如果模板引擎是 velocity
	     // String templatePath = "/templates/mapper.xml.vm";

	     // 自定义输出配置
	     List<FileOutConfig> focList = new ArrayList<FileOutConfig>();
	     // 自定义配置会被优先输出
	     focList.add(new FileOutConfig(templatePath) {
	         @Override
	         public String outputFile(TableInfo tableInfo) {
	             // 自定义输出文件名 ， 如果你 Entity 设置了前后缀、此处注意 xml 的名称会跟着发生变化！！
	             return projectPath + "/src/main/resources/mapper/" + pc.getModuleName()
	                     + "/" + tableInfo.getEntityName() + "Mapper" + StringPool.DOT_XML;
	         }
	     });
	     /*
	     cfg.setFileCreate(new IFileCreate() {
	         @Override
	         public boolean isCreate(ConfigBuilder configBuilder, FileType fileType, String filePath) {
	             // 判断自定义文件夹是否需要创建
	             checkDir("调用默认方法创建的目录");
	             return false;
	         }
	     });
	     */
	     cfg.setFileOutConfigList(focList);
	     mpg.setCfg(cfg);

	     // 配置模板
	     TemplateConfig templateConfig = new TemplateConfig();

	     // 配置自定义输出模板
	     //指定自定义模板路径，注意不要带上.ftl/.vm, 会根据使用的模板引擎自动识别
	     // templateConfig.setEntity("templates/entity2.java");
	     // templateConfig.setService();
	     // templateConfig.setController();

	     templateConfig.setXml(null);
	     mpg.setTemplate(templateConfig);

	     // 策略配置
	     StrategyConfig strategy = new StrategyConfig();
	     strategy.setNaming(NamingStrategy.underline_to_camel);// 数据库表映射到实体的命名策略
	     strategy.setColumnNaming(NamingStrategy.underline_to_camel);// 数据库表字段映射到实体的命名策略, 未指定按照 naming 执行
//	     strategy.setSuperEntityClass("com.xxx.application.common.BaseEntity");//自定义继承的Entity类全称，带包名
	     strategy.setEntityLombokModel(true);// 是否为lombok模型
	     strategy.setRestControllerStyle(true);// 生成 @RestController 控制器
	     // 公共父类
//	     strategy.setSuperControllerClass("com.xxx.application.common.BaseController");
	     // 写于父类中的公共字段
//	     strategy.setSuperEntityColumns("id");
	     strategy.setInclude(scanner("表名，多个英文逗号分割").split(","));
	     strategy.setControllerMappingHyphenStyle(true); // 驼峰转连字符 如 umps_user 变为 upms/user
	     strategy.setTablePrefix(pc.getModuleName() + "_");// 表前缀
	     mpg.setStrategy(strategy);
	     mpg.setTemplateEngine(new FreemarkerTemplateEngine());//设置模板引擎类型，默认为 velocity
	     mpg.execute();
	 }
}
```

<!-- # 使用 pv 记录方式，每访问一次，记录一次--> 
<span id="busuanzi_container_page_pv">  本文总阅读量<span id="busuanzi_value_page_pv"></span>次</span>

