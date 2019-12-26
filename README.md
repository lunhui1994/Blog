# Material X

Hexo 说明文档


### hexo 命令
1. 创建文章:
> hexo new [layout] <title>
2. 生成静态文件HTML
> hexo generate
   监听生成静态文件HTML
> hexo generate -w
3. 上传至github
> hexo deploy


### 目录说明: 
    (可配置: 根目录文件 _config.yml 中配置source_dir)
    1. 资源存放位置：'/source/' 下,地址：'/'; 例如 图片存放位置/source/img/xxx.png ,引用地址: /img/xxx.png
    2. 文章存放位置: '/source/_posts/'; 生成的文章在public中.

