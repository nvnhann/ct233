# ct233
[https://code.visualstudio.com/download](https://code.visualstudio.com/download)

```shell
sudo dpkg -i package_file.deb
```

```git
git init
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
git add .           #Lưu ý có dấu chấm sau add
git commit -m "Initial import of Hello Heroku"
```

Liệt kê danh sách các ứng dụng mình đã tạo:

```heroku apps```

Xem thông tin chi tiết về ứng dụng example

```heroku apps:info --app example```

Xóa ứng dụng example

```heroku apps:destroy --app example```

Thêm một add-on để sử dụng dịch vụ database Postgres cung cấp miễn phí bởi Heroku

```heroku addons:create heroku-postgresql:hobby-dev
heroku pg:wait
```

Xem thông tin về database đã tạo ra cho ứng dụng

```heroku pg:info```

Xem chuỗi kết nối vào database DATABASE_URL từ ứng dụng

```heroku config```

=> Kết qủa DATABASE_URL: postgres://jozemgtezsssog:iSHEmlrZsA96srKxAAEr8W0ywY@ec2-54-75-242-208.eu-west-1.compute.amazonaws.com:5432/dfnugibvvam3o9 , với ý nghĩa như sau:

Username:Password: jozemgtezsssog:iSHEmlrZsA96srKxAAEr8W0ywY

Database server:cồng: ec2-54-75-242-208.eu-west-1.compute.amazonaws.com:5432

Database Name: dfnugibvvam3o9

Tài liệu chi tiết tại: [https://devcenter.heroku.com/articles/heroku-postgresql#provisioning-the-add-on](https://devcenter.heroku.com/articles/heroku-postgresql#provisioning-the-add-on)

```shell
git add  .   # Chú ý dấu chấm .
git commit  -m " Thêm tập tin createtable.php"
git push heroku main
```


