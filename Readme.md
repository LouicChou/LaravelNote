# Build Multi Vendor Ecommerce Website (2023)

<p>Use Laragon be host. </p>
<p>Laragon是整合Apache跟mysql的服務程式，方便開發PHP程式</p>

## Change PHP Version
<p>PHP版本下載: https://windows.php.net/downloads/releases/</p>
<p>將要換的PHP版本程式解壓縮後放在C:\laragon\bin\php之下，
在Laragon程式中點右鍵>PHP，即可切換版本</p>


## VS Code useful extension
- Laravel Extension pack(整合包)

    Uninstall - EditConfig for vscode, Laravel Create view, Laravel Blade wrapper

- One Dark Pro

## Install Laravel
<a href='https://laravel.com/docs/10.x/installation'>Install Laravel</a>

<p>Laragon點擊下方Terminal輸入</p>

    install Laravel tool : Enter "composer global require laravel/installer"

<p>再輸入</p>

    create new Project: Enter "laravel new [Project_Name]"

## open project
<p>Laragon中點右鍵>www，可看到所有建立的專案</p>

## 切換預設網站
<p>Laragon中點右鍵>第二個項目>Switch Document Root>Select another，選到index網頁的那個路徑，像是Laravel專案就是選[Public]資料夾</p>

## folder structure
- [app]
    - [http]    
        - [Controllers]
        - [Middleware]
        - kernal.php - Middleware中的東西都會註冊在此檔案
    - [Models] - 檢索資料與資料庫通訊
    - [Provider] - 提供者的服務，Router, Event之類的
- [bootstrap] - 基本上不用動
    - [cache] - 系統會放一些暫存文件用於優化
- [config] - 常用
- [database]
    - [seeder] - 假數據庫
    - [migrations] - 重要! 編修資料表
- [public] - 開放檔案，如首頁index.php
- [resource]
    - [css]
    - [js]
    - [views] - 所有HTML程式都在放在這
- [routes] - 所有路由都放在這
- [storage] - 舊版本檔案?
- [test] - 測試檔案可放在這
- [vendor] - 所有laravel依賴庫(連結外部工具庫Libraries)
- .env - 環境變數，放資料庫連接字串或密碼之類的東西
- artisan - 
- composer.json - 所有庫之間關係的內容
- package.json - 有安裝的NPM會在這

## artisan
查看artisan命令 - 在Cmder(terminal)輸入"php artisan"

常用命令:
- "php artisan serve" - 查看laragon伺服器的狀態與ip跟port，8000是laragon的預設PORT
- 所有命令後面加上"-h"就可以查看系統功能指令有哪些，譬如輸入"php artisan serve -h"
- "php artisan route:list" - 顯示所有應用程式的路由列表
- "php artisan tinker" - 進入php的即時運算功能，像是輸入"7 == "7"",會得到Ture的結果，輸入"strlen("ABCDE")"會得到5，可用來在網站運行中創建假數據測試之類的

## Terminal指令
### 查看所有Route

    php artisan route:list


## VS code的PHP錯誤
<p>錯誤訊息:</p> 
    Cannot validate since a PHP installation could not be found. Use the setting 'php.validate.executablePath' to configure the PHP executable.

<br>
<p>將C:\Users\user\AppData\Roaming\Code\User\settings.json中"php.validate.executablePath"的值設定為使用中的PHP版本執行檔路徑</p>

<p>ex:</p>

    C:\\laragon\\bin\\php\\php-8.1.10-Win32-vs16-x64\\php.exe"

## Routes

<p>在[routes]>web.php中定義</p>

### 基本語法為`Route::get(URL,方法);`
<p>ex:</p>

    Route::get('about',function(){
        return "<h1>About Page</h1>;
    })

### 加上參數寫法
    Route::get('about/{id}',function($id){
       return $id;
    });

<p>進入此網頁URL: "Localhost/about/12"</p>

### 使用別名
<p>避免URL太長，可使用此方式</p>
<p>先定義有別名的路由</p>

    Route::get('about', function () {
        return "<h1>About Page</h1>";
    })->name('hello');

