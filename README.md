# springmvc-springdata-mongodb-example
>1. 本工程演示了spring 4.3.2/spring mvc 4.3.2 + spring data for mongodb 1.9.4 + log4j2(搭配slf4j而不是commons-logging) 搭建的mvc工程，spring采用java-based config和xml config一起使用的配置方式。log4j2是log4j的大改动之后的版本，具体例子可参考[log4j2-example](https://github.com/GrayOxygen/log4j2-example)
>2. 可参考src/test/java下的com.shineoxygen.work.junit包和com.shineoxygen.work.maintest包的测试方法，前者是junit和spring test框架编写的单元测试（测试spring data操作mongodb），后者用main方法写的测试方法（测试mongodb driver类库操作mongodb）。
3. 在com.shineoxygen.work.base.dao中提供了公用的repository及实现，我们在编写应用程序时，每添加一个业务dao接口即repository接口，继承base.dao下的公用repository即可，如果需要自定义方法，还可通过定义一个普通的接口并实现MongoRepository接口，该接口定义自定义方法，且给出这个普通接口的实现（可注入其他bean，如MongoOperations/MongoTemplate注入后可操作mongodb数据库），这个实现同时继承了SimpleMongoRepository类，免去实现MongoRepository接口的方法的工作量，然后我们的业务dao接口实现这个普通的接口，从而我们的repository实现方法自定义。这是一种开发方式。
另一种开发方式，我们可以封装一个基础dao，所有业务dao继承之，基础dao提供所有公用数据库方法，这个dao注入MongoOperations/MongoTemplate即可。
4. 因为考虑到我想用Mongo Repository而不是Mongo Template，所有第二种方式没有封装一个基础dao。


    	<properties>
		<!-- Generic properties -->
		<!-- jre版本，java.lang.System系统属性 -->
		<java.version>1.8</java.version>
		<!--compiler插件属性，指定编译器所用编码 -->
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<!-- site插件属性，站点生成所用编码 -->
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>

		<!-- junit -->
		<junit.version>4.12</junit.version>
		<!-- aspectj -->
		<org.aspectj-version>1.6.10</org.aspectj-version>
		<!-- slf4j -->
		<org.slf4j-version>1.7.9</org.slf4j-version>
		<!-- log4j2 -->
		<log4j2-version>2.7</log4j2-version>

		<!-- spring data mongodb -->
		<spring-data-version>1.9.4.RELEASE</spring-data-version>

	</properties>
	<!-- spring的bom统一spring包版本 -->
	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-framework-bom</artifactId>
				<version>4.3.2.RELEASE</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>


	<dependencies>
		<!-- Spring -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<exclusions>
				<!-- Exclude Commons Logging in favor of SLF4j -->
				<exclusion>
					<groupId>commons-logging</groupId>
					<artifactId>commons-logging</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aop</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
		</dependency>
		<!-- spring data mongodb -->
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-mongodb</artifactId>
			<version>${spring-data-version}</version>
		</dependency>
		<!-- AspectJ -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjrt</artifactId>
			<version>${org.aspectj-version}</version>
		</dependency>

		<!-- Logging -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>${org.slf4j-version}</version>
		</dependency>
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-slf4j-impl</artifactId>
			<version>${log4j2-version}</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>jcl-over-slf4j</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-core</artifactId>
			<version>${log4j2-version}</version>
		</dependency>




		<!-- log4j2 日志 -->
		<!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-core -->
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-core</artifactId>
>- 本工程演示了spring 4.3.2/spring mvc 4.3.2 + spring data for mongodb 1.9.4 + log4j2(搭配slf4j而不是commons-logging) 搭建的mvc工程。spring采用java-based config和xml config一起使用的配置方式，通过spring提供的BOM来统一所有spring相关的maven依赖的版本号，log4j2是log4j的大改动之后的版本，具体例子可参考![log4j2-example](https://github.com/GrayOxygen/log4j2-example)。
- 可参考src/test/java下的com.shineoxygen.work.junit包和com.shineoxygen.work.maintest包的测试方法，前者是junit和spring test框架编写的单元测试（测试spring data操作mongodb），后者用main方法写的测试方法（测试mongodb driver类库操作mongodb）。
- 在com.shineoxygen.work.base.dao中提供了公用的repository及实现，我们在编写应用程序时，每添加一个业务dao接口即repository接口，继承base.dao下的公用repository即可，如果需要自定义方法，还可通过定义一个普通的接口并实现MongoRepository接口，该接口定义自定义方法，且给出这个普通接口的实现（可注入其他bean，如MongoOperations/MongoTemplate注入后可操作mongodb数据库），这个实现同时继承了SimpleMongoRepository类，免去实现MongoRepository接口的方法的工作量，然后我们的业务dao接口实现这个普通的接口，从而我们的repository实现方法自定义。这是一种开发方式。
另一种开发方式，我们可以封装一个基础dao，所有业务dao继承之，基础dao提供所有公用数据库方法，这个dao注入MongoOperations/MongoTemplate来实现公用数据库方法。
-  因为考虑到我想用Mongo Repository而不是Mongo Template，所有第二种方式没有封装一个基础dao。

下面是POM文件的属性和依赖配置，可了解本工程所用技术以及相应版本
    	<properties>
		<!-- Generic properties -->
		<!-- jre版本，java.lang.System系统属性 -->
		<java.version>1.8</java.version>
		<!--compiler插件属性，指定编译器所用编码 -->
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<!-- site插件属性，站点生成所用编码 -->
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>

		<!-- junit -->
		<junit.version>4.12</junit.version>
		<!-- aspectj -->
		<org.aspectj-version>1.6.10</org.aspectj-version>
		<!-- slf4j -->
		<org.slf4j-version>1.7.9</org.slf4j-version>
		<!-- log4j2 -->
		<log4j2-version>2.7</log4j2-version>

		<!-- spring data mongodb -->
		<spring-data-version>1.9.4.RELEASE</spring-data-version>

	</properties>
	<!-- spring的bom统一spring包版本 -->
	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-framework-bom</artifactId>
				<version>4.3.2.RELEASE</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>


	<dependencies>
		<!-- Spring -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<exclusions>
				<!-- Exclude Commons Logging in favor of SLF4j -->
				<exclusion>
					<groupId>commons-logging</groupId>
					<artifactId>commons-logging</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aop</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
		</dependency>
		<!-- spring data mongodb -->
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-mongodb</artifactId>
			<version>${spring-data-version}</version>
		</dependency>
		<!-- AspectJ -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjrt</artifactId>
			<version>${org.aspectj-version}</version>
		</dependency>

		<!-- Logging -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>${org.slf4j-version}</version>
		</dependency>
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-slf4j-impl</artifactId>
			<version>${log4j2-version}</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>jcl-over-slf4j</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-core</artifactId>
			<version>${log4j2-version}</version>
		</dependency>




		<!-- log4j2 日志 -->
		<!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-core -->
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-core</artifactId>
			<version>2.6.2</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-api -->
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-api</artifactId>
			<version>2.6.2</version>
		</dependency>



		<!-- @Inject -->
		<dependency>
			<groupId>javax.inject</groupId>
			<artifactId>javax.inject</artifactId>
			<version>1</version>
		</dependency>

		<!-- Servlet -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.1</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>

		<!-- Test Artifacts -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>${junit.version}</version>
			<scope>test</scope>
		</dependency>
		<!-- commons-lang3 -->
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-lang3</artifactId>
			<version>3.5</version>
		</dependency>
		<dependency>
			<groupId>commons-beanutils</groupId>
			<artifactId>commons-beanutils</artifactId>
			<version>1.9.3</version>
		</dependency>

		<dependency>
			<groupId>com.google.code.gson</groupId>
			<artifactId>gson</artifactId>
			<version>2.7</version>
		</dependency>



	</dependencies>
