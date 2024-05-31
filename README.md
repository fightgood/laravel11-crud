# Laravel 11 CRUD Application with Oracle

Hi, In this tutorial, I will show you step by step Laravel 11 crud operation example.

CRUD Meaning: CRUD is an acronym that comes from the world of computer programming and refers to the four functions that are considered necessary to implement persistent storage in your application: create, read, update, and delete.

We will create a product CRUD application using Laravel 11 in this example. We will create a products table with name and detail columns using Laravel 11 migration. Then, we will create routes, a controller, views, and model files for the product module. We will use Bootstrap 5 for design. So, let's follow the steps below to create CRUD operations with Laravel 11.

Step for Laravel 11 CRUD Operation Example
------------------------------------------

*   **Step 1:** Install Laravel 11
*   **Step 2:** Install Oracle DB driver for Laravel via OCI8
*   **Step 3:** Oracle Database Configuration
*   **Step 4:** Create Migration
*   **Step 5:** Create Form Request Validation Class
*   **Step 6:** Create Controller and Model
*   **Step 7:** Add Resource Route
*   **Step 8:** Update AppServiceProvider
*   **Step 9:** Add Blade Files
*   **Run Laravel App**

**Step 1: Install Laravel 11**
------------------------------

First of all, we need to get a fresh Laravel 11 version application using the command below because we are starting from scratch. So, open your terminal or command prompt and run the command below:

```
composer create-project laravel/laravel example-app
```

**Step 2: Install Oracle DB driver for Laravel via OCI8**
---------------------------------------------------------

<a href="https://github.com/yajra/laravel-oci8" target="_blank">Laravel-OCI8</a> is an Oracle Database Driver package for Laravel. Laravel-OCI8 is an extension of Illuminate/Database that uses OCI8 extension to communicate with Oracle. So, open your terminal or command prompt and run the command below:

```
composer require yajra/laravel-oci8:^11
```


**Step 3: Oracle Database Configuration**
----------------------------------------

Add a Oracle connection with the database name, username, and password to the \`.env\` file.

.env

```
# Oracle Connection
DB_CONNECTION=oracle
DB_HOST=localhost
DB_PORT=1521
DB_SERVICE_NAME=orcl
DB_DATABASE=orcl
DB_USERNAME=sudee
DB_PASSWORD=oracle
```

config/database.php like this:

```
<?php

declare(strict_types=1);

use Illuminate\Support\Str;