<p>以別名方式製作的路由</p>

    Route::get('home',function(){
    return "<a href='".route('hello')."'>home</a>";
    });

<p>進入此網頁URL: "Localhost/home"，進入之後按下連結就會進入about網頁</p>

### 使用別名套用參數
<p>定義使用帶參數的路由別名</p>

    Route::get('contant/{id}',function($id){
    return $id;
    })->name('edit-contant');
<p>寫死帶入1</p>

    Route::get('contant',function(){
        return "<a href='".route('edit-contant',1)."'>about contant</a>";
    });

<p>進入此網頁URL: "Localhost/contant"</p>

### 路由群組
<p>可以在程式碼中省略相同的根目錄customer
或是有其他設計需求，相同的子網頁可以直接複製，只更換根目錄名稱，類似Model的概念</p>

    route::group(['prefix' => 'customer'],function(){
        Route::get('/', function () {
            return "<h1>customer list</h1>";
        });
        
        Route::get('/create', function () {
            return "<h1>customer create</h1>";
        });
        
        Route::get('/show', function () {
            return "<h1>customer show</h1>";
        });
    });
<p>進入此網頁URL: "Localhost/customer"
"Localhost/customer/create"
"Localhost/customer/show"</p>

### Fallback Route
<p>若輸入錯的URL不希望出現404網頁而是指定別的網頁</p>

    route::fallback(function (){
        return "Route not exist";
    });
<p>此段一定要放在web.php的最下面才會生效</p>

## View
<p>給使用者看的網頁都會放在[resouces]>[views]裡</p>

<p>routes預設語法</p>

    Route::get('/', function () {
    return view('welcome');
    });

<p>指的是連接到[resouces]>[views]裡的welcome.blade.php，這就是View()的功能</p>
<p>所以可以在[views]裡建一個名為about.blade.php的網頁，然後在路由的web.php中加入以下語法</p>

    Route::get('/about', function () {
        return view('about');;
    });

<p>URL輸入"http://localhost/about"即可進入</p>

<p>檔案名稱的blade是讓laravel可以在HTML中使用Laravel功能，純PHP若要在PHP中使用PHP語法要加上<?php>，這個Blade可以用"@"開頭來表示此段是寫laravel語法，像是"@if".</p>

### 網頁放在資料夾中
<p>假設在[Views]中新增一個[contant]資料夾，裡面有一個index.blade.php網頁的呼叫方式如下</p>

    Route::get('/contant', function () {
        return view('contant.index');;
    });

### 變數帶入View
<p>web.app輸入以下代碼</p>

    Route::get('/about', function () {
    $about = 'This is about page';
    $about2 = 'This is about two';
    return view('about',compact('about','about2'));;
    });
<p>[view]的about.blade.php輸入以下代碼</p>

    <h1>{{$about}}</h1>
    <h1>{{$about2}}</h1>

### 使用樣板
<p>完成檔案結構如下</p>

![迴圈條件](img/useTemplateStructure.jpg)
<p>在[views]創建home.blade.php檔案，內容輸入如下</p>

    @extends('layouts.master') {{-- 主樣板 --}}

    @section('content') {{-- master.blade.php中master.blade.php的內容 --}}
        <main role="main" class="container">
            <h1 class="mt-5 text-danger">Home</h1>
            Lorem,ipsum dolor
        </main>
    @endsection

<p>在[views]下建立資料夾[layouts]，再建立master.blade.php，程式碼內容如下</p>

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Home</title>
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet">
    </head>
    <body>
        @include('layouts.header')
            @yield('content')

        @include('layouts.footer')
    
        <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.2/dist/umd/popper.min.js" ></script>
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.min.js"></script>
    </body>
    </html>

<p>在[layouts]建立header.blade.php，程式碼內容如下</p>

    <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container-fluid">
        <a class="navbar-brand" href="#">Navbar</a>
        <button class="navbar-togger" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupporedContent">
            <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                <li class="nav-item">
                    <a class="nav-link active" aria-current="page" href="#">Home</a>
                </li>

                <li class="nav-item">
                    <a class="nav-link" href="#">link</a>
                </li>
            </ul>
        </div>
    </div>
    </nav>

