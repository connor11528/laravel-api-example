Laravel 5 REST API
---

I'll be working alongside [this tutorial](https://www.toptal.com/laravel/restful-laravel-api-tutorial) for building the API

**Step 1:** Create project with [this handy script](https://gist.github.com/connor11528/fcfbdb63bc9633a54f40f0a66e3d3f2e)

I also wrote [this Medium article](https://medium.com/@connorleech/build-an-online-forum-with-laravel-initial-setup-and-seeding-part-1-a53138d1fffc) that goes through installing and configuring a MySQL database for a Laravel 5 application. I find myself refering back to it regularly. 

**Step 2:** Configure database and make models

If you don't have MySQL installed on Mac may the force be with you. It comes preinstalled and there's MAMP but getting your machine set up to run SQL to MySQL from the terminal can be tricky.

My setup is to run:

```
$ mysql -uroot -p 
> create database MY_APP_NAME;
$ php artisan make:model Article -m
```

The `;` is required in order to end all SQL statements.

Add a **title** and **body** string columns to the articles database and run the migrations:

```
$ php artisan migrate
```

If you get an error saying cannot connect to homestead, even after you updated your .env to not point to homestead anymore clear the config and try again.

To clear config in Laravel:

```
$ php artisan config:clear
```

Make the models fillable (Laravel will protect them by default). Add this line to **app/Article.php**:

```
protected $fillable = ['title', 'body'];
```

Fillable will allow reads and writes to those database columns. You could also set a guarded property to an empty array:

```
$guarded = []
```

Which for our purposes will do the same thing. To learn more about this you can check the Eloquent docs on [Mass Assignment](https://laravel.com/docs/5.5/eloquent#mass-assignment).