return [
  
  /*
  |--------------------------------------------------------------------------
  | Default Database Connection Name
  |--------------------------------------------------------------------------
  |
  | Here you may specify which of the database connections below you wish
  | to use as your default connection for database operations. This is
  | the connection which will be used unless another connection
  | is explicitly specified when you execute a query / statement.
  |
  */
  
  'default' => env('DB_CONNECTION', env('DB_CONNECTION', 'sqlite')),
  
  /*
  |--------------------------------------------------------------------------
  | Database Connections
  |--------------------------------------------------------------------------
  |
  | Below are all the database connections defined for your application.
  | An example configuration is provided for each database system that
  | is supported by Laravel. You're free to add / remove connections.
  |
  */
  
  'connections' => [
    
    'sqlite' => [
      'driver'                  => 'sqlite',
      'url'                     => env('DB_URL'),
      'database'                => env('DB_DATABASE', database_path('database.sqlite')),
      'prefix'                  => '',
      'foreign_key_constraints' => env('DB_FOREIGN_KEYS', true),
    ],
    
    'mysql' => [
      'driver'         => 'mysql',
      'url'            => env('DB_URL'),
      'host'           => env('DB_HOST', '127.0.0.1'),
      'port'           => env('DB_PORT', '3306'),
      'database'       => env('DB_DATABASE', 'laravel'),
      'username'       => env('DB_USERNAME', 'root'),
      'password'       => env('DB_PASSWORD', ''),
      'unix_socket'    => env('DB_SOCKET', ''),
      'charset'        => env('DB_CHARSET', 'utf8mb4'),
      'collation'      => env('DB_COLLATION', 'utf8mb4_unicode_ci'),
      'prefix'         => '',
      'prefix_indexes' => true,
      'strict'         => true,
      'engine'         => null,
      'options'        => extension_loaded('pdo_mysql') ? array_filter([PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),]) : [],
    ],
    
    'mariadb' => [
      'driver'         => 'mariadb',
      'url'            => env('DB_URL'),
      'host'           => env('DB_HOST', '127.0.0.1'),
      'port'           => env('DB_PORT', '3306'),
      'database'       => env('DB_DATABASE', 'laravel'),
      'username'       => env('DB_USERNAME', 'root'),
      'password'       => env('DB_PASSWORD', ''),
      'unix_socket'    => env('DB_SOCKET', ''),
      'charset'        => env('DB_CHARSET', 'utf8mb4'),
      'collation'      => env('DB_COLLATION', 'utf8mb4_unicode_ci'),
      'prefix'         => '',
      'prefix_indexes' => true,
      'strict'         => true,
      'engine'         => null,
      'options'        => extension_loaded('pdo_mysql') ? array_filter([PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),]) : [],
    ],
    
    'pgsql' => [
      'driver'         => 'pgsql',
      'url'            => env('DB_URL'),
      'host'           => env('DB_HOST', '127.0.0.1'),
      'port'           => env('DB_PORT', '5432'),
      'database'       => env('DB_DATABASE', 'laravel'),
      'username'       => env('DB_USERNAME', 'root'),
      'password'       => env('DB_PASSWORD', ''),
      'charset'        => env('DB_CHARSET', 'utf8'),
      'prefix'         => '',
      'prefix_indexes' => true,
      'search_path'    => 'public',
      'sslmode'        => 'prefer',
    ],
    
    'sqlsrv' => [
      'driver'         => 'sqlsrv',
      'url'            => env('DB_URL'),
      'host'           => env('DB_HOST', 'localhost'),
      'port'           => env('DB_PORT', '1433'),
      'database'       => env('DB_DATABASE', 'laravel'),
      'username'       => env('DB_USERNAME', 'root'),
      'password'       => env('DB_PASSWORD', ''),
      'charset'        => env('DB_CHARSET', 'utf8'),
      'prefix'         => '',
      'prefix_indexes' => true,
      // 'encrypt' => env('DB_ENCRYPT', 'yes'),
      // 'trust_server_certificate' => env('DB_TRUST_SERVER_CERTIFICATE', 'false'),
    ],
    
    'oracle' => [
      'driver'         => 'oracle',
      'tns'            => env('DB_TNS', ''),
      'host'           => env('DB_HOST', ''),
      'port'           => env('DB_PORT', '1521'),
      'database'       => env('DB_DATABASE', ''),
      'service_name'   => env('DB_SERVICE_NAME', ''),
      'username'       => env('DB_USERNAME', ''),
      'password'       => env('DB_PASSWORD', ''),
      'charset'        => env('DB_CHARSET', 'AL32UTF8'),
      'prefix'         => env('DB_PREFIX', ''),
      'prefix_schema'  => env('DB_SCHEMA_PREFIX', ''),
      'edition'        => env('DB_EDITION', 'ora$base'),
      'server_version' => env('DB_SERVER_VERSION', ''),
      'load_balance'   => env('DB_LOAD_BALANCE', 'yes'),
      'dynamic'        => [],
    ],
  
  ],
  
  /*
  |--------------------------------------------------------------------------
  | Migration Repository Table
  |--------------------------------------------------------------------------
  |
  | This table keeps track of all the migrations that have already run for
  | your application. Using this information, we can determine which of
  | the migrations on disk haven't been run on the database.
  |
  */
  
  'migrations' => [
    'table'                  => 'migrations',
    'update_date_on_publish' => true,
  ],
  
  /*
  |--------------------------------------------------------------------------
  | Redis Databases
  |--------------------------------------------------------------------------
  |
  | Redis is an open source, fast, and advanced key-value store that also
  | provides a richer body of commands than a typical key-value system
  | such as Memcached. You may define your connection settings here.
  |
  */
  
  'redis' => [
    
    'client' => env('REDIS_CLIENT', 'phpredis'),
    
    'options' => [
      'cluster' => env('REDIS_CLUSTER', 'redis'),
      'prefix'  => env('REDIS_PREFIX', Str::slug(env('APP_NAME', 'laravel'), '_').'_database_'),
    ],
    
    'default' => [
      'url'      => env('REDIS_URL'),
      'host'     => env('REDIS_HOST', '127.0.0.1'),
      'username' => env('REDIS_USERNAME'),
      'password' => env('REDIS_PASSWORD'),
      'port'     => env('REDIS_PORT', '6379'),
      'database' => env('REDIS_DB', '0'),
    ],
    
    'cache' => [
      'url'      => env('REDIS_URL'),
      'host'     => env('REDIS_HOST', '127.0.0.1'),
      'username' => env('REDIS_USERNAME'),
      'password' => env('REDIS_PASSWORD'),
      'port'     => env('REDIS_PORT', '6379'),
      'database' => env('REDIS_CACHE_DB', '1'),
    ],
  
  ],

];

```


**Step 4: Create Migration**
----------------------------

In the third step, we will create a "products" table with "name" and "details" columns using Laravel migration. So, let's use the following command to create a migration file:

```
php artisan make:migration create_products_table --create=products
```


After executing the above command, you will find a file in the following path: "database/migrations". You have to put the code below in your migration file to create the products table.

```
<?php
  
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;
  
return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('products', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->text('detail');
            $table->timestamps();
        });
    }
  
    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('products');
    }
};
```


Now you have to run this migration by the following command:

```
php artisan migrate
```


**Step 5: Create Form Request Validation Class**
------------------------------------------------

In this step, we will create a form request validation class for the \`store()\` and \`update()\` methods in the controller. In this class, we will define validation rules and use it in the controller file. So, let's create it.

```
php artisan make:request ProductStoreRequest
```


Just put the below code in your request class:

app/Http/Requests/ProductStoreRequest.php

```
<?php
  
namespace App\Http\Requests;
  
use Illuminate\Foundation\Http\FormRequest;
  
class ProductStoreRequest extends FormRequest
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
     * @return array|string>
     */
    public function rules(): array
    {
        return [
            'name' => 'required',
            'detail' => 'required'
        ];
    }
}
```


Now, let's do the same thing for the Update Request Class.

```
php artisan make:request ProductUpdateRequest
```


Just put the below code in your request class:

app/Http/Requests/ProductUpdateRequest.php

```
<?php
   
namespace App\Http\Requests;
  
use Illuminate\Foundation\Http\FormRequest;
  
class ProductUpdateRequest extends FormRequest
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
     * @return array|string>
     */
    public function rules(): array
    {
        return [
            'name' => 'required',
            'detail' => 'required'
        ];
    }
}
```


**Step 6: Create Controller and Model**
---------------------------------------

In this step, now we should create a new resource controller named ProductController. So run the below command to create the new controller. Below is the controller for creating the resource controller.

```
 artisan make:controller ProductController --resource --model=Product
```


After the following command, you will find a new file at this path: "app/Http/Controllers/ProductController.php".

In this controller, seven methods will be created by default as follows:

1)index()

2)create()

3)store()

4)show()

5)edit()

6)update()

7)destroy()

So, let's copy the code below and put it in the ProductController.php file.

app/Http/Controllers/ProductController.php

```
<?php

declare(strict_types=1);

namespace App\Http\Controllers;

use App\Http\Requests\ProductStoreRequest;
use App\Http\Requests\ProductUpdateRequest;
use App\Models\Product;
use Illuminate\Http\RedirectResponse;
use Illuminate\View\View;

/**
 *
 */
class ProductController extends Controller
{
  /**
   * Display a listing of the resource.
   */
  public function index(): View
  {
    $products = Product::latest()->paginate(5);
    
    return view('products.index', compact('products'))
      ->with('i', (request()->input('page', 1) - 1) * 5);
  }
  
  /**
   * Store a newly created resource in storage.
   */
  public function store(ProductStoreRequest $request): RedirectResponse
  {
    Product::create($request->validated());
    
    return redirect()->route('products.index')
      ->with('success', 'Product created successfully.');
  }
  
  /**
   * Show the form for creating a new resource.
   */
  public function create(): View
  {
    return view('products.create');
  }
  
  /**
   * Display the specified resource.
   */
  public function show(Product $product): View
  {
    return view('products.show', compact('product'));
  }
  
  /**
   * Show the form for editing the specified resource.
   */
  public function edit(Product $product): View
  {
    return view('products.edit', compact('product'));
  }
  
  /**
   * Update the specified resource in storage.
   */
  public function update(ProductUpdateRequest $request, Product $product): RedirectResponse
  {
    $product->update($request->validated());
    
    return redirect()->route('products.index')
      ->with('success', 'Product updated successfully');
  }
  