<p>在[layouts]建立footer.blade.php，程式碼內容如下</p>

    <footer class="bg-light text-center text-lg-start" style="position: fixed; width: 100%; bottom:0;">
    <div class="text-center p-3" style="background-color: rgba(0,0,0,0.2);">
        <a class="text-dark" href="#">WebSolutionUs</a>
    </div>
</footer> 

<p>在web.app建立一個指向home的Route，如下:</p>

    Route::get('/home', function () {
    return view('home');;
    });

### 使用Foreach帶出條件式內容
<p>在Route帶入變數內容如下:</p>

    Route::get('/home', function () {
        $blogs = [
            [
                'title' => 'title one',
                'body' => 'this is body text',
                'status' => '1'
            ],
            [
                'title' => 'title two',
                'body' => 'this is body text',
                'status' => '0'
            ],
            [
                'title' => 'title three',
                'body' => 'this is body text',
                'status' => '1'
            ],
            [
                'title' => 'title four',
                'body' => 'this is body text',
                'status' => '1'
            ]
        ];
        return view('home', compact('blogs'));
    });

<p>在home.blase.php輸入程式碼如下:</p>

    @extends('layouts.master') {{-- 主樣板 --}}

    @section('content')
    <main role="main" class="container">
        <h1 class="mt-5 text-danger">Home</h1>
        Lorem,ipsum dolor

        <div class="row mt-5">
            @foreach ($blogs as $blog)
            @if ($blog['status'] == 1)
                <div class="col-md-4">
                    <div class="card">
                        <div class="card-body">
                            <h2>{{$blog['title']}}</h2>
                            <p>{{$blog['body']}}</p>
                        </div>
                    </div>
                </div>
            @else
                <div class="col-md-4">
                    <div class="card">
                        <div class="card-body">
                            <h2>{{$blog['title']}}</h2>
                            <p>{{$blog['body']}}</p>
                            <div class="btn-sm btn-warning">Pending</div>
                        </div>
                    </div>
                </div>
            @endif
            @endforeach
    </main>
    @endsection

<p>結果如下</p>

![迴圈條件](img/foreachCondition.jpg)

## Controller
### Create controller
<p>開啟Terminal後進入專案路徑

![開啟Termital](img/openTerminal.jpg)
![進入專案](img/intoProject.jpg)
<p>輸入指令:</p>

    php artisan make:controller HomeController

![創建成功](img/createSuccessed.jpg)

<p>單動作控制器</p>

    php artisan make:controller HomeController --invokable

<p>資源控制器</p>

    php artisan make:controller HomeController -r

## 表單提交Use Token(使用csrf)
### View內容
- form元素屬性要加method="POST" action="{{ route('login.sumbit') }}" 
- form元素下面那行要加一行@csrf
- input元素都要加上name屬性


        <div class="row mt-5 justify-content-center">
            <div class="col-md-4">
                <h2 class="mb-4">Login</h2>
                <div class="card">
                    <div class="card-body">
                        <form action="{{ route('login.sumbit') }}" method="POST">
                            @csrf
                            <div class="mb-2">
                                <label for="" class="form-label">User Name</label>
                                <input name="name" type="text" class="form-control">
                            </div>
                            <div class="mb-2">
                                <label for="" class="form-label">User Password</label>
                                <input name="password" type="text" class="form-control">
                            </div>
                            <div class="mb-2">
                                <label for="" class="form-label">User Email</label>
                                <input name="email" type="email" class="form-control">
                            </div>
                            <button type="submit" class="btn btn-primary">Sumbit</button>
                        </form>
                    </div>
                </div>
            </div>
        </div>

### Controller內容
- 參數要加入Request，此處為提交後回傳的內容

    namespace App\Http\Controllers;
    use Illuminate\Http\Request;

    class LoginController extends Controller
    {
        public function handleLogin(Request $req)
        {
            dd($req->all());
        }
    }

### web.php
- 新增一個跟View相同名稱的POST方法

    Route::get('/login', [LoginController::class, 'index'])->name('login');

    Route::post('/login', [LoginCon troller::class, 'handleLogin'])->name('login.sumbit');

