# API 

1. ## REST API

   1. ### 基本概念

      1. 无需画面,直接通过HTTP请求代码操作SF系统数据

   2. ### 基本要求

      1. API Access
         1. 支持版本
            1. Professional Edition
            2. Performance Edition
            3. Enterprise Edition
            4. Unlimited Edition
            5. Developer Edition
      2. API Enabled Permission
         1. Set Up&rArr;Profiles&rArr;System Permissions&rArr;API Enabled

   3. ### 请求格式

      1. URI

         1. https://MyDomain/services/data/vXX.X/resource/

      2. HTTP method

         | 请求方式 | Apex注解    |        |
         | -------- | ----------- | ------ |
         | HEAD     |             |        |
         | GET      | @HttpGet    | Read   |
         | POST     | @HttpPost   | Create |
         | PATCH    | @HttpPatch  | Update |
         | PUT      | @HttpPut    | Upsert |
         | DELETE   | @HttpDelete | Delete |

      3. Headers

         | Key              | Value                               | 含义               |
         | ---------------- | ----------------------------------- | ------------------ |
         | Accept           | application/json或者application/xml | 响应体格式         |
         | Content-type     | application/json或者application/xml | 请求体格式         |
         | Authorization    | token_type+" "+access_token         | Token认证信息      |
         | Content-Encoding | gzip/deflate                        | 请求压缩格式[可选] |
         | Accept-Encoding  | gzip/deflate                        | 响应压缩格式       |

      4. Request body

         1. JSON
         2. XML

   4. ### 认证Authorization

      1. SF确保访问安全使用Connected Apps和OAuth 2.0进行认证

         ![oauth gives app restricted access](https://resources.help.salesforce.com/images/18dba155966d9e106431b2afdb43a735.png)

      2. 设定过程

         1. [创建Connected App](https://help.salesforce.com/s/articleView?id=sf.connected_app_create.htm&type=5)

         2. [Apply an OAuth Authorization](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_oauth_flows.htm&type=5)

         3. 获取Token

            1. post请求获取Token

               ```bash
               https://自己的域名/services/oauth2/token?grant_type=password&client_id=公钥&client_secret=秘钥&username=用户名&password=密码
               ```

            2. 响应得到Token

               ```json
               {
                   "access_token": "00D5h000004eAF0!ARIAQPDkGrkWZUlN8TjSDk8mF2oBptVafZuVg0UEz9QhVKEntSfD_nkkYSDCf2uBoTD3AYxaeDkslGAIMRAV7fbBVKEZDrhX",
                   "instance_url": "https://jpanda-dev-ed.my.salesforce.com",
                   "id": "https://login.salesforce.com/id/00D5h000004eAF0EAM/0055h000003yxsXAAQ",
                   "token_type": "Bearer",
                   "issued_at": "1649511637621",
                   "signature": "+pCGWKnDRFPQtrHBjbA6bNxruDu8Kg4j76oy2naPqyQ="
               }
               ```

            3. 请求时带上Authorization=token_type+" "+access_token

               ```bash
               https://自己的域名/services/apexrest/URL映射/
               ```

   5. ### 将Apex暴露为REST Web Service

      1. 创建一个global的class

      2. 加上@RestResource(urlMapping='URL')注解,映射请求URL
      
      3. 创建global static的方法

      4. 加上Apex特定Http注解

         ```java
         @RestResource(urlMapping='/Account/*')
         global with sharing class MyRestResource {
          @HttpGet
             global static Account getRecord() {
              // Add your code
             }
      }
         ```
      
      5. 外部系统调用
      
         ```
         https://自己的域名/services/apexrest/URL映射/
         ```

   6. ### 请求方式

      1. CURL工具

         1. ```bash
            curl https://MyDomainName.my.salesforce.com/services/data/v54.0/sobjects/Account/ -H "Authorization: Bearer access-token" -H "Content-Type: application/json" -H "Content-Encoding: gzip" -data-binary/-d @new-account.json -X POST
            ```

         2. 参数

            1. -H:请求头
            2. -X:请求方法
            3. --data-binary/d:请求体文件

         3. 注意

            1. 使用CURL工具时Token中含有感叹号时需使用转义或者使用单引号

               ```bash
               //转义
               -H "Authorization: Bearer 00D50000000IehZ\!AQcAQH0dMHZfz"
               
               //单引号
               -H 'Authorization: Bearer 00D50000000IehZ!AQcAQH0dMHZfz'
               ```

      2. 测试工具PostMan等第三方工具

      3. Java Http请求

         1. 依赖

            ```xml
            <dependency>
                <groupId>org.apache.httpcomponents</groupId>
                <artifactId>httpclient</artifactId>
                <version>4.5.6</version>
            </dependency>
            ```

         2. 代码示例

            ```java
            package com.panda.springboot.controller;
            
            import java.util.HashMap;
            import java.util.List;
            import java.util.Map;
            
            import org.apache.http.client.methods.CloseableHttpResponse;
            import org.apache.http.client.methods.HttpDelete;
            import org.apache.http.client.methods.HttpGet;
            import org.apache.http.impl.client.CloseableHttpClient;
            import org.apache.http.impl.client.HttpClients;
            import org.apache.http.util.EntityUtils;
            import org.springframework.beans.factory.annotation.Autowired;
            import org.springframework.boot.configurationprocessor.json.JSONObject;
            import org.springframework.http.HttpEntity;
            import org.springframework.http.HttpHeaders;
            import org.springframework.web.bind.annotation.RequestMapping;
            import org.springframework.web.bind.annotation.ResponseBody;
            import org.springframework.web.bind.annotation.RestController;
            import org.springframework.web.client.RestTemplate;
            
            @RestController
            public class HttpController {
            	public static String Token = "Bearer 00D5h000004eAF0!ARIAQGKJG0OOpOiwauXbmCMbwUZufApz5XDPe8YHHdrYsxwvKY7vkzl02wOzhpAso_d9k92uT4M5OEXo0LGXp4ZRtN0yTY57";
            
            	@Autowired
            	RestTemplate restTemplate;
            
            	// 获取Token
            	@RequestMapping("/getToken")
            	@ResponseBody
            	public Object getToken() throws Exception {
            		HttpHeaders headers = new HttpHeaders();
            		Map<String, String> params = new HashMap<String, String>();
            		params.put("grant_type", "password");
            		params.put("client_id", "3MVG95mg0lk4batgUcNTfbzbDIMYLxflxYHUstEZa8P0.5lpS13SoRDryDw2Ya32yXxxtzG_XVjyVudp4rjFH");
            		params.put("client_secret", "6D8D0B82F0D4BB6516C56DDFADF84F3864DD25A2A05ACB054B3D9A89D4A793E6");
            		params.put("username", "770233258@qq.com");
            		params.put("password", "123456");
            		String response = restTemplate.postForObject("https://jpanda-dev-ed.my.salesforce.com/services/oauth2/token", new HttpEntity<>(params, headers), String.class);
            		return response;
            	}
            
            	// 获取所有api版本信息
            	@SuppressWarnings("unchecked")
            	@RequestMapping("/getRestApiVersions")
            	@ResponseBody
            	public Object getRestApiVersions() throws Exception {
            		List<JSONObject> result = restTemplate.getForObject("https://jpanda-dev-ed.my.salesforce.com/services/data", List.class);
            		return result.toString();
            	}
            
            	// 获取限制信息
            	@RequestMapping("/getOrganizationLimits")
            	@ResponseBody
            	public Object getOrganizationLimits() throws Exception {
            		CloseableHttpClient closeableHttpClient = HttpClients.createDefault();
            		HttpGet get = new HttpGet("https://jpanda-dev-ed.my.salesforce.com/services/data/v54.0/limits");
            		get.setHeader("Authorization", Token);
            		CloseableHttpResponse response = closeableHttpClient.execute(get);
            		return EntityUtils.toString(response.getEntity(), "utf-8");
            	}
            
            	// 获取可访问资源
            	@RequestMapping("/getAvailableRESTResources")
            	@ResponseBody
            	public Object getAvailableRESTResources() throws Exception {
            		CloseableHttpClient closeableHttpClient = HttpClients.createDefault();
            		HttpGet get = new HttpGet("https://jpanda-dev-ed.my.salesforce.com/services/data/v54.0");
            		get.setHeader("Authorization", Token);
            		CloseableHttpResponse response = closeableHttpClient.execute(get);
            		String ret = EntityUtils.toString(response.getEntity(), "utf-8");
            		JSONObject jsonObject = new JSONObject(ret);
            		System.out.println(jsonObject);
            		return ret;
            	}
            
            	// 获取所有对象
            	@RequestMapping("getAllObjects")
            	@ResponseBody
            	public Object getAllObjects() throws Exception {
            		CloseableHttpClient closeableHttpClient = HttpClients.createDefault();
            		HttpGet get = new HttpGet("https://jpanda-dev-ed.my.salesforce.com/services/data/v54.0/sobjects");
            		get.setHeader("Authorization", Token);
            		CloseableHttpResponse response = closeableHttpClient.execute(get);
            		String ret = EntityUtils.toString(response.getEntity(), "utf-8");
            		JSONObject jsonObject = new JSONObject(ret);
            		System.out.println(jsonObject.getJSONArray("sobjects").get(0).toString());
            		return ret;
            	}
            
            	// 获取对象信息
            	@RequestMapping("getAccount")
            	@ResponseBody
            	public Object getAccount() throws Exception {
            		CloseableHttpClient closeableHttpClient = HttpClients.createDefault();
            		HttpGet get = new HttpGet("https://jpanda-dev-ed.my.salesforce.com/services/data/v54.0/sobjects/Account");
            		get.setHeader("Authorization", Token);
            		CloseableHttpResponse response = closeableHttpClient.execute(get);
            		String ret = EntityUtils.toString(response.getEntity(), "utf-8");
            		JSONObject jsonObject = new JSONObject(ret);
            		System.out.println(jsonObject.toString());
            		return ret;
            	}
            
            	// 获取对象详细信息
            	@RequestMapping("getAccountDescribe")
            	@ResponseBody
            	public Object getAccountDescribe() throws Exception {
            		CloseableHttpClient closeableHttpClient = HttpClients.createDefault();
            		HttpGet get = new HttpGet("https://jpanda-dev-ed.my.salesforce.com/services/data/v54.0/sobjects/Account/describe");
            		get.setHeader("Authorization", Token);
            		CloseableHttpResponse response = closeableHttpClient.execute(get);
            		String ret = EntityUtils.toString(response.getEntity(), "utf-8");
            		JSONObject jsonObject = new JSONObject(ret);
            		System.out.println(jsonObject.toString());
            		return ret;
            	}
            
            	// 创建记录
            	@RequestMapping("createAccount")
            	@ResponseBody
            	public Object createAccount() throws Exception {
            		HttpHeaders headers = new HttpHeaders();
            		headers.set("Authorization", Token);
            		Map<String, String> params = new HashMap<String, String>();
            		params.put("Name", "Java");
            		String response = restTemplate.postForObject("https://jpanda-dev-ed.my.salesforce.com/services/data/v54.0/sobjects/Account/", new HttpEntity<>(params, headers), String.class);
            		return response;
            	}
            
            	// 更新记录
            	@RequestMapping("updateAccount")
            	@ResponseBody
            	public Object updateAccount() throws Exception {
            		String id = "0015h00000m3PPDAA2";
            		HttpHeaders headers = new HttpHeaders();
            		headers.set("Authorization", Token);
            		Map<String, String> params = new HashMap<String, String>();
            		params.put("BillingCity", "San Francisco");
            		String response = restTemplate.patchForObject("https://jpanda-dev-ed.my.salesforce.com/services/data/v54.0/sobjects/Account/" + id, new HttpEntity<>(params, headers), String.class);
            		return response;
            	}
            
            	// 删除记录
            	@RequestMapping("deleteAccount")
            	@ResponseBody
            	public Object deleteAccount() throws Exception {
            		String id = "0015h00000m3PPDAA2";
            		CloseableHttpClient closeableHttpClient = HttpClients.createDefault();
            		HttpDelete delete = new HttpDelete("https://jpanda-dev-ed.my.salesforce.com/services/data/v54.0/sobjects/Account/" + id);
            		delete.setHeader("Authorization", Token);
            		CloseableHttpResponse response = closeableHttpClient.execute(delete);
            		return "Success";
            	}
            
            	// 获取对象的指定字段
            	@RequestMapping("getAccountFields")
            	@ResponseBody
            	public Object getAccountFields() throws Exception {
            		CloseableHttpClient closeableHttpClient = HttpClients.createDefault();
            		HttpGet get = new HttpGet("https://jpanda-dev-ed.my.salesforce.com/services/data/v54.0/sobjects/Account/0015h00000m3PPDAA2" + "?" + "fields=Id,Name,BillingCity");
            		get.setHeader("Authorization", Token);
            		CloseableHttpResponse response = closeableHttpClient.execute(get);
            		return EntityUtils.toString(response.getEntity(), "utf-8");
            	}
            
            	// 执行SOQL访问标准对象
            	@RequestMapping("getAccountQuery")
            	@ResponseBody
            	public Object getAccountQuery() throws Exception {
            		String soql = "SELECT+ID,name,BillingCity+from+Account+where+ID='0015h00000m3PPDAA2'";
            		CloseableHttpClient closeableHttpClient = HttpClients.createDefault();
            		HttpGet get = new HttpGet("https://jpanda-dev-ed.my.salesforce.com/services/data/v54.0/query/" + "?q=" + soql);
            		get.setHeader("Authorization", Token);
            		CloseableHttpResponse response = closeableHttpClient.execute(get);
            		return EntityUtils.toString(response.getEntity(), "utf-8");
            	}
            
            	// 执行SOQL访问外部对象
            	@RequestMapping("getExternalObjectQuery")
            	@ResponseBody
            	public Object getExternalObjectQuery() throws Exception {
            		CloseableHttpClient closeableHttpClient = HttpClients.createDefault();
            		HttpGet get = new HttpGet("https://jpanda-dev-ed.my.salesforce.com/services/data/v54.0/sobjects/LJ_c__x");
            		get.setHeader("Authorization", Token);
            		CloseableHttpResponse response = closeableHttpClient.execute(get);
            		return EntityUtils.toString(response.getEntity(), "utf-8");
            	}
            
            	// 访问自定义RestApi
            	@RequestMapping("getMyRestApi")
            	@ResponseBody
            	public Object getMyRestApi() throws Exception {
            		String soql = "SELECT+ID,name,BillingCity+from+Account+where+ID='0015h00000m3PPDAA2'";
            		CloseableHttpClient closeableHttpClient = HttpClients.createDefault();
            		HttpGet get = new HttpGet("https://jpanda-dev-ed.my.salesforce.com/services/apexrest/test");
            		get.setHeader("Authorization", Token);
            		CloseableHttpResponse response = closeableHttpClient.execute(get);
            		return EntityUtils.toString(response.getEntity(), "utf-8");
            	}
            
            }
            
            ```

      4. Workbench

         1. 登录Workbench&rArr;Rest Explorer

   7. ### 常用URL及其含义

      [官方参考文档](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm)

2. ## SOAP API

   1. ###### 将Apex暴露为SOAP Web Service

      1. 创建一个global的class

      2. 创建webservice static的方法

         ```java
         global with sharing class MySOAPWebService {
             webservice static Account getRecord(String id) {
                 // Add your code
             }
         }
         ```

   2. ###### 生成WSDL文件

      1. Enterprise WSDL
         1. 单一sf组织,强类型,兼容性差
         2. URL:https://login.salesforce.com/services/Soap/c/54.0
      2. Partner WSDL
         1. 多个sf组织通用,弱类型.兼容性好,采用键值对传输
         2. URL:https://login.salesforce.com/services/Soap/u/54.0

   3. ###### webservice关键字使用注意

      1. webservice关键字只使用在外部类
      2. 不能写在接口里面,必须为实现类,无法用在Trigger中
      3. webservice关键字所在类必须为global修饰
      4. webservice修饰的方法必须为static,修饰变量的时候无需static
      5. webservice方法输入和输出参数不能为下列参数类型
         1. Maps
         2. Sets
         3. Pattern objects
         4. Matcher objects
         5. Exception objects
      6. webservice修饰的方法不支持方法重载

   4. ###### 生成Jar文件

      1. 准备force-wsc-54.0.0.jar;ST4-4.3.3.jar;antlr-runtime-3.5.2.jar将第一步生成的xml文件生成jar包

      2. ```bash
         java -DcompileTarget=1.8  -classpath force-wsc-54.0.0.jar;ST4-4.3.3.jar;antlr-runtime-3.5.2.jar com.sforce.ws.tools.wsdlc wsdl.xml wsdl.jar
         ```

3. ## Bulk 2.0 API

4. ## Metadata API

   1. ### 理论知识

      1. Salesforce Metadata分类

         1. schema:对象字段元数据
         2. process:流程控制元数据
         3. presentation:布局展示元数据
         4. authorization:认证授权元数据
         5. general configuration:通用配置元数据

      2. 主要功能

         1. 开发过程中在不同Org之间进行元数据传输
         2. deploy, retrieve, create, update, delete元数据,不能直接操作数据,如要直接操作数据则应当用REST API,SOAP API

      3. 作用对象

         1. metadata types
         2. components

      4. 操作分类

         1. deploy
         2. retrieve
         3. push
         4. pull

      5. 环境准备

         1. Set Up&rArr;API

            1. Generate Metadata WSDL
            2. Generate Enterprise WSDL

         2. 生成java jar包

            1. 准备force-wsc-54.0.0.jar;ST4-4.3.3.jar;antlr-runtime-3.5.2.jar将第一步生成的xml文件生成jar包

            2. ```bash
               java -DcompileTarget=1.8  -classpath force-wsc-54.0.0.jar;ST4-4.3.3.jar;antlr-runtime-3.5.2.jar com.sforce.ws.tools.wsdlc wsdl.xml wsdl.jar
               ```

   2. ## Connect REST API

