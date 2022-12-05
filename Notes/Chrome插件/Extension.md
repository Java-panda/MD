- 插件结构

  - HTML
    - popup页面
    - optionals页面
  - CSS
  - JavaScript
    - service worker
      - 定义
        - 伺服工人
      - 作用
        - 处理并监听浏览器事件
      - 特点
        - 可以使用所有chrome api
        - 不能直接和页面内容交互
    - Content scripts
      - 定义
        - 内容脚本
      - 作用
        - 操作dom页面内容
      - 特点
        - 只能使用部分chrome api
        - 但是可以通过消息实现全chrome api的调用
  - images
    - 图标
    - 图片
  - manifest.json

- API种类

  - 所有JS API
  - Chrome API

- 基本功能

- 核心功能

  - Message passing(消息传递)

    - 分类

      - 一次性消息

        - content js→popup/options

          请求

          ```js
          (async() => {
              const response = await chrome.runtime.sendMessage({ key: "ausername" });
              console.log(response.value);
          })();
          ```

          监听

          ```js
          chrome.runtime.onMessage.addListener(
              function(request, sender, sendResponse) {
                  const key = request.key;
                  chrome.storage.sync.set({ key: "test" }, function() {});
                  chrome.storage.sync.get(key, function(result) {
                      console.log(result);
                      console.log(sender.tab ? "from a content script:" + sender.tab.url : "from the extension");
                      sendResponse({ value: result[key] });
                  });
              }
          );
          ```

        - popup/options→content js

      - 长期消息

      - 跨扩展插件消息传递