### app>Http>Middleware>VerifyCsrfToken.php
<p>要確認內容為空</p>

![進入專案](img/VerifyCsrfTokenEmpty.jpg)

## Form驗證Validation
### 提交內容加validate

    public function handleLogin(Request $req)
    {
        $req->validate([
            // 多條件驗證:必填，只能是英文字元，至少六個字元
            'name' => ['required', 'alpha', 'min:6', 'max:10'],
            'password' => 'required',
            'email' => ['required', 'email']
        ]);

        return $req;
    }

### View中在form元素前加入以下程式碼

    {{-- 驗證有任何錯誤就出現提示 --}}
            @if ($errors->any())
                @foreach ($errors->all() as $error)
                    <div class="alert alert-danger">{{ $error }}</div>
                @endforeach
            @endif

### 自定義驗證錯誤訊息
<p>加上第二個[]參數</p>

    {
        $req->validate([
            // 多條件驗證:必填，只能是英文字元，至少六個字元
            'name' => ['required', 'alpha', 'min:6', 'max:10'],
            'password' => 'required',
            'email' => ['required', 'email']
        ],[
            'name.required' => 'The name field is required!!',
            'name.alpha' => '只能是英文'
        ]);

        return $req;
    }

### 建立class製作驗證內容與自定義訊息
- 新增request

        php artisan make:request LoginRequest

    <p>app>http多一個Request資料夾，下面多一個LoginRequest.php</p>

- 將裡面的authorize方法內容由flase改成true
- 將驗證項目跟自定義訊息都放到LoginRequest.php

    <p>最後LoginRequest.php內容如下:

        namespace App\Http\Requests;

        Illuminate\Foundation\Http\FormRequest;

        class LoginRequest extends FormRequest
        {
        /**
        * Determine if the user is authorized to make this request.
        */
            public function authorize(): bool
            {
                return true;
            }

            /**
            * Get the validation rules that apply to the request.
            *
            * @return array<string, \Illuminate\Contracts\Validation\ValidationRule|array<mixed>|string>
            */
            public function rules()
            {
                return [
                    'name' => ['required', 'alpha', 'min:6'],
                    'password' => 'required',
                    'email' => ['required', 'email']
                ];
            }

            public function messages()
            {
                return [
                    'name.required' => 'The name field is required!!',
                    'name.alpha' => '只能是英文'
                ];
            }
        }

    <p>LoginController.php內容如下

        namespace App\Http\Controllers;

        use App\Http\Requests\LoginRequest;

        class LoginController extends Controller
        {
            public function index()
            {
                return view('login');
            }

            public function handleLogin(LoginRequest $req)
            {
                return $req;
            }
        }

## Database
- config>database.php中預設是使用mysql

    ![database_default](img/database_default.jpg)

- 在.env輸入資料庫資訊

    ![db_connection](img/db_conntection.jpg)

### 建立實例
- Terminal輸入

    php artisan tinker

    ![tinker](img/tinker.jpg)

- terminal輸入
    DB::connection()->getPDO();

    <p>得到mysql的實例</p>

    ![getPDO](img/getPDO.jpg)

### 建立Migration檔案方便遷移資料庫

### 範例建立posts資料表(僅創php檔)
- Terminal輸入

    php artisan make:migration create_posts_table

    <p>須以'create_'開頭，'_table'結束，忠鈿建議定義負數名稱(+s)

- 可在database>migrations資料夾下看到新建的table.php

    ![migration_newTable](img/migration_newTable.jpg)

- 建立自定義欄位

        public function up(): void
        {
            Schema::create('posts', function (Blueprint $table) {
                $table->id();
                $table->string('title');
                $table->text('description');
                $table->boolean('status');
                $table->timestamps();
            });
        }


    ![migration_newTable](img/defined_col.jpg)

- terminal輸入:

        php artisan migrate

    <p>產生database>migrations內所有的資料表(其中有預設資料表)

    ![migration_newTable](img/migrate_table.jpg)

    <p>查看資料庫內容也會出現資料表</p>

    ![newly_added_db](img/newly_added_db.jpg)

