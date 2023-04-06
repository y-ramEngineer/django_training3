# Django構築トレーニング

## トレーニング用のディレクトリを作成する
```
cd /Users/takumi/training
mkdir conf
```

## PyCharmを起動する
PyCharmを起動して下記ディレクトリを開く
```
/Users/takumi/training/conf
```
main.pyが作成されたら削除する

## 仮想環境を構築する
```
cd conf
python3 -m venv env
```

### 仮想環境に入る
```
source env/bin/activate
```

### Djangoをインストールする
```
pip install django
```

## プロジェクトをスタートする
```
django-admin startproject conf
```

## ディレクトリ名を変更する
```
mv conf django-backend
```

## ディレクトリを移動する
```
cd django-backend
```

## サーバーを起動する
```
python3 manage.py runserver
```

## サーバーにアクセスする
[ローカルホスト](http://localhost:8000)にアクセスする

## サーバーを停止する
Ctrl-cを押下する

## アプリをスタートする
```
python3 manage.py startapp sites
```

## INSTALLED_APPSにアプリを追加する
settings.pyのINSTALLED_APPSに下記アプリ名を追加する
```
'sites',
```

## Homebrewがインストールされているか確認する
```
brew doctor
```

## Homebrewをインストールする
※インストールされていない場合
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## PostgreSQLがインストールされているか確認する
```
psql --version
```

## postgreSQLをインストールする
※インストールされていない場合
```
brew install postgresql@14
```

## インストールされたバージョンを確認する
※インストールした場合
```
psql --version
```

## PostgreSQLを起動する
```
brew services start postgresql@14
```

## PostgreSQLの起動を確認する
```
psql postgres
```

## PostgreSQLから抜ける
```
\q
```

## データベースの一覧を表示する
```
psql -l
```

## recreate.sqlを作成する
recreate.sqlを作成する
```
drop database masaara;
CREATE DATABASE "massara";
```

## データベースを作り直す
```
psql -h localhost -p 5432 -U takumi -d postgres -f recreate.sql
```

## pgAdminをインストールする
※インストールされていない場合
```
brew install pgadmin4
```

## データベースを設定する
conf/django-backend/settings.py
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'massara',
        'USER': 'takumi',
        'PASSWORD': '',
        'HOST': 'localhost',
        'PORT': 5432,
    }
}
```

## プロジェクトのurls.pyファイルを設定する
conf/django-backend/conf/urls.py
```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('sites.urls')),
]
```

## アプリのurls.pyファイルを作成する
conf/django-backend/sites/urls.py
```
from django.urls import path
from .views import BlogList

urlpatterns = [
    path('list/', BlogList.as_view()),
]
```

## views.pyを定義する
conf/django-backend/sites/views.py
```
from django.views.generic import ListView
from .models import BlogModel

# Create your views here.
class BlogList(ListView):
    template_name = 'list.html'
    model = BlogModel
```

## モデルを定義する
conf/django-backend/sites/models.py
```
from django.db import models

# Create your models here.
class BlogModel(models.Model):
    title = models.CharField(max_length=100)
```

## モデルを管理画面に追加する
conf/django-backend/sites/admin.py
```
from django.contrib import admin
from .models import BlogModel

# Register your models here.
admin.site.register(BlogModel)
```

## テンプレートのディレクトリを定義する
osをimportする
conf/django-backend/conf/settings.py
```
import os
```

TEMPLATESのDIRSを変更する  
conf/django-backend/conf/settings.py
```
'DIRS': [os.path.join(BASE_DIR, 'templates')],
```

## テンプレートを作成する
conf/django-backend/templates/list.html
```
{% for item in object_list %}
  <ul>
    <li>{{ item.title}}</li>
  </ul>
{% endfor %}
```

### ライブラリをインストールする
```
pip install psycopg2
pip install psycopg
```

## makemigrationsとmigrateを実行する
```
python3 manage.py makemigrations
python3 manage.py migrate
```

## サーバー起動する
```
python3 manage.py runserver
```

## ブラウザ起動する
[ローカルホスト](http://localhost:8000/list)にアクセスする

## サーバーを停止する
Ctrl-cを押下する

## 管理画面のユーザーを作成する
```
python manage.py createsuperuser

