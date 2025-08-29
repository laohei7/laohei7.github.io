# 启动Search功能

Writeside 本身没有搜寻功能，但可以利用 Algolia search 来做搜寻。

## Algolia search

首先参照官方说明文件来注册 Algolia 帐号，并取得 API key。[Algolia search](https://www.jetbrains.com/help/writerside/algolia-search.html)

1. 注册 [Algolia](https://www.algolia.com/) 帐号，并取得 API key。
2. 登入后应该会有预设的应用程式，如果没有就新增一个，地区目前不支援亚洲。 [App](https://dashboard.algolia.com/account/applications)
3. 在 Algolia 应用程式页面，点选 Data sources | Indices，然后点选 Create Index。
![aligolia_page.png](aligolia_page.png){ style="inline" width="800"}
4. 提供一个有意义的名称并建立索引。
5. 在索引页面，点选 Configuration | Facets，然后点选 Attributes for faceting 下的 Add an attribute。
![aligolia_index_page.png](aligolia_index_page.png){ style="inline" width="800"}
6. 新增两个属性：product 和 version。 
7. 点选 Review and Save Settings 并确认。 
8. 在左下角点 Settings 到设定页面，点选 API Keys。
![aligolia_key.png](aligolia_key.png){ style="inline" width="800"}
9. 然后找到 Application ID, Search API Key, Admin API Key，这些资讯会用在 Writeside 的设定。
![aligolia_key_2.png](aligolia_key_2.png){ style="inline" width="800"}

## Add search to config

继续参照 [官方文件](https://www.jetbrains.com/help/writerside/algolia-search.html#add-search-to-config) 来设定 [buildprofiles.xml](https://github.com/laohei7/laohei7.github.io/blob/main/Writerside/cfg/buildprofiles.xml#L14)

```xml
<build-profile instance="hi">
    <variables>
        <algolia-id>YourAppId</algolia-id>
        <algolia-index>YourIndexName</algolia-index>
        <algolia-api-key>YourSearchApiKey</algolia-api-key>
    </variables>  
</build-profile>
```

## Github Action

1. 到 Github repo 的 Settings | Secrets，新增以下变数：
   * ALGOLIA_KEY: YourAdminApiKey
2. 参考官方文件 [Upload search indexes](https://www.jetbrains.com/help/writerside/deploy-docs-to-github-pages.html#search) 建立 [.github/workflows/deploy.yml](https://github.com/laohei7/laohei7.github.io/blob/main/.github/workflows/build-docs.yml#L17) 并确保以下设定有被正确设置
```YAML
ALGOLIA_ARTIFACT: 'algolia-indexes-HI.zip'
ALGOLIA_APP_NAME: 'YourAppId'
ALGOLIA_INDEX_NAME: 'YourIndexName'
ALGOLIA_KEY: '${{ secrets.ALGOLIA_KEY }}'
CONFIG_JSON_PRODUCT: 'writerside'
CONFIG_JSON_VERSION: 'master'
```

> 注意事项
> 
> * CONFIG_JSON_PRODUCT 需要与 [buildprofiles.xml](https://github.com/laohei7/laohei7.github.io/blob/main/Writerside/cfg/buildprofiles.xml#L10) 的 `<build-profile instance="hi">` 的 instance 设定一致。
> * CONFIG_JSON_VERSION 需要与 [writerside.cfg](https://github.com/laohei7/laohei7.github.io/blob/main/Writerside/writerside.cfg#L10) 的 `instance.version` 设定一致。
> 
>否则会导致 Algolia search Filter product与 version 条件不符而搜寻不到。

## 结果

完成后，就可以在 Writeside 网站右上角点放大镜来使用搜寻功能了。

![aligolia_result_page.png](aligolia_result_page.png)