  /**
   * Remove the specified resource from storage.
   */
  public function destroy(Product $product): RedirectResponse
  {
    $product->delete();
    
    return redirect()->route('products.index')
      ->with('success', 'Product deleted successfully');
  }
}

```

So, let's update the Product model code as follows:

app/Models/Product.php

```
<?php

declare(strict_types=1);

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

/**
 *
 */
class Product extends Model
{
  use HasFactory;
  
  protected $fillable
    = [
      'name',
      'detail',
    ];
}

```


**Step 6: Add Resource Route**
------------------------------

Here, we need to add a resource route for the product CRUD application. So, open your \`routes/web.php\` file and add the following route.

routes/web.php

```
<?php

declare(strict_types=1);

use App\Http\Controllers\ProductController;
use Illuminate\Support\Facades\Route;

Route::get('/', [ProductController::class, 'index']);
Route::resource('products', ProductController::class);

```


**Step 8: Update AppServiceProvider**
-------------------------------------

Here, we will use bootstrap 5 for pagination. so, we need to import it on AppServiceProvider.php file. let's update it.

app/Provides/AppServiceProvider.php

```
<?php
  
namespace App\Providers;
  
use Illuminate\Support\ServiceProvider;
use Illuminate\Pagination\Paginator;
  
class AppServiceProvider extends ServiceProvider
{
    /**
     * Register any application services.
     */
    public function register(): void
    {
           
    }
  
    /**
     * Bootstrap any application services.
     */
    public function boot(): void
    {
        Paginator::useBootstrapFive();
    }
}

```


**Step 9: Add Blade Files**
---------------------------

In the last step, we need to create only blade files. So, primarily, we have to create a layout file and then a new folder called "products." After that, we create blade files for the CRUD app. So, finally, you have to create the following blade files below:

1) layout.blade.php

2) index.blade.php

3) create.blade.php

4) edit.blade.php

5) show.blade.php

So let's just create the following file and put the code below in it.

resources/views/products/layout.blade.php

```
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Laravel 11 CRUD Application with Oracle</title>
  <link
    href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css"
    rel="stylesheet"
  >
  <link
    rel="stylesheet"
    href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css"
  />
</head>
<body>

<div class="container">
  @yield('content')
</div>

</body>
</html>
```


resources/views/products/index.blade.php

```
@extends('products.layout')

@section('content')
  
  <div class="card mt-5">
    <h2 class="card-header">Laravel 11 CRUD Application</h2>
    <div class="card-body">
      
      @session('success')
      <div
        class="alert alert-success"
        role="alert"
      > {{ $value }} </div>
      @endsession
      
      <div class="d-grid gap-2 d-md-flex justify-content-md-end">
        <a
          class="btn btn-success btn-sm"
          href="{{ route('products.create') }}"
        > <i class="fa fa-plus"></i> Create New Product</a>
      </div>
      
      <table class="table table-bordered table-striped mt-4">
        <thead>
        <tr>
          <th style="width:80px;">No</th>
          <th>Name</th>
          <th>Details</th>
          <th style="width:250px;">Action</th>
        </tr>
        </thead>
        
        <tbody>
        @forelse ($products as $product)
          <tr>
            <td>{{ ++$i }}</td>
            <td>{{ $product->name }}</td>
            <td>{{ $product->detail }}</td>
            <td>
              <form
                action="{{ route('products.destroy',$product->id) }}"
                method="POST"
              >
                
                <a
                  class="btn btn-info btn-sm"
                  href="{{ route('products.show',$product->id) }}"
                ><i class="fa-solid fa-list"></i> Show</a>
                
                <a
                  class="btn btn-primary btn-sm"
                  href="{{ route('products.edit',$product->id) }}"
                ><i class="fa-solid fa-pen-to-square"></i> Edit</a>
                
                @csrf
                @method('DELETE')
                
                <button
                  type="submit"
                  class="btn btn-danger btn-sm"
                ><i class="fa-solid fa-trash"></i> Delete
                </button>
              </form>
            </td>
          </tr>
        @empty
          <tr>
            <td colspan="4">There is no data.</td>
          </tr>
        @endforelse
        </tbody>
      
      </table>
      
      {!! $products->links() !!}
    
    </div>
  </div>
@endsection

```


resources/views/products/create.blade.php

```
@extends('products.layout')

