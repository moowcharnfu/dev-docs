<pre>
场景mapper包1下用alibaba-durid-datasource来操作sql, mapper包2下用spring-boot-datasource来操作sql
关键点, 1Mapperscan扫描包, 2不同dataSource的创建语法 3必须用@Primary指定其中一个数据源为主要数据源

/**
 * 数据源1(alibaba-druid):多数据源增加@Primary让其中某个为主数据源
 */
@Configuration
@MapperScan(basePackages = {"{mapper包名1}"}, sqlSessionFactoryRef = "localSqlSessionFactory", sqlSessionTemplateRef = "localSqlSessionTemplate")
public class LocalDataSourceConfig {

    @Bean("localDataSource")
	// druid前缀
    @ConfigurationProperties("spring.datasource.druid")
    public DataSource localDataSource() {
		// dataSource创建实现,根据自己的dataSource类型改变,此处alibaba-druid写法
        return com.alibaba.druid.spring.boot.autoconfigure.DruidDataSourceBuilder.create().build();
    }

    @Bean("localSqlSessionFactory")
    public SqlSessionFactory localSqlSessionFactory(@Qualifier("localDataSource") DataSource dataSource) throws Exception {
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSource);
        sqlSessionFactoryBean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath*:{xml目录名1}/*.xml"));
        return sqlSessionFactoryBean.getObject();
    }

    @Bean("localSqlSessionTemplate")
    public SqlSessionTemplate localSqlSessionTemplate(
            @Qualifier("localSqlSessionFactory") SqlSessionFactory sessionfactory) {
        return new SqlSessionTemplate(sessionfactory);
    }
}

/**
 * 数据源2(spring-boot-datasource):多数据源增加@Primary让其中某个为主数据源
 */
@Configuration
@MapperScan(basePackages = {"{mapper包名2}"}, sqlSessionFactoryRef = "anotherSqlSessionFactory", sqlSessionTemplateRef = "anotherSqlSessionTemplate")
public class AnotherDataSourceConfig {
    @Bean(name = "anotherDataSource")
    @Primary
    public DataSource anotherDataSource() throws Exception {
		// dataSource创建实现,根据自己的dataSource类型改变,此处spring-boot-datasource写法
        return org.springframework.boot.jdbc.DataSourceBuilder.create().build();
    }

    @Bean("anotherSqlSessionFactory")
    @Primary
    public SqlSessionFactory anotherSqlSessionFactory(@Qualifier("anotherDataSource") DataSource dataSource) throws Exception {
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSource);
        sqlSessionFactoryBean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath*:{xml目录名2}/*.xml"));
        return sqlSessionFactoryBean.getObject();
    }

    @Bean("anotherSqlSessionTemplate")
    @Primary
    public SqlSessionTemplate anotherSqlSessionTemplate(
            @Qualifier("anotherSqlSessionFactory") SqlSessionFactory sessionfactory) {
        return new SqlSessionTemplate(sessionfactory);
    }
}
</pre>