- 更多資料類型與用法

        $table->string('title', 100); // 255
        $table->decimal('amount')->nullable(); // 可空白
        $table->double('double')->default(0); // 預設值
        $table->float('float');
        $table->integer('integer');

### 更多migration命令
- 重置現有的資料庫(刪除所有資料表只剩下資料表migrations)

        php artisan migrate:reset

- 新增欄位(不會刪除舊資料)

    * 新增一個要新增欄位的資料表同名的add table(以'add_'開頭，'_table'結尾)

            php artisan make:migration add_posts_table --table=posts

    ![add_col](img/add_col.jpg)

    * 在這個add檔案加入要新增的欄位(因為已有舊資料在表內，所以新的欄位要加上nullable()或是default())

    ![dd_col_defined](img/add_col_defined.jpg)

    * 執行migrate

            php artisan migrate



- 新增欄位(<font color=#FF0000>**會刪除資料內容!!**</font> php artisan migrate只會新增目前沒有的資料表，不會新增資料表內的新欄位)

        php artisan migrate:refresh

- 回到上一次的資料庫狀態(可能新建的資料表會消失,不會復歸新增的欄位)

        php artisan migrate:rollback

- 只遷移單一資料表(指定資料表的php路徑)

        php artisan migrate --path=/database/migtations/2024_03_17_044156_create_posts_table.php

### 創建假資料
- 創建假資料用的php檔，檔名建議跟資料表名稱相關

        php  artisan make:seeder PostSeeder

    <p>在database>seeders資料夾中就產生檔案</p>

![create_seeder](img/create_seeder.jpg)

- 輸入要新增資料內容(迴圈產生10筆)


        for ($i = 0; $i <= 10; $i++) {
            DB::table('posts')->insert([
                'title' => Str::random(20),
                'description' => Str::random(30),
                'status' => 1,
                'Publish_date' => date('Y-m-d'),
                'user_id' => 1,
            ]);
        }

    ![fakeData](img/fakeData.jpg)

- 在DatabaseSeeder.php新增呼叫

        $this->call(PostSeeder::class);

    ![callSeeder](img/callSeeder.jpg)

- 執行

        php artisan db:seed

    ![callSeederResult](img/callSeederResult.jpg)

### Query builder
- 在controller回傳DB搜尋結果

    return DB::table('posts')->get();    //all data
    return DB::table('posts')->find(7);  //id=7
    return DB::table('posts')->first(); //第一筆
    // 只搜尋最多2個欄位(相同資料只會有一筆)
    return DB::table('posts')->pluck('id', 'title', 'user_id');
    return DB::table('posts')->where('id', '>', 3)->where('id', '<', 8)->get();

    ![callSeederResult](img/controller_get.jpg)

- insert 

        DB::table('posts')->insert([
            [
                'title' => 'This is one',
                'description' => 'des 1',
                'status' => 1,
                'Publish_date' => date('Y-m-d'),
                'user_id' => 1
            ],
            [
                'title' => 'This is two',
                'description' => 'des 2',
                'status' => 0,
                'Publish_date' => date('Y-m-d'),
                'user_id' => 2
            ]
        ]);

- update

        DB::table('posts')->where('id', 12)->update([
            'title' => 'This is one updated',
            'description' => 'des 1 updated'
        ]);

- delete

        DB::table('posts')->where('id', 12)->delete();

- join

    ![categories_tb](img/categories_tb.jpg)

    ![posts_tb](img/posts_tb.jpg)

    <p>搜尋條件[posts]的category_id=[categories]的id，兩個資料表全部欄位</p>

        return DB::table('posts')->join('categories', 'posts.category_id', '=', 'categories.id')
            ->get();

    ![join_result01](img/join_result01.jpg)

    <p>只要顯示categories的欄位</p>

        return DB::table('posts')->join('categories', 'posts.category_id', '=', 'categories.id')
            ->select('categories.*')
            ->get();

    ![posts_tb](img/join_result02.jpg)

- 使用聚集Aggregates

    <p>得到資料筆數</p>

        return DB::table('posts')->count();

    <p>欄位內容總和</p>

        return DB::table('posts')->sum('id');