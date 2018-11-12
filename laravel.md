```
安装composer
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
composer中文镜像，参考地址：http://pkg.phpcomposer.com/
全局镜像
composer config -g repo.packagist composer https://packagist.phpcomposer.com
局部镜像
composer config repo.packagist composer https://packagist.phpcomposer.com

推荐的库
https://packagist.org/packages/prettus/l5-repository
https://packagist.org/packages/zizaco/entrust
https://github.com/Zizaco/confide-mongo
https://packagist.org/packages/leroy-merlin-br/mongolid
https://packagist.org/packages/barryvdh/laravel-debugbar
https://packagist.org/packages/laravelcollective/html
https://packagist.org/packages/jenssegers/mongodb
https://packagist.org/packages/voku/simple_html_dom
https://packagist.org/packages/laracasts/generators
https://packagist.org/packages/nesbot/carbon
https://packagist.org/packages/guzzlehttp/guzzle
https://packagist.org/packages/fabpot/goutte
https://github.com/laracasts/Laravel-5-Generators-Extended
https://github.com/jenssegers/date
https://github.com/RoumenDamianoff/laravel-feed
https://packagist.org/packages/mews/captcha
https://github.com/kevinkhill/lavacharts
https://github.com/hernandev/hipchat-laravel
https://github.com/GrahamCampbell/Laravel-Flysystem
#非常方便的方案来判断导航元素的 active 状态
https://github.com/letrunghieu/active
#让 Laravel 支持 FTP 操作
https://github.com/harishanchu/Laravel-FTP   
https://packagist.org/packages/encore/laravel-admin
https://packagist.org/packages/cviebrock/eloquent-sluggable
https://packagist.org/packages/laracasts/flash
https://packagist.org/packages/qxsch/worker-pool
https://packagist.org/packages/nmred/kafka-php

https://github.com/laravel-notification-channels/bearychat
https://packagist.org/packages/laravel/horizon
https://github.com/lokielse/omnipay-wechatpay
https://github.com/xiaohuilam/LuoCaptcha

https://github.com/acacha/adminlte-laravel
http://git.razko.nl/InlineAttachment/
https://packagist.org/packages/appstract/laravel-opcache

https://packagist.org/packages/xethron/migrations-generator
https://github.com/etrepat/baum
https://github.com/antonioribeiro/tracker


#高性能千万级定时任务管理服务
https://github.com/snower/forsun-laravel

最好用的markdown编辑器
composer require chenhua/laravel5-markdown-editor

页面压缩
https://github.com/renatomarinho/laravel-page-speed

composer require intervention/image
#记录用户行为扩展包
https://docs.spatie.be/laravel-activitylog/v1/introduction

https://github.com/chongyi/swoole-laravel-framework
https://github.com/garveen/laravoole

https://github.com/kitetail/zttp

https://github.com/tymondesigns/jwt-auth

https://github.com/PHP-FFmpeg/PHP-FFmpeg/
#消息队列管理
https://horizon.laravel.com/

https://packagist.org/packages/jacobcyl/ali-oss-storage
https://github.com/nWidart/laravel-modules

安装lararvel安装器
composer global require "laravel/installer"


https://github.com/Cherry-Pie/websocket


ron:check-movie-third-url

https://github.com/morozovsk/websocket-examples

创建项目
laravel new blog
或者
composer create-project laravel/laravel blog "5.1.*"

修改项目名称
php artisan app:name Horsefly

php artisan key:generate

临时关闭/打开项目
php artisan up/down

创建控制器
php artisan make:controller HomeController
php artisan make:controller Admin/AdminHomeController

php artisan route:cache
php artisan route:clear


//创建一个console
php artisan make:command  SendEmails

//创建一个控制台命令
php artisan make:command  FooCommand

php artisan make:model User
php artisan make:model Models\\User
php artisan make:model Models\\Category
php artisan make:model Models\\Video
php artisan make:model Models\\VideoPackage
php artisan make:model Models\\Product
php artisan make:model Models\\User

php artisan make:model Models/Admin -m

//创建模型，同时创建controller和Migration
php artisan make:model Models/User -crm


//运行内建server
php artisan serve --port 8080
http://localhost:8080/

$ip = filter_var($originIp, FILTER_VALIDATE_IP);


Incoming机器人

php artisan make:migration create_users_table
php artisan make:migration create_users_table --create=users
php artisan migrate:refresh --seed
php artisan migrate
php artisan migrate:refresh
php artisan migrate --force

php artisan make:seeder UserTableSeeder
php artisan db:seed
php artisan db:seed --class=UserTableSeeder


php artisan make:job UpdateDayProgram

php artisan make:migration:schema create_users_table --schema="username:string, email:string:unique"
php artisan make:migration:schema create_wares_table --schema="slug:string:unique,name:string,cover:string,qr:string,playtime:string,cost_price:float,now_price:float,created_user:string"
php artisan make:migration:schema create_tags_table --schema="name:string:unique,cover:string,created_user:string"
php artisan make:migration:schema create_interests_table --schema="name:string,cover:string,created_user:string"
php artisan make:migration:schema create_schedules_table --schema="date:string,ware_id:integer:foreign,start_time:date,end_time:date,tag:string,sort_no:integer:default(0),created_user:string"
php artisan make:migration:schema create_products_table --schema="name:string:unique,cover:string,cost_price:float:default(0),now_price:float:default(0),created_user:string"

username:string
body:text
age:integer
published_at:date
excerpt:text:nullable
email:string:unique:default('foo@example.com')


php artisan queue:listen
php artisan queue:listen connection
php artisan queue:listen --timeout=60
php artisan queue:listen --sleep=5





php artisan make:provider EpgJsonServiceProvider

DB::connection()->

注释服务的使用
php artisan event:scan
php artisan route:scan
php artisan model:scan
样例
@Get("/", as="index", middleware="guest")
@Get("/profiles/{id}", as="profiles.show", middleware="guest", domain="foo.com", where={"id": "[0-9]+"})
@Post, @Options, @Put, @Patch, @Delete, @Any
@Middleware("guest")
在整个控制上
@Middleware("guest", except={"logout"})
@Resource('users')
@Resource('users', only={"index", "show"})
@Resource('users', names={"index"="user.all", "show"="user.view"})
@Controller(prefix="admin", domain="foo.com")


wget https://bootstrap.pypa.io/ez_setup.py -O - | python
或者
yum install python-setuptools
easy_install supervisor
echo_supervisord_conf > /etc/supervisord.conf
  
yum -y install supervisor
vi /etc/supervisord.conf
[program:laravel-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /hwdata/www/SpEpg/artisan queue:work redis --sleep=3 --tries=3 --daemon
autostart=true
autorestart=true
user=root
numprocs=8
redirect_stderr=true
stdout_logfile=/hwdata/logs/epg_worker.log

修改完配置，reread
如果修改了 /etc/supervisord.conf ,,需要执行 supervisorctl reload 来重新加载配置文件
$ supervisorctl reread
$ supervisorctl update
$ supervisorctl start laravel-worker:*
restart laravel-worker:*

启动服务
$ supervisord

WEB管理界面
http://127.0.0.1:9001

Cache::get('key')
Cache::has('key')
Cache::put('key', 'value', $minutes)
Cache::pull('key')
Cache::add('key', 'value', $minutes)
Cache::forever('key', 'value')
Cache::forget('key')

======== 路由 ==========
//简单路由
Route::get('/', function () {
    return view('welcome');
});
//使用路由名称
Route::any('/test', ['as' => 'rTest', function () {
    return "<h1>HELLO</h1>";
}]);
//使用request，需要引入Request类
//use Illuminate\Http\Request;
Route::any('/test', ['as' => 'rTest', function (Request $request) {
    $name = $request->input("name");
    return "<h1>HELLO, $name</h1>";
}]);
//参数正则
Route::get('/user/{id}', function ($id) {
    return "UserId:" . $id;
})->where('id', '[0-9]+');
//多个参数
Route::get('/user/{id}/{name}', function ($id, $name) {
    return "UserId:" . $id . "UserName:" . $name;
})->where(['id' => '[0-9]+', 'name' => '[a-z]+']);
//路由到Controller
Route::get('/user/profile', [
    'as' => 'profile', 'uses' => 'UserController@showProfile'
]);
//group中使用prefix
Route::group(['prefix' => 'admin', 'namespace' => 'Admin', 'middleware' => 'auth'], function()
{
    Route::get('/user', [
    	'as' => 'admin_user', 'uses' => 'AdminUserController@index'
    ]);
     Route::get('/comment', [
    	'as' => 'admin_comment', 'uses' => 'AdminCommentController@index'
    ]);
    //资源控制器
    Route::resource('pages', 'PagesController');
});
//使用controller
Route::controller('users', 'UserController');
//使用controller，并配置名称
Route::controller('users', 'UserController', [
    'anyLogin' => 'user.login',
]);
对应的controller,需要符合一定的规范
public function getIndex()
public function postProfile()
public function anyLogin()
//控制器行为包含多个字词
UserController 中的 getAdminProfile() 对应的地址为 users/admin-profile


======== 控制器 ==========
指向控制器行为的 URL
action('App\Http\Controllers\FooController@method');

获取正在执行的控制器行为名称
$action = Route::currentRouteAction();

控制器中使用中间件
public function __construct()
{
		$this->middleware('auth');
		$this->middleware('log', ['only' => ['fooAction', 'barAction']]);
		$this->middleware('subscribed', ['except' => ['fooAction', 'barAction']]);
}

$name = $request->input('name');
$name = Request::input('name', 'Sally');
Request::has('name')
$input = Request::all();
$input = Request::input('products.0.name');

将输入数据存成一次性 Session
Request::flash();

将部分输入数据存成一次性 Session
Request::flashOnly('username', 'email');
Request::flashExcept('password');

快闪及重定向
return redirect('form')->withInput();
return redirect('form')->withInput(Request::except('password'));

取得旧输入数据
$username = Request::old('username');
在模板中使用
{{ old('username') }}

取得 Cookie 值
$value = Request::cookie('name');

取得上传文件
$file = Request::file('photo');
确认文件是否有上传
if (Request::hasFile('photo'))
{
    //
}
移动上传的文件
Request::file('photo')->move($destinationPath);
Request::file('photo')->move($destinationPath, $fileName);

取得请求 URI
$uri = Request::path();

判断一个请求是否使用了 AJAX
if (Request::ajax())
{
    //
}
取得请求方法
$method = Request::method();
if (Request::isMethod('post'))
{
    //
}
确认请求路径是否符合特定格式
if (Request::is('admin/*'))
{
    //
}

Debugbar::info($object);
Debugbar::error('Error!');
Debugbar::warning('Watch out…');
Debugbar::addMessage('Another message', 'mylabel');

======== 视图 ===============


php artisan make:provider MogileFsServiceProvider
php artisan make:provider GridfsServiceProvider


重点框架
https://github.com/lucadegasperi/oauth2-server-laravel/wiki
https://github.com/Zizaco/entrust

```