username：takumi
Email address：
Password：1
```

## サーバー起動する
```
python3 manage.py runserver
```

## 管理画面にログインする
[管理画面](http://localhost:8000/admin)にアクセスしてログインする  
モデルが定義されていることを確認する

## モデルを追加する
モデルを押下して「ADD BLOG MODEL」ボタンを押下する

## ブラウザ起動する
[ローカルホスト](http://localhost:8000/list)にアクセスする  
追加したモデルが表示されることを確認する

## Gitにアクセスする
[Git](https://github.com/)にアクセスする

## リポジトリを作成する
1. 左上のTop RepositoriesのNewボタンを押下する
2. Repository Nameを「django-training」と入力する
3. 「Create repository」ボタンを押下する。

## READMEを作成する
confディレクトリにREADME.mdを作成する  
README.mdに本手順をコピペする

## .gitignoreを作成する
conf/.gitignore
```
__pycache__/
env
.idea
```

## Gitに登録する
```
git init
git branch -M main
git add .
git commit
git remote add origin git@github.com:y-ramEngineer/django_training.git
git push -u origin main
```

## コミット時にエラーが出た場合の対処
コミット時に下記エラーが出た場合
```
Author identity unknown

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: unable to auto-detect email address (got 'takumi@mba.(none)')
```

下記のコマンドを打つ
```
git config --global user.email "takumi.daayoshi@mineo.jp"
git config --global user.name "y-ramEngineer"
```

---
## 参考サイト
- https://mashimashi.net/skill/db/postgresql/745/
- https://ai-trend.jp/programming/db/postgre-mac-install/
- https://www.ashisuto.co.jp/db_blog/article/how-use-postgresql-django.html
- https://fclout.hateblo.jp/entry/2020/06/30/%E3%80%90%E8%A7%A3%E6%B1%BA%E3%80%91sphinx-build%E6%99%82%E3%81%AB%E3%80%8Cdoesn%27t_declare_an_explicit_app_label_and_isn%27t_in_an_application_in_INSTALLED_APPS_%E3%80%8D%E3%81%A8%E8%A8%80%E3%82%8F
- https://geniusium.hatenablog.com/entry/2021/09/13/233328
- https://www.ni4.jp/2021/09/04-141000.html
- https://amateur-engineer.com/postgresql-mac-install/
- https://qiita.com/inabe49/items/16ee3d9d1ce68daa9fff

pgadminのマスターパスワードは1

## 使用したコマンドの履歴
1. mkdir training  
1003. cd tra  
1004. cd training  
1005. ls  
1006. django-admin startproject conf  
1007. ls  
1008. cd conf  
1009. ls  
1010. python manage.py startapp sites  
1011. ls  
1012. cd sites  
1013. ls  
1014. cd ..  
1015. ls  
1016. psql -h localhost -p 5432 -U root -d postgres -f recreate.sql  
1017. sudo yum install postgresql  
1018. homebrew  
1019. brew  
1020. brew search postgresql  
1021. brew doctor  
1022. xcode-select --install  
1023. brew search postgresql  
1024. brew install postgresql  
1025. psql --version  
1026. /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"  
1027. xcode-select --install  
1028. xcode-select --update  
1029. /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"  
1030. brew doctor  
1031. brew search postgresql  
1032. postgresql@9.6: Invalid bottle tag symbol  
1033. brew -v  
1034. brew install postgresql  
1035. psql --version  
1036. brew -v  
1037. brew install postgresql  
1038. brew search postgresql  
1039. brew doctor  
1040. brew cleanup  
1041. brew doctor  
1042. brew search postgresql  
1043. /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"  
1044. brew search postgresql  
1045. brew install postgresql  
1046. brew doctor  
1047. xcode-select -install  
1048. xcode-select --install  
1049. ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"  
1050. ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"  
1051. ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"  
1052. ruby -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"  
1053. brew doctor  
1054. brew doctor  
1055. opensll version  
1056. openssl version  
1057. brew update  
1058. brew doctor  
1059. brew reinstall openssl  
1060. brew link --force openssl  
1061. brew doctor  
1062. echo 'export PATH=$(brew --prefix openssl)/bin:$PATH' >> ~/.zshrc  
1063. source ~/.zshrc  
1064. openssl version  
1065. brew doctor  
1066. /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"  
1067. brew install openssl  
1068. which openssl  
1069. brew search postgresql  
1070. brew doctor  
1071. brew search postgresql  
1072. psql -v  
1073. psql --version  
1074. ls  
1075. pwd  
1076. cd ..  
1077. ls  
1078. cd co  
1079. cd conf  
1080. ls  
1081. cd conf  
1082. ls  
1083. cd ..  
1084. cd sites  
1085. ls  
1086. cd ..  
1087. ls  
1088. ls  
1089. cd ..  
1090. ls  
1091. cd conf  
1092. python3 -m venv sample  
1093. source sample/bin/actiate  
1094. ls  
1095. source sample/bin/activate  
1096. python  
1097. pip freeze  
1098. pip install django  
1099. ls  
1100. python3 manage.py makemigrations  
1101. ls  
1102. python3 manage.py migrate  
1103. python manage.py createsuperuser  
1104. python3 manage.py runserver  
1105. python3 manage.py runserver  
1106. pip freeze  
1107. python3 manage.py runserver  
1108. python3 manage.py makemigrations  
1109. python3 manage.py migrate  
1110. ls  
1111. python3 manage.py makemigrations  
1112. python3 manage.py runserver  
1113. python3 manage.py runserver  
1114. python3 manage.py runserver  
1115. python3 manage.py runserver  
1116. python3 manage.py runserver  
1117. python3 manage.py runserver  
1118. python3 manage.py runserver  
1119. python3 manage.py makemigrations  
1120. python3 manage.py migrate  
1121. python3 manage.py runserver  
1122. python3 manage.py makemigrations  
1123. python3 manage.py migrate  
1124. python3 manage.py runserver  
1125. python3 manage.py runserver  
1126. brew search postgresql  
1127. brew search postgresql==8.6  
1128. brew install postgresql  
1129. brew install postgresql@14  
1130. psql --version  
1131. brew search postgresql  
1132. ls  
1133. cd ..  
1134. ls  
1135. cd conf  
1136. pwd  
1137. ls  
1138. psql -h localhost -p 5432 -U root -d postgres -f recreate.sql  
1139. psql --version  
1140. brew services start postgresql  
1141. brew services start postgresql@14  
1142. brew services list  
1143. psql -l  
1144. psql postgres  
1145. ls  
1146. brew services list  
1147. \l  
1148. psql postgres  
1149. psql -h localhost -p 5432 -U root -d postgres -f recreate.sql  
1150. psql postgres  
1151. psql -h localhost -p 5432 -U takumi -d postgres -f recreate.sql  
1152. touch recreate.sql  
1153. vi recreate.sql  
1154. psql -h localhost -p 5432 -U takumi -d postgres -f recreate.sql  
1155. psql -h localhost -p 5432 -U takumi -d postgres -f recreate.sql  
1156. python3 manage.py runser  
1157. python3 manage.py runserver  
1158. history  
1159. python3 manage.py makemigrations  
1160. python3 manage.py makemigrations  
1161. pip install psycopg2  
1162. pip install psycopg  
1163. python3 manage.py makemigrations  
1164. python3 manage.py migrate  
1165. python3 manage.py runserver  
1166. deactivate  
1167. brew cask install pgadmin4  
1168. brew --cask install pgadming4  
1169. brew --cask install pgadmin4  
1170. brew install pgadmin4  
1171. postgres -D /usr/local/var/postgres  
1172. psql postgres  
1173. ls  
1174. cd ..  
1175. ls  
1176. cd conf  
1177. echo "# django_training" >> README.md  
1178. git init  
1179. git branch  
1180. git add README.md  
1181. git commit -m "first commit"  
1182. git branch -M main  
1183. git remote add origin git@github.com:y-ramEngineer/django_training.git  
1184. git push -u origin main  
1185. git add .  
1186. git status  
1187. git commit  
1188. git branch  
1189. git push origin main  
1190. history  
1191. ls  
1192. cd ..  
1193. pwd  
1194. ls  
1195. cd conf  
1196. ls  
1197. cd conf  
1198. ls  
1199. cd ..  
1200. ls  