# Tomcat部署

方式一，在webapps下直接建立目录，如你想建立一个项目在[http://localhost:8080/FristProject/上访问，你可以直接在这个目录下建立FristProject文件夹；](http://localhost:8080/FristProject/上访问，你可以直接在这个目录下建立FristProject文件夹；)

方式二，在webapps下建立xml文档，如你想建立FristProject这个项目，可以在这个目录下建立FristProject.xml，然后编辑这个文档如下：

Path=””放你想在[http://localhost:8080上要访问这个项目的路径，这里写成/FristProject就是要在http://localhost:8080/FristProject上访问这个项目。](http://localhost:8080上要访问这个项目的路径，这里写成/FristProject就是要在http://localhost:8080/FristProject上访问这个项目。) DocBase=””放你项目文档的本地路径，这里写成了e:\javaweb（这个目录在你重新启动tomcat之前应该是已经存在的）；

