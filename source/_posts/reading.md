---
title: 阅读
categories:
- 阅读
tags:
- Reading
---

## 阅读网站

- [Google 学术搜索](https://scholar.google.com/)
- [arXiv](https://arxiv.org/)
- [开源中国](https://www.oschina.net/news)
- [掘金](https://juejin.cn/)
- [trending](https://github.com/trending/javascript?since=daily)
- [Medium](https://medium.com/search?q=web)
- [infoQ](https://www.infoq.cn/)
- [Tutorialspoint](https://www.tutorialspoint.com/index.htm)
- [MDN Web Docs](https://developer.mozilla.org/zh-CN/)
- [小林 coding](https://www.xiaolincoding.com/)

## 知识拓展

### 展望 2024：构建更快、更高效的 Web 体验 ( A faster web in 2024 )

- [A faster web in 2024](https://rviscomi.dev/2023/11/a-faster-web-in-2024/)
- [展望 2024：构建更快、更高效的 Web 体验](https://www.infoq.cn/article/kTLeBxItF3w376Xww8sG)
  - [WPO Stats - Web 性能案例研究](https://wpostats.com/)
  - 优化 Web 性能原因例举
    - 跳出率
    - 购物车大小
    - 转化率
    - 收入
    - 停留时间
    - 页面浏览量
    - 用户满意度
    - 用户留存率
    - 自然搜索流量
    - 品牌认知度
    - 生产力
    - 节省带宽 / CDN
    - 竞争优势
  - 优化 - LCP
    - 优化图像本身，包括使用更高效的图像格式，更长时间地缓存它，将其调整为更小的尺寸等（传统方式）
    - 尽早加载慢速 LCP 图像
      - LCP 图像永远不应该被懒加载
    - 服务器端渲染（有争议）
    - CSS 样式中声明的 LCP 图像 --- 背景图
      - 该用普通的 \<img src="cat.gif"> 元素
      - 声明式预加载
        ```html
        <link rel="preload" as="image" href="cat.gif" />
        ```
    - fetchpriority=high
    - 即时导航：
      - 利用向后/向前缓存和预加载（unload 监听器或 Cache-Control: no-store -> 不符合缓存条件）
        - bfcache -> 之前访问过的页面被保留在内存中，可以立即从历史堆栈中重新访问它们
      - 推测加载 - 用户不曾访问过的页面也可以被预渲染
        - 如果用户有很大可能导航到下一个页面，就应该预先渲染整个页面
        - 支持预取
        - 缺点
          - 只加载文档本身，不加载子资源 -> 比预渲染模式更不太会实现“即时导航”的承诺
      ```js
      // 推测加载示例
      <script type="speculationrules">
        {
          "prerender": [
            {
              "source": "list",
              "urls": ["next3.html", "next4.html"]
            }
          ]
        }
      </script>
      ```
  - 性能评估网站
    - [PageSpeed Insights](https://pagespeed.web.dev/)
    - [Google Search Console - 检测网站性能](https://support.google.com/webmasters/answer/9205520?hl=en)

### 流媒体

- 概念

  - 将一连串的多媒体资料压缩后，经过互联网分段发送资料，在互联网上即时传输影音以供观赏的一种技术与过程

    > 流媒体在播放前并不下载整个文件，而仅仅是将开始的部分内容存入内存。

    > 流媒体的数据流随传随播，仅在开始时有一些延迟。

- 流媒体实现的关键技术
  - 流式传输
    - 实时流
    - 顺序流
- 流媒体传输协议

### 数字人（虚拟数字人）

- 概念
  - 存在于非物理世界中，由计算机手段创造及使用，并具有多重人类特征（外貌特征、人类表演、交互能力等）的综合产物
- 虚拟数字人通用系统框架
  - 人物形象
  - 语音生成
  - 动画生成
  - 音视频合成显示
  - 交互
