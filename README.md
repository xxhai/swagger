# swagger

实在不想写文档..
目前只做了一个TP5版本的swagger 自动文档生成
##在controller里面加入以下代码

```php
 public function doc()
    {

        $path=dirname(dirname(dirname(__FILE__)));  //用文件绝对路径取application路径
        $project_path=$path.'\index\controller';//你想要哪个文件夹下面的注释生成对应的API文档
        $swagger_json_path=dirname($path).'\public\static\swagger-docs\swagger.json';//需要生成swagger.json的路径
        $swagger = \Swagger\scan($project_path);
        // header('Content-Type: application/json');
        // echo $swagger;
        $res = file_put_contents($swagger_json_path, $swagger);
        if ($res == true) {
            $this->redirect('http://'.$_SERVER['HTTP_HOST'].'/dist/index.html');//swagger-ui放在public下面的这个路径里面了
        }else{
            return 'swagger.json写入失败！';
        }

    }
```
###$project_path 这里的 路径需要自己手动设置（需要生成文档的项目的路径）
###$swagger_json_path 可以不用配置 默认设置在\public\static下 swagger-docs文件夹需要自己建（不然找不到可能报错）

##swagger-ui 里面的dist 文件夹 移动到 public下面

<br>
如果是public 目录为访问目录的话 在
.htaccess 文件  加入以下代码 否则可能会是找不到模块，实际是被重写拦截了 
<br>
RewriteCond $1 !^(static|swagger-docs|upload)
<br>

还有swagger-ui dist文件夹里面index.html 需要修改的内容
```javascript
window.onload = function() {
        var urlx = document.location.protocol+'//'+document.domain+'/static/swagger-docs/swagger.json';
     // Build a system
      const ui = SwaggerUIBundle({
          url:urlx,
        dom_id: '#swagger-ui',
        deepLinking: true,
        presets: [
          SwaggerUIBundle.presets.apis,
          SwaggerUIStandalonePreset
        ],
        plugins: [
          SwaggerUIBundle.plugins.DownloadUrl
        ],
        layout: "StandaloneLayout"
      })

      window.ui = ui
```

大概就是这样了
