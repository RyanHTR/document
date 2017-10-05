## Ubuntu下百度云安装
一由于所里网络的限制，直接用网页下载可能不稳定，二是有些大文件的上传网页端有限制，故需要ubuntu下的百度云客户端

- 下载https://github.com/Yufeikang/bcloud 解压后cd进目录
- sudo python3 setup.py install --record files.txt #记录安装后文件的路径
- 然后在该目录执行  bcloud-gui

### 备注 

- 卸载: cat files.txt | xargs rm -rf  #删除这些文件
- 执行过bcloud-gui之后就不用在控制台输入了，可以在搜索栏找到它了(第一次必须在命令行输入bcloud-gui，不然直接在搜索栏搜素后点击会没反应)
