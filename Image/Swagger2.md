# Swagger2

## maven

```java
<dependency>
  <groupId>io.springfox</groupId>
  <artifactId>springfox-swagger2</artifactId>
  <version>2.7.0</version>
</dependency>

<dependency>
  <groupId>io.springfox</groupId>
  <artifactId>springfox-swagger-ui</artifactId>
  <version>2.7.0</version>
</dependency>
```

## 步骤一

```java
@Configuration //配置类
@EnableSwagger2 //swagger注解
public class SwaggerConfig {
    @Bean
    public Docket webAppiConfig(){
        return new Docket(DocumentationType.SWAGGER_2)
                // 设置值
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("cn.intelhotel"))
                .paths(PathSelectors.any())
                .paths(Predicates.not(PathSelectors.regex("/error.")))//错误路径不监控
                //.paths(Predicates.not(PathSelectors.regex("/admin/")))//后台接口不监控
                .build();
    }
    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("智慧酒店文档")
                .description("智慧酒店API ")
                .termsOfServiceUrl("localhost:7010/api")
                .contact(new Contact("Swagger测试","localhost:7010/api/swagger-ui.html","baidu@qq.com"))
                .version("1.0.0")
                .build();
    }
}
```

## 步骤二 启动类配置

```java
// 组件扫描 扫描到Swagger2包
@ComponentScan(basePackages = {"cn.intelhotel"})
@SpringBootApplication
public class ConfigurationApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConfigurationApplication.class, args);
    }
}
```

## 访问

http://localhost:7010/swagger-ui.html

定义接口说明和参数说明

定义在类：@Api

定义在方式：@ApiOperation

定义在参数：@ApiParam

```java
@Api(description = "测试控制器") // 类中文注解
@RestController
public class Swagger2Controller {
    // 方法中文注解
    @ApiOperation(value = "登录")
    @RequestMapping("/login")
    public String out(@ApiParam(name = "username",value = "用户名",readOnly = true)@PathVariable String username,
                      @ApiParam(name = "password",value = "密码",readOnly = true)@PathVariable String password){
        return "success";
    }
}
```

![20200923175900276](https://github.com/2830980049/pubimages.git/Image/20200923175900276.jpg)

