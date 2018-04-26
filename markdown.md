# Sublime Text 下 markdown 写作环境的配置

## 实现 Markdown 语法高亮的教程

### 安装 Package Control  
在 Sublime text 中使用 control+` 调出控制台   
输入如下代码，回车安装   

```
import urllib.request,os,hashlib; h = 'eb2297e1a458f27d836c04bb0cbaf282' + 'd0e7a3098092775ccb37ca9d6b2e4b7d'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

### 安装Monokai Extended & Markdown Extended
Shift + Command + P 调出 Command Palette，输入 pci（模糊匹配），找到 Package Control: Install Package，回车;    
分别输入两个插件名称、回车，等待安装；   
点击 Sublime 右下角文档格式，在列表最上方名为 Open all with current extension as 二级列表中选择 Markdown Extended；   
在 Preferences——Color Scheme——Mononkai Extended 下选择一个皮肤   

## 实现实时预览的教程

Shift + Command + P 调出 Command Palette，输入 pci（模糊匹配），找到 Package Control: Install Package，回车;   
找到OmniMarkupPreviewer，回车，安装；   
在 markdown 文件中通过 command+alt+o 调出浏览器实时预览   
通过修改OmniMarkupPreviewer插件配置文件(注意：是修改-User的配置文件，需要复制一份配置到-User配置中)，将第一行   
"server_host": "127.0.0.1"   
修改为    
"server_host": "192.168.1.100"   
192.168.1.100是你本机局域网内的 IP 地址。   
这样就可以实现同一局域网下 不同设备 的实时预览   



