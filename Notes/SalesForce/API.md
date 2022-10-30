# API 

1. 总括

   1. 分类
      1. REST
      2. SOAP
      3. BULK
      4. Streaming

   2. 限制
      1. 时常限制
      2. 并发限制
      3. 访问次数限制 

2. ## REST API

   1. 定位:Rest风格的API,有XML和JSON两种格式,使用简单

   2. ### 基本概念

      1. 无需画面,直接通过HTTP请求代码操作SF系统数据

   3. ### 基本要求

      1. API Access
         1. 支持版本
            1. Professional Edition
            2. Performance Edition
            3. Enterprise Edition
            4. Unlimited Edition
            5. Developer Edition
      2. API Enabled Permission
         1. Set Up&rArr;Profiles&rArr;System Permissions&rArr;API Enabled

   4. ### 请求格式

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

   5. ### 认证Authorization

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

   6. ### 将Apex暴露为REST Web Service

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

   7. ### 请求方式

      1. CURL工具

         1. ```bash
            curl -v https://login.salesforce.com/services/oauth2/token -d "grant_type=password&client_id=3MVG95mg0lk4batgUcNTfbzbDIMYLxflxYHUstEZa8P0.5lpS13SoRDryDw2Ya32yXxxtzG_XVjyVudp4rjFH&client_secret=6D8D0B82F0D4BB6516C56DDFADF84F3864DD25A2A05ACB054B3D9A89D4A793E6&username=770233258@qq.com&password=sf152979@" -H "Content-Type:application/x-www-form-urlencoded"
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

   8. ### 常用URL及其含义

      [官方参考文档](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm)

3. ## SOAP API

   1. 定位:基于WSDL文件的Web Service数据访问API只支持XML

   2. ###### 将Apex暴露为SOAP Web Service

      1. 创建一个global的class

      2. 创建webservice static的方法

         ```java
         global with sharing class EmployeeSoapApi {
             public EmployeeSoapApi() {
             }
             webservice static List<Employee__c> getAllEmps(){
                 return [SELECT Id, LastName__c, Email__c, Gender__c, Birthday__c, Department__r.Id,Department__r.Name FROM Employee__c];
             }
         }
         ```

      3. 测试自己创建的webservice

         ``` java
         @isTest
         private class EmployeeSoapApiTest {
             testMethod static void getAllEmpsTest(){
                 Employee__c e = new Employee__c();
                 e.LastName__c='LJ';
                 e.Email__c='770233258@qq.com';
                 insert e;
                 Employee__c[] emps=EmployeeSoapApi.getAllEmps();
                 System.assertEquals(e.LastName__c, emps[0].LastName__c);
                 System.assertEquals(e.Email__c, emps[0].Email__c);
             }
         }
         ```

   3. ###### 生成WSDL文件(Web Services Description Language)

      1. Enterprise WSDL
         1. 单一sf组织,强类型,兼容性差
         2. URL:https://login.salesforce.com/services/Soap/c/54.0
      2. Partner WSDL
         1. 多个sf组织通用,弱类型.兼容性好,采用键值对传输
         2. URL:https://login.salesforce.com/services/Soap/u/54.0

   4. ###### webservice关键字使用注意

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
      6. webservice标记的方法总是System级别的上下文,对对象和字段级别的权限将失效

   5. ###### 生成Jar文件

      1. 命令行方式

         1. 准备force-wsc-54.0.0.jar;ST4-4.3.3.jar;antlr-runtime-3.5.2.jar将第一步生成的xml文件生成jar包

         2. ```bash
            java -classpath force-wsc-54.0.0.jar;ST4-4.3.3.jar;antlr-runtime-3.5.2.jar com.sforce.ws.tools.wsdlc wsdl.xml wsdl.jar
            ```

      2. Java项目方式

         1. com.sforce.ws.tools.wsdlc本质是调用这个Main函数生成相应的jar,可以自行创建Java工程来运行该类

         2. ``` xml
            <dependency>
                <groupId>com.force.api</groupId>
                <artifactId>force-wsc</artifactId>
                <version>54.0.0</version>
            </dependency>
            ```

   6. 调用官方API案例

      1. ``` java
         package com.panda.SpringBoot.demo.soap;
         import com.sforce.soap.enterprise.Connector;
         import com.sforce.soap.enterprise.EnterpriseConnection;
         import com.sforce.soap.enterprise.LoginResult;
         import com.sforce.soap.enterprise.QueryResult;
         import com.sforce.soap.enterprise.sobject.Department__c;
         import com.sforce.soap.enterprise.sobject.Employee__c;
         import com.sforce.soap.enterprise.sobject.SObject;
         import com.sforce.ws.ConnectionException;
         import com.sforce.ws.ConnectorConfig;
         
         import java.text.SimpleDateFormat;
         import java.time.Instant;
         import java.util.Date;
         
         public class SoapEnterpriseApiDemo {
             public static void main(String[] args) {
                 ConnectorConfig connectorConfig = new ConnectorConfig();
                 String username = "770233258@qq.com";
                 String password = "sf152979@";
                 // 默认官方Endpoint
                 connectorConfig.setAuthEndpoint(Connector.END_POINT);
                 connectorConfig.setUsername(username);
                 connectorConfig.setPassword(password);
                 try {
                     // 创建连接
                     EnterpriseConnection connection = new EnterpriseConnection(connectorConfig);
         
                     // 登录
                     LoginResult login = connection.login(username, password);
                     // 获取登录获得的实际Endpoint
                     connectorConfig.setAuthEndpoint(login.getServerUrl());
                     System.out.println(login.getSessionId());
                     // 查询SOQL
                     QueryResult query = connection.query("SELECT Id, LastName__c, Email__c, Gender__c, Birthday__c, Department__r.Id,Department__r.Name FROM Employee__c limit 3");
                     for (SObject record : query.getRecords()) {
                         // 类型强制转换
                         // 主对象
                         Employee__c employee__c = (Employee__c)record;
                         // 引用对象
                         Department__c department__r = employee__c.getDepartment__r();
                         // 遍历结果集
                         System.out.println(employee__c.getName());
                         // 时间转换
                         Instant instant = employee__c.getBirthday__c().toInstant();
                         Date from = Date.from(instant);
                         SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
                         String birth = simpleDateFormat.format(from);
                         System.out.println(birth);
                         System.out.println(employee__c.getEmail__c());
                         System.out.println(employee__c.getGender__c());
                         System.out.println(employee__c.getLastName__c());
                         System.out.println(department__r.getName());
                         System.out.println();
                     }
                     // 退出登录
                     connection.logout();
                 } catch (ConnectionException e) {
                     throw new RuntimeException(e);
                 }
             }
         }
         ```

   7. 调用自定义WebService API案例

      1. ``` java
         package com.panda.SpringBoot.demo.soap;
         
         import com.sforce.soap.EmployeeSoapApi.Connector;
         import com.sforce.soap.EmployeeSoapApi.Department__c;
         import com.sforce.soap.EmployeeSoapApi.Employee__c;
         import com.sforce.soap.EmployeeSoapApi.SoapConnection;
         import com.sforce.soap.enterprise.EnterpriseConnection;
         import com.sforce.soap.enterprise.LoginResult;
         import com.sforce.ws.ConnectionException;
         import com.sforce.ws.ConnectorConfig;
         
         import java.text.SimpleDateFormat;
         import java.time.Instant;
         import java.util.Date;
         
         public class SoapWebServiceApiDemo {
             public static void main(String[] args) {
                 ConnectorConfig connectorConfig = new ConnectorConfig();
                 String username = "770233258@qq.com";
                 String password = "sf152979@";
                 // 默认官方Session授权Endpoint(enterprise.Connector.END_POINT)中可以获取)
                 connectorConfig.setAuthEndpoint(com.sforce.soap.enterprise.Connector.END_POINT);
                 connectorConfig.setUsername(username);
                 connectorConfig.setPassword(password);
         
                 try {
                     // 首先调用EnterpriseConnection获取SessionId
                     EnterpriseConnection enterpriseConnection = new EnterpriseConnection(connectorConfig);
                     LoginResult loginResult = enterpriseConnection.login(username,password);
                     // 设置SessionId
                     connectorConfig.setSessionId(loginResult.getSessionId());
                     // 设置业务结点
                     connectorConfig.setServiceEndpoint(Connector.END_POINT);
                     // 创建业务连接
                     SoapConnection soapConnection = Connector.newConnection(connectorConfig);
                     // 调用业务方法
                     Employee__c[] allEmps = soapConnection.getAllEmps();
                     for (Employee__c employee__c : allEmps) {
                         Department__c department__r = employee__c.getDepartment__r();
                         Instant instant = employee__c.getBirthday__c().toInstant();
                         Date from = Date.from(instant);
                         SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
                         String birth = simpleDateFormat.format(from);
                         System.out.println(birth);
                         System.out.println(employee__c.getEmail__c());
                         System.out.println(employee__c.getGender__c());
                         System.out.println(employee__c.getLastName__c());
                         System.out.println(department__r.getName());
                         System.out.println();
                     }
                 } catch (ConnectionException e) {
                     throw new RuntimeException(e);
                 }
             }
         }
         ```

   8. SoapUi工具测试Soap API

      1. 创建Soap工程,导入相应的WSDL文件

      2. 点击请求函数,设置相应请求参数以及URL

      3. 发送请求

      4. 响应结果

      5. 常用操作

         1. login()获取sessionId和serverUrl

            1. 请求[https://login.salesforce.com/services/Soap/c/56.0/0DF5h000000XaRJ]

               1. ``` xml
                  
                  <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:urn="urn:enterprise.soap.sforce.com">
                     <soapenv:Body>
                        <urn:login>
                           <urn:username>770233258@qq.com</urn:username>
                           <urn:password>sf152979@</urn:password>
                        </urn:login>
                     </soapenv:Body>
                  </soapenv:Envelope>
                  ```

            2. 响应

               1. ``` xml
                  <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns="urn:enterprise.soap.sforce.com" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
                     <soapenv:Body>
                        <loginResponse>
                           <result>
                              <metadataServerUrl>https://jpanda-dev-ed.my.salesforce.com/services/Soap/m/56.0/00D5h000004eAF0</metadataServerUrl>
                              <passwordExpired>false</passwordExpired>
                              <sandbox>false</sandbox>
                              <serverUrl>https://jpanda-dev-ed.my.salesforce.com/services/Soap/c/56.0/00D5h000004eAF0/0DF5h000000XaRJ</serverUrl>
                              <sessionId>00D5h000004eAF0!ARIAQF_MtoLFZFIpxb5S8ZwaCj7GiBf4p10enRAB4mKH8Bc6anmwWB_wSnmaCzOI0JDZXOxTXSfeUuFvTgQb3oqfMmqVzT3L</sessionId>
                              <userId>0055h000003yxsXAAQ</userId>
                              <userInfo>
                                 <accessibilityMode>false</accessibilityMode>
                                 <chatterExternal>false</chatterExternal>
                                 <currencySymbol>￥</currencySymbol>
                                 <orgAttachmentFileSizeLimit>26214400</orgAttachmentFileSizeLimit>
                                 <orgDefaultCurrencyIsoCode>JPY</orgDefaultCurrencyIsoCode>
                                 <orgDefaultCurrencyLocale>ja_JP</orgDefaultCurrencyLocale>
                                 <orgDisallowHtmlAttachments>false</orgDisallowHtmlAttachments>
                                 <orgHasPersonAccounts>false</orgHasPersonAccounts>
                                 <organizationId>00D5h000004eAF0EAM</organizationId>
                                 <organizationMultiCurrency>false</organizationMultiCurrency>
                                 <organizationName>Jpanda</organizationName>
                                 <profileId>00e5h000003AZ8tAAG</profileId>
                                 <roleId xsi:nil="true"/>
                                 <sessionSecondsValid>7200</sessionSecondsValid>
                                 <userDefaultCurrencyIsoCode xsi:nil="true"/>
                                 <userEmail>liujianipanda@gmail.com</userEmail>
                                 <userFullName>LIU JIAN</userFullName>
                                 <userId>0055h000003yxsXAAQ</userId>
                                 <userLanguage>en_US</userLanguage>
                                 <userLocale>ja_JP</userLocale>
                                 <userName>770233258@qq.com</userName>
                                 <userTimeZone>Asia/Tokyo</userTimeZone>
                                 <userType>Standard</userType>
                                 <userUiSkin>Theme3</userUiSkin>
                              </userInfo>
                           </result>
                        </loginResponse>
                     </soapenv:Body>
                  </soapenv:Envelope>
                  ```

         2. 调用标准接口

            1. 请求[https://jpanda-dev-ed.my.salesforce.com/services/Soap/c/56.0/00D5h000004eAF0/0DF5h000000XaRJ]

               1. ``` xml
                  <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:urn="urn:enterprise.soap.sforce.com">
                     <soapenv:Header>
                  	 <urn:SessionHeader>
                           <urn:sessionId>00D5h000004eAF0!ARIAQF_MtoLFZFIpxb5S8ZwaCj7GiBf4p10enRAB4mKH8Bc6anmwWB_wSnmaCzOI0JDZXOxTXSfeUuFvTgQb3oqfMmqVzT3L</urn:sessionId>
                        </urn:SessionHeader>
                     </soapenv:Header>
                     <soapenv:Body>
                        <urn:query>
                           <urn:queryString>SELECT Id, LastName__c, Email__c, Gender__c, Birthday__c, Department__r.Id,Department__r.Name FROM Employee__c</urn:queryString>
                        </urn:query>
                     </soapenv:Body>
                  </soapenv:Envelope>
                  ```

         3. 调用自定义接口

            1. 请求[https://ap25.salesforce.com/services/Soap/class/EmployeeSoapApi]

               1. ``` xml
                  <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:emp="http://soap.sforce.com/schemas/class/EmployeeSoapApi">
                     <soapenv:Header>
                        <emp:SessionHeader>
                           <emp:sessionId>00D5h000004eAF0!ARIAQF_MtoLFZFIpxb5S8ZwaCj7GiBf4p10enRAB4mKH8Bc6anmwWB_wSnmaCzOI0JDZXOxTXSfeUuFvTgQb3oqfMmqVzT3L</emp:sessionId>
                        </emp:SessionHeader>
                     </soapenv:Header>
                     <soapenv:Body>
                        <emp:getAllEmps/>
                     </soapenv:Body>
                  </soapenv:Envelope>
                  ```

4. ## Bulk 2.0 API

   1. 定位:处理大量数据的异步API

   2. 数据导入

      1. 创建Job(Post)

         1. Create job

            1. ``` 
               {{_endpoint}}/services/data/v{{version}}/jobs/ingest
               ```

      2. 加载数据(Put)

         1. Upload Job Data

            1. ``` 
               {{_endpoint}}/services/data/v{{version}}/jobs/ingest/{{_jobId}}/batches
               ```

      3. 开启Job(Patch)

         1. Close or Abort a Job

            1. ``` 
               {{_endpoint}}/services/data/v{{version}}/jobs/ingest/{{_jobId}}
               ```

5. ## Streaming API

   1. 定位:基于publish-subscribe模型,当数据发生变化时,进行通知
   2. 基于CometD使服务器能够向客户端推送消息
      1. 一代
         1. PushTopic events
            1. 记录的增删改,事件消息只能是记录的指定字段

         2. Generic Streaming events
            1. 任意事件消息

      2. 二代
         1. Change Data Capture
         2. Platform Events

6. ## Metadata API

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

7. SF访问外部系统的两种方式

   1. REST Http

      1. Remote Site Settings将外部域名添加到SF安全域名中

      2. 写apex代码访问外部系统API

         1. ``` java
            HttpRequest request =new HttpRequest();
            request.setEndpoint('目标Endpoint');
            request.setMethod('GET');
            request.setHeader('Content-Type', 'application/json;charset=UTF-8');
            // Post请求可以添加请求体
            // request.setBody('{"key":"value"}');
            Http htp=new Http();
            HttpResponse response= htp.send(request);
            // 响应状态码
            System.debug(response.getStatusCode());
            // 响应体
            System.debug(response.getBody());
            ```

      3. 测试Callout Apex

         1. 方法一:

            1. 实现HttpCalloutMock接口,并实现public HttpResponse respond(HttpRequest request)方法,返回一个期望的HttpResponse对象

            2. ``` java
               //自定义实现类
               public with sharing class MyHttpCalloutMock implements HttpCalloutMock {
                   public MyHttpCalloutMock() {
                   }
                   public HttpResponse respond(HttpRequest request){
                       HttpResponse response=new HttpResponse();
                       response.setBody('123');
                       response.setStatusCode(201);
                       response.setHeader('Content-Type', 'application/json');
                       return response;
                   }
               }
               ```

            3. ``` java
               //测试调用
               @istest
               static void testMethodName(){
                   Test.setMock(HttpCalloutMock.class, new MyHttpCalloutMock());
                   //Callout业务逻辑调用
                   HttpResponse response;
                   System.debug(response.getBody());
               } 
               ```

         2. 方法二:

            1. 使用StaticResourceCalloutMock,并设置期望的staticResource,statusCode,header

            2. ``` java
               @istest
               static void testMethodName(){
                   StaticResourceCalloutMock mock= new StaticResourceCalloutMock();
                   mock.setStaticResource('StaticResource静态资源');
                   mock.setStatusCode(200);
                   mock.setHeader('Content-Type', 'application/json;charset=UTF-8');
                   Test.setMock(HttpCalloutMock.class, mock);
                   //Callout业务逻辑调用
                   HttpResponse response;
                   System.debug(response.getBody());
               }
               ```

   2. Soap