@section('content')
  
  <div class="card mt-5">
    <h2 class="card-header">Add New Product</h2>
    <div class="card-body">
      
      <div class="d-grid gap-2 d-md-flex justify-content-md-end">
        <a
          class="btn btn-primary btn-sm"
          href="{{ route('products.index') }}"
        ><i class="fa fa-arrow-left"></i> Back</a>
      </div>
      
      <form
        action="{{ route('products.store') }}"
        method="POST"
      >
        @csrf
        
        <div class="mb-3">
          <label
            for="inputName"
            class="form-label"
          ><strong>Name:</strong></label>
          <input
            type="text"
            name="name"
            class="form-control @error('name') is-invalid @enderror"
            id="inputName"
            placeholder="Name"
          >
          @error('name')
          <div class="form-text text-danger">{{ $message }}</div>
          @enderror
        </div>
        
        <div class="mb-3">
          <label
            for="inputDetail"
            class="form-label"
          ><strong>Detail:</strong></label>
          <textarea
            class="form-control @error('detail') is-invalid @enderror"
            style="height:150px;"
            name="detail"
            id="inputDetail"
            placeholder="Detail"
          ></textarea>
          @error('detail')
          <div class="form-text text-danger">{{ $message ?? null }}</div>
          @enderror
        </div>
        <button
          type="submit"
          class="btn btn-success"
        ><i class="fa-solid fa-floppy-disk"></i> Submit
        </button>
      </form>
    
    </div>
  </div>
@endsection

```


resources/views/products/edit.blade.php

```
@extends('products.layout')

@section('content')
  
  <div class="card mt-5">
    <h2 class="card-header">Edit Product</h2>
    <div class="card-body">
      
      <div class="d-grid gap-2 d-md-flex justify-content-md-end">
        <a
          class="btn btn-primary btn-sm"
          href="{{ route('products.index') }}"
        ><i class="fa fa-arrow-left"></i> Back</a>
      </div>
      
      <form
        action="{{ route('products.update',$product->id) }}"
        method="POST"
      >
        @csrf
        @method('PUT')
        
        <div class="mb-3">
          <label
            for="inputName"
            class="form-label"
          ><strong>Name:</strong></label>
          <input
            type="text"
            name="name"
            value="{{ $product->name }}"
            class="form-control @error('name') is-invalid @enderror"
            id="inputName"
            placeholder="Name"
          >
          @error('name')
          <div class="form-text text-danger">{{ $message }}</div>
          @enderror
        </div>
        
        <div class="mb-3">
          <label
            for="inputDetail"
            class="form-label"
          ><strong>Detail:</strong></label>
          <textarea
            class="form-control @error('detail') is-invalid @enderror"
            style="height:150px;"
            name="detail"
            id="inputDetail"
            placeholder="Detail"
          >{{ $product->detail }}</textarea>
          @error('detail')
          <div class="form-text text-danger">{{ $message }}</div>
          @enderror
        </div>
        <button
          type="submit"
          class="btn btn-success"
        ><i class="fa-solid fa-floppy-disk"></i> Update
        </button>
      </form>
    
    </div>
  </div>
@endsection

```


resources/views/products/show.blade.php

```
@extends('products.layout')

@section('content')
  
  <div class="card mt-5">
    <h2 class="card-header">Show Product</h2>
    <div class="card-body">
      
      <div class="d-grid gap-2 d-md-flex justify-content-md-end">
        <a
          class="btn btn-primary btn-sm"
          href="{{ route('products.index') }}"
        ><i class="fa fa-arrow-left"></i> Back</a>
      </div>
      
      <div class="row">
        <div class="col-xs-12 col-sm-12 col-md-12">
          <div class="form-group">
            <strong>Name:</strong> <br/>
            {{ $product->name }}
          </div>
        </div>
        <div class="col-xs-12 col-sm-12 col-md-12 mt-2">
          <div class="form-group">
            <strong>Details:</strong> <br/>
            {{ $product->detail }}
          </div>
        </div>
      </div>
    
    </div>
  </div>
@endsection

```


**Run Laravel App:**
--------------------

All the required steps have been done, now you have to type the given below command and hit enter to run the Laravel app:

```
php artisan serve
```


Now, Go to your web browser, type the given URL and view the app output:

```
http://localhost:8000/products
```
I hope it can help you...

Ref: https://www.itsolutionstuff.com/post/laravel-11-crud-application-example-tutorialexample.html