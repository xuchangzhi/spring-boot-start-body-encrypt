## 前言

- 前后端分离从数据的安全性考虑，http请求过程中，请求体和响应体加密非常重要。
- 考虑通用性，不会对所有请求都执行加解密，基于注解来做控制。
- spring-boot-start-body-encrypt是对基于spring mvc 的Web框架API请求进行统一加解密操作的框架。

## 功能点

- 内置AES加密算法
- 基于注解控制请求数据解密
- 基于注解控制响应数据加密
- 使用简单，操作示列

## 使用说明

- 启动类上增加@EnableEncrypt注解开启加解密
  ```java 
      @EnableEncrypt
      @SpringBootApplication
      public class App {
        
          public static void main(String[] args) {
            SpringApplication.run(App.class, args);
          }
      }
    ```
  
  
- application.properties 增加加密的key配置

  ```java 
    spring.body.encrypt.enable=true
    #加密key，必须是16位
    spring.body.encrypt.key=abcdef0123456789
    #是否开启调试模式,默认为false,如果为true则不启用加解密操作
    spring.body.encrypt.debug=false
  ```
- 响应数据加密

   ```java 
    @Encrypt
    @ApiOperation(value = "001、响应加密")
    @RequestMapping(value = "/encrypt", method = RequestMethod.POST)
    public CommonResult encrypt(@RequestBody Student student) {

        return CommonResult.success(student);

    }
    ```
    加了@Encrypt注解后，返回的数据就是加密之后base64编码的格式
- 请求数据解密
    ```java 
    @Decrypt
    @ApiOperation(value = "002、请求数据解密")
    @RequestMapping(value = "/decrypt", method = RequestMethod.POST)
    public CommonResult Decrypt(@RequestBody Student student) {
        return CommonResult.success(student);
    }
     ```
    加了@Decrypt注解后，前端提交的数据需要按照AES加密算法，进行加密，然后提交到后端，后端这边会自动解密，然后再映射到参数对象中。
     
## 前端
- 统一处理数据的响应，在渲染到页面之前进行解密操作
- 当有POST请求的数据发出时，统一加密