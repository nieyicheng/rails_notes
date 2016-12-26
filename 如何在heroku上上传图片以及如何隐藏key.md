


如何在在 heroku上可以上传图片以及如何隐藏秘钥

1.要安装一个gem叫做 fog

2.新增config/initializers/carrierwave.rb，填入教程里的代码

3.需要有一个空间可以上传图片，可以去AWS注册一个账号 然后到账号下拉选单里面点security Credentials 然后选导览列的Details 可以看到有个 [+Accesss Keys]准备按可以申请key跟secret，然后去s3页面创建一个bucket_name 和选择一个region

4.不要把aws的id与key push到github上面，避免被盗用，可以使用figaro或者heroku config

5.app/uploader/image_uploader.rb 隐藏storage :file 加上 storage :fog会导致本地服务器页面无法预览

6.使用figaro来隐藏秘钥

1. 安装figaro  gem 'figaro' , bundle ,figaro install
   会产生一个config/application.yml的文件,并且会被.gitignore文件自动隐藏。

2. cp config/application.yml conifg/application.yml.example 复制一个例子文件告诉别人你有这个文件

3. 在该文件中加入 

   ```
   production:
     AWS_ACCESS_KEY_ID: "" # 你的 key
     AWS_SECRET_ACCESS_KEY: ""# 你的 secret key
     AWS_BUCKET_NAME: ""# 你的 S3 bucket 的 Region 位置
     REGION: ""# 你設定的 bucket name
   ```

   然后就可以在carrierwave的文件中加入 上面的的名字类似下图。这样就可以保护你的key![Screen Shot 2016-08-10 at 6.37.45 PM](../../../Desktop/Screen Shot 2016-08-10 at 6.37.45 PM.png)

   4. 在 terminal 中输入 `figoro heroku:set -e production`部署到heroku上
   5. 然后在重新commit & push to heroku 这些设定会生效。

