BUG集

1. SalesForce
2. Apex
3. LWC
   1. LWC下载项目到本地的时候,自己千万不要创建lwc名的文件夹,否则将会被认为是lwc路径,导致出错
4. VisualForce
5. API
   1. Rest
   2. Soap
      1. 自定义WS类进行WSDL转jar包时出现类型找不到
         1. 现象:类 Json 位置: 程序包 com.sforce.soap.EmployeeSoapApi ..... 找不到符号
         2. 原因:某些依赖的对象定义没有
         3. 解决方式:从Enterprise WSDL和Partner WSDL中找到相应的对象定义加入自己的xml中
      2. No operation available for request
         1. 现象:No operation available for request {http://soap.sforce.com/schemas/class/EmployeeSoapApi}getAllEmps
         2. 原因:没有设置正确的业务请求Endpoint
         3. 解决方式:connectorConfig.setServiceEndpoint(Connector.END_POINT);
      3. UNKNOWN_EXCEPTION: Destination URL not reset
         1. 现象:UNKNOWN_EXCEPTION: Destination URL not reset. The URL returned from login must be set in the SforceService
         2. 原因:SoapUI请求时没设置正确的请求Endpoint
         3. 解决方式: