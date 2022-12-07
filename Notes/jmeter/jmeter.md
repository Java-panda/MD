### JMETER

1. 安装
   1. 官网下载
   2. 解压到本地磁盘
   3. bin目录打开jmeter.bat启动程序

2. 预备知识

   1. HTTP

      1. General

         1. Request URL: https://www.baidu.com/s?wd=jmeter%E5%BD%95%E5%88%B6%E5%B7%A5%E5%85%B7&rsv_spt=1&rsv_iqid=0x9c400eef000dbf7c&issp=1&f=8&rsv_bp=1&rsv_idx=2&ie=utf-8&tn=baiduhome_pg&rsv_enter=1&rsv_dl=tb&rsv_sug3=19&rsv_sug1=17&rsv_sug7=101&rsv_sug2=0&rsv_btype=i&inputT=4801&rsv_sug4=5647
         2. Request Method: GET
         3. Status Code: 200 OK
         4. Remote Address: 119.63.197.139:443
         5. Referrer Policy: strict-origin-when-cross-origin

      2. Request Headers

         1. Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
         2. Accept-Encoding: gzip, deflate, br
         3. Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7,ko;q=0.6,ja;q=0.5,zh-TW;q=0.4
         4. Cache-Control: no-cache
         5. Connection: keep-alive
         6. Cookie: ****
         7. Host: www.baidu.com
         8. Pragma: no-cache
         9. sec-ch-ua: "Google Chrome";v="89", "Chromium";v="89", ";Not A Brand";v="99"
         10. sec-ch-ua-mobile: ?0
         11. Sec-Fetch-Dest: document
         12. Sec-Fetch-Mode: navigate
         13. Sec-Fetch-Site: same-origin
         14. Sec-Fetch-User: ?1
         15. Upgrade-Insecure-Requests: 1
         16. User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.82 Safari/537.36

      3. Response Headers

         1. Bdpagetype: 3
         2. Bdqid: 0x89e59570000db13d
         3. Cache-Control: private
         4. Ckpacknum: 2
         5. Ckrndstr: 0000db13d
         6. Connection: keep-alive
         7. Content-Encoding: gzip
         8. Content-Type: text/html;charset=utf-8
         9. Date: Fri, 23 Apr 2021 15:05:59 GMT
         10. P3p: CP=" OTI DSP COR IVA OUR IND COM "
         11. Server: BWS/1.1
         12. Set-Cookie: BDRCVFR[feWj1Vr5u3D]=mk3SLVN4HKm; path=/; domain=.baidu.com
         13. Set-Cookie: delPer=0; path=/; domain=.baidu.com
         14. Set-Cookie: BD_CK_SAM=1;path=/
         15. Set-Cookie: PSINO=7; domain=.baidu.com; path=/
         16. Set-Cookie: BDSVRTM=27; path=/
         17. Set-Cookie: H_PS_PSSID=33764_33820_31660_33849_33676_33894_33810; path=/; domain=.baidu.com
         18. Strict-Transport-Security: max-age=172800
         19. Traceid: 161919035903779770989936512461132640573
         20. Transfer-Encoding: chunked
         21. Vary: Accept-Encoding
         22. X-Ua-Compatible: IE=Edge,chrome=1

      4. 状态码

         | 状态码 | 含义           |
         | ------ | -------------- |
         | 2**    | 成功           |
         | 3**    | 重定向         |
         | 4**    | 请求错误       |
         | 5**    | 服务器内部错误 |

3. 项目结构

   1. Test Plan(项目根目录)
      1. Thread Group(一个线程组)
         1. Http Request(一个请求)
            1. PreProcessors(请求前置处理器)
            2. PostProcessors(请求后置处理器)
         2. View Results Tree(结果树不同格式显示)
            1. Text
            2. HTML
            3. XML
            4. JSON
            5. Xpath
            6. RegExp
            7. CSS Selector

4. 语法规则

   1. 变量引用
      1. ${变量名}
   2. 提取器
      1. Xpath
         1. //table//th
         2. Match:
            1. 0:随机
            2. -1:全部
            3. n:第n个匹配结果
      2. RegExp
         1. "title":"(.*?)"
         2. Template:i
            1. 匹配到的第i组
            2. 0代表整体匹配的值,n代表第n个括号的匹配值
         3. Match:
            1. 0:随机
            2. -1:全部
            3. n:第n个匹配结果
      3. JSON
         1. data.list[2].title
         2. Match:
            1. 0:随机
            2. -1:全部
            3. n:第n个匹配结果