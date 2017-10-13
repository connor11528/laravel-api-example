Laravel 5 REST API
---

> Build an API with Laravel 5

I'll be working alongside [this tutorial](https://www.toptal.com/laravel/restful-laravel-api-tutorial) for building the API. The author put the source code up for it [here](https://github.com/andrecastelo/example-api)

###Step 1: Create the project 

We can use [this handy script](https://gist.github.com/connor11528/fcfbdb63bc9633a54f40f0a66e3d3f2e) for generating a new Laravel 5 app.

I also wrote [this Medium article](https://medium.com/@connorleech/build-an-online-forum-with-laravel-initial-setup-and-seeding-part-1-a53138d1fffc) that goes through installing and configuring a MySQL database for a Laravel 5 application. I find myself refering back to it regularly. 

### Step 2: Configure database and make models

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


### Step 3: Step up a database seeders

By default we want data to play with so we must seed the database. Generate a new seeder for creating articles:

Create the articles table seeder:

```
$ php artisan make:seeder ArticlesTableSeeder
```

That file is in **database/seeds/ArticlesTableSeeder.php**. Update it so it looks like:

```
<?php

use App\Article;
use Illuminate\Database\Seeder;

class ArticlesTableSeeder extends Seeder
{
    public function run()
    {
        // Let's truncate our existing records to start from scratch.
        Article::truncate();

        $faker = \Faker\Factory::create();

        // And now, let's create a few articles in our database:
        for ($i = 0; $i < 50; $i++) {
            Article::create([
                'title' => $faker->sentence,
                'body' => $faker->paragraph,
            ]);
        }
    }
}
```

Run the database seeder:

```
$ php artisan db:seed --class=ArticlesTableSeeder
```

To make sure it worked we can use the `artisan tinker` command. For the uninitiated there's a great intro article about the tool [on scotch.io](https://scotch.io/tutorials/tinker-with-the-data-in-your-laravel-apps-with-php-artisan-tinker).

To use it we run:

```
$ php artisan tinker 
> App\Article::all();
```

Fifty articles in JSON format should fill up your console screen. Congrats we seeded the database full of articles!

We'll do nearly the same thing for the users table and then call both seeders from **database/seeds/DatabaseSeeder.php**:

```
class DatabaseSeeder extends Seeder
{
    public function run()
    {
        $this->call(ArticlesTableSeeder::class);
        $this->call(UsersTableSeeder::class);
    }
}
```

We'll be able to populate both database tables by running `php artisan db:seed`.

The full users table seeder in **database/seeds/UsersTableSeeder.php` looks like:

```
<?php

use Illuminate\Database\Seeder;
use App\User;

class UsersTableSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        // Let's clear the users table first
        User::truncate();

        $faker = \Faker\Factory::create();

        // Let's make sure everyone has the same password and 
        // let's hash it before the loop, or else our seeder 
        // will be too slow.
        $password = Hash::make('toptal');

        User::create([
            'name' => 'Administrator',
            'email' => 'admin@test.com',
            'password' => $password,
        ]);

        // And now let's generate a few dozen users for our app:
        for ($i = 0; $i < 10; $i++) {
            User::create([
                'name' => $faker->name,
                'email' => $faker->email,
                'password' => $password,
            ]);
        }
    }
}
```





