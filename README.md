# [Material Blog PRO Laravel](https://material-blog-pro-laravel.creative-tim.com/?ref=mbl-readme) [![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social&logo=twitter)](https://twitter.com/home?status=Material%20Dashboard%20Pro%20Laravel%E2%9D%A4%EF%B8%8F%0Ahttps%3A//material-blog-pro-laravel.creative-tim.com/%20%23%material%20%23design%20%23dashboard%20%23laravel%20%23pro%20via%20%40CreativeTim)

![version](https://img.shields.io/badge/version-1.0.0-blue.svg) ![license](https://img.shields.io/badge/license-MIT-blue.svg) [![GitHub issues open](https://img.shields.io/github/issues/creativetimofficial/ct-material-blog-pro-laravel.svg?maxAge=2592000)](https://github.com/creativetimofficial/ct-material-blog-pro-laravel/issues?q=is%3Aopen+is%3Aissue) [![GitHub issues closed](https://img.shields.io/github/issues-closed-raw/creativetimofficial/ct-material-blog-pro-laravel/ct-material-blog-pro-laravel.svg?maxAge=2592000)](https://github.com/creativetimofficial/ct-material-blog-pro-laravel/issues?q=is%3Aissue+is%3Aclosed)

*Frontend version*: Material Dashboard v2.1.0. More info at https://www.creative-tim.com/product/material-dashboard-pro
*Frontend version*: Material UI Kit v2.1.0. More info at https://www.creative-tim.com/product/material-kit-pro

[<img src="https://s3.amazonaws.com/creativetim_bucket/products/222/original/opt_mb_laravel_thumbnail.jpg" width="100%" />](https://www.creative-tim.com/live/material-blog-pro-laravel) 

Material Blog PRO with Laravel has all the core features you need in a blog, right out of the box and with a fresh, new design inspired by Google's Material Design.

## Prerequisites

If you don't already have an Apache local environment with PHP and MySQL, use one of the following links:

 - Windows: https://updivision.com/blog/post/beginner-s-guide-to-setting-up-your-local-development-environment-on-windows
 - Linux: https://howtoubuntu.org/how-to-install-lamp-on-ubuntu
 - Mac: https://wpshout.com/quick-guides/how-to-install-mamp-on-your-mac/

Also, you will need to install Composer: https://getcomposer.org/doc/00-intro.md
## Installation

1. Unzip the downloaded archive
2. Copy and paste **material-blog-pro-laravel** folder in your **projects** folder. Rename the folder to your project's name
3. In your terminal run `composer install`
4. Copy `.env.example` to `.env` and updated the configurations (mainly the database configuration)
5. In your terminal run `php artisan key:generate`
6. Run `php artisan migrate --seed` to create the database tables and seed the roles and users tables
7. Run `php artisan storage:link` to create the storage symlink (if you are using **Vagrant** with **Homestead** for development, remember to ssh into your virtual machine and run the command from there).

# Usage

## Blog

The Material Blog PRO with Laravel has all the core features a proper blog should have. A homepage with lists of featured and latest articles, also a section with the most popular authors in the platform. Pages with lists of articles, filtered by category, tag, author and also a search results page. And, last but not least, the article page, which also includes a list of related articles and a comments section, in order to comment you need to login with a member type account: 
<ul>
    <li> member type - <b>member@material.com</b> with the password <b>secret</b> </li>
</ul>

All the articles, categories and tags are managed by the Material Dashboard PRO interface, which you can access by logging in as an admin or author role:
<ul>
    <li> admin role - <b>admin@material.com</b> with the password <b>secret</b> </li>
    <li> author type - <b>author@material.com</b> with the password <b>secret</b> </li>
</ul>

## Dashboard

To start testing the Material Dashboard PRO, register as a user or log in using one of the default users:
<ul>
    <li> admin role - <b>admin@material.com</b> with the password <b>secret</b> </li>
    <li> author type - <b>author@material.com</b> with the password <b>secret</b> </li>
    <li> member type - <b>member@material.com</b> with the password <b>secret</b> </li>
</ul>

Make sure to run the migrations and seeders for the above credentials to be available.

The Material Dashboard PRO theme had user and  role management examples, as well as tag management, category management and article management examples. All the necessary files (controllers, requests, views) are installed out of the box and all the needed routes are added to <code>routes/web.php</code>. Keep in mind that all the features can be viewed once you log in using the credentials provided above or by registering your own user.

Each role has a different privilege level and can perform a certain number of actions according to this level.
A member type user can log in, update his profile and view a list of added articles.
A author type user can log in, update his profile and perform actions on categories, tags and articles.
A admin type user can log in, update his profile and perform actions on categories, tags, articles, roles and users.

### Profile edit

You have the option to edit the current logged in user's profile information (name, email, profile picture) and password. To access this page, just click the "**Examples/Profile**" link in the left sidebar or add **/profile** in the URL.

The `App\Http\Controllers\ProfileController` handles the update of the user information and password.

```
public function update(ProfileRequest $request)
{
    auth()->user()->update(
        $request->merge(['picture' => $request->photo ? $request->photo->store('profile', 'public') : null])
            ->except([$request->hasFile('photo') ? '' : 'picture'])
    );

    return back()->withStatus(__('Profile successfully updated.'));
}

/**
* Change the password
*
* @param  \App\Http\Requests\PasswordRequest  $request
* @return \Illuminate\Http\RedirectResponse
*/
public function password(PasswordRequest $request)
{
    auth()->user()->update(['password' => Hash::make($request->get('password'))]);

    return back()->withStatus(__('Password successfully updated.'));
}
```

If you input the wrong data when editing the profile, don`t worry. Validation rules have been added to prevent this (see `App\Http\Requests\ProfileRequest`). If you try to change the password, you will see that additional validation rules have been added in `App\Http\Requests\PasswordRequest`. You also have  a custom validation rule that can be found in `App\Rules\CurrentPasswordCheckRule`.

```
public function rules()
{
    return [
        'old_password' => ['required', 'min:6', new CurrentPasswordCheckRule],
        'password' => ['required', 'min:6', 'confirmed', 'different:old_password'],
        'password_confirmation' => ['required', 'min:6'],
    ];
}
```

### Role management

The Pro theme allows you to add user roles. By default, the theme comes with **Admin**, **Author** and **Member** roles. To access the role management example click the "**Examples/Role Management**" link in the left sidebar or add **/role** to the URL. Here you can add/edit new roles. Deletion is not allowed in this section.
To add a new role, click the "**Add role**" button. To edit an existing role, click the dotted menu (available on every table row) and then click "**Edit**". In both cases, you will be directed to a form which allows you to modify the name and description of a role.

The policy which authorizes the user to access the role management option is implemented in `App\Policies\RolePolicy.php`.``

You can find this functionality in `App\Http\RoleController.php`.

Also, validation rules were added so you will know exactly what to input in the form fields (see `App\Http\Requests\RoleRequest`). Note that these validation rules also apply for the role edit option.
```
public function rules()
{
    return [
        'name' => [
            'required', 'min:3', Rule::unique((new Role)->getTable())->ignore($this->route()->role->id ?? null)
        ],
        'description' => [
            'nullable', 'min:5'
        ]
    ];
}
```

### User management

The theme comes with an out of the box user management option. To access this option ,click the "**Examples/User Management**" link in the left sidebar or add **/user** to the URL.
The first thing you will see is a list of existing users. You can add new ones by clicking the "**Add user**" button (above the table on the right). On the Add user page, you will find a form which allows you to fill out the user`s name, email, role and password. All pages are generated using blade templates:

```
<div class="row">
  <label class="col-sm-2 col-form-label">{{ __('Name') }}</label>
  <div class="col-sm-7">
    <div class="form-group{{ $errors->has('name') ? ' has-danger' : '' }}">
      <input class="form-control{{ $errors->has('name') ? ' is-invalid' : '' }}" name="name" id="input-name" type="text" placeholder="{{ __('Name') }}" value="{{ old('name') }}" required="true" aria-required="true"/>
      @include('dashboard.alerts.feedback', ['field' => 'name'])
    </div>
  </div>
</div>
```

Validation rules were added to prevent errors in the form fields (see `App\Http\Requests\UserRequest`). Note that these validation rules also apply for the user edit option.

```
public function rules()
{
    return [
        'name' => [
            'required', 'min:3'
        ],
        'email' => [
            'required', 'email', Rule::unique((new User)->getTable())->ignore($this->route()->user->id ?? null)
        ],
        'role_id' => [
            'required', 'exists:'.(new Role)->getTable().',id'
        ],
        'password' => [
            $this->route()->user ? 'nullable' : 'required', 'confirmed', 'min:6'
        ]
    ];
}

/**
* Get the validation attributes that apply to the request.
*
* @return array
*/
public function attributes()
{
    return [
        'role_id' => 'role',
    ];
}
```

The policy which authorizes the user to access the user management pages is implemented in `App\Policies\UserPolicy.php`.``

Once you add more users, the list will grow and for every user you will have edit and delete options (access these options by clicking the three dotted menu that appears at the end of every row).

All the sample code for the user management can be found in `App\Http\Controllers\UserController`. See store method example bellow:

```
public function store(UserRequest $request, User $model)
{
    $model->create($request->merge([
        'picture' => $request->photo ? $request->photo->store('profile', 'public') : null,
        'password' => Hash::make($request->get('password'))
    ])->all());

    return redirect()->route('user.index')->withStatus(__('User successfully created.'));
}
```

### Tag management

The Pro theme also comes with a tag management option. To access this example, click the "**Examples/Tag Management**" link in the left sidebar or add **/tag** to the URL.
In this section you can add and edit tags, but you can only delete them if they are not attached to any articles. You can add new ones by clicking the "**Add tag**" button (above the table on the right). You will then be directed to a form which allows you to add new tags.
Although you can add in the form only the name and color of a tag, you can write your own migrations to extend this functionality.

```
public function destroy(Tag $tag)
{
    if (!$tag->articles->isEmpty()) {
        return redirect()->route('tag.index')->withErrors(__('This tag has articles attached and can\'t be deleted.'));
    }

    $tag->delete();

    return redirect()->route('tag.index')->withStatus(__('Tag successfully deleted.'));
}
```

The policy which authorizes the user to access tags management pages is implemented in `App\Policies\TagPolicy.php`.

### Category management

Out of the box you will have an example of category management (for the cases in which you are developing a blog or a shop). To access this example, click the "**Examples/Category Management**" link in the left sidebar or add **/category** to the URL.
You can add and edit categories here, but you can only delete them if they are not attached to any articles.

```
@if ($category->articles->isEmpty() && auth()->user()->can('delete', $category))
    <button type="button" class="btn btn-danger btn-link" data-original-title="" title="" onclick="confirm('{{ __("Are you sure you want to delete this category?") }}') ? this.parentElement.submit() : ''">
        <i class="material-icons">close</i>
        <div class="ripple-container"></div>
    </button>
@endif
```

The policy which authorizes the user on categories management pages is implemented in `App\Policies\CategoryPolicy.php`.

### Article management

Article management is the most advanced example included in the Pro theme, because every article has a picture, belongs to a category and has multiple tags. To access this example click the "**Examples/Article Management**" link in the left sidebar or add **/article** to the URL.
Here you can manage the articles. A list of articles will appear once you start adding them (to access the add page click "**Add article**").
On the add page, besides the Name and Description fields (which are present in most of the CRUD examples) you can see a category dropdown, which contains the categories you added, a file input and a tag multi select. If you did not add any categories or tags, please go to the corresponding sections (category management, tag management) and add some.  


The code for saving a new article is a bit different than before (see snippet bellow):

```
public function store(ArticleRequest $request, Article $model)
    {
        $article = $model->create($request->merge([
            'picture' => $request->photo->store('pictures', 'public'),
            'status' => $request->status ? 'published' : 'draft',
            'show_on_homepage' => $request->show_on_homepage ? 1 : 0,
            'publish_date' => $request->publish_date ? Carbon::parse($request->publish_date)->format('Y-m-d') : null,
            'author_id' => auth()->user()->id
        ])->all());
        
        $article->tags()->sync($request->get('tags'));

        return redirect()->route('article.index')->withStatus(__('Article successfully created.'));
    }
```

Notice how the picture and tags are easily saved in the database and on the disk.

Similar to all the examples included in the theme, this one also has validation rules in place. Note that the picture is mandatory only on the article creation. On the edit page, if no new picture is added, the old picture will not be modified.

```
public function rules()
    {
        return [
            'title' => [
                'required', 'min:3', Rule::unique((new Article)->getTable())->ignore($this->route()->article->id ?? null)
            ],
            'category_id' => [
                'required', 'exists:'.(new Category)->getTable().',id'
            ],
            'content' => [
                'required'
            ],
            'tags' => [
                'required'
            ],
            'tags.*' => [
                'exists:'.(new Tag)->getTable().',id'
            ],
            'photo' => [
                $this->route()->article ? 'nullable' : 'required', 'image'
            ],
            'publish_date' => [
                'required',
                'date_format:d-m-Y'
            ]
         ];
    }
```

In the article management we have an **observer** (`app\Observers\ArticleObserver`) example. This observer handles the deletion of the picture from the disk when the article is deleted or when the picture is changed via the edit form. The **observer** also removes the association between the article and the tags.

```
public function deleting(Article  $article)
{
    File::delete(storage_path("/app/public/{$article->picture}"));
    
    $article->tags()->detach();
}
```

The policy which authorizes the user on article management pages is implemented in `App\Policies\ArticlePolicy.php`.

# Blog

### Homepage

<p>
    Material Blog PRO with Laravel has all the core features you need in a blog, right out of the box: 
    <ul>
      <li>A homepage which lists featured and latest articles  </li>
      <li>A top authors section </li>
      <li>Articles listed by category, tag or author </li>
      <li>A search results page </li>
      <li>Article page (complete with related articles and a comment section) </li>
    </ul>
</p>
<p>
    The homepage is structured in three sections: 
    <ul>
      <li>the featured articles section. Featured articles are selected by the admin from the Material Dashboard admin panel  </li>
      <li>a list of the latest articles </li>
      <li>a section with the most popular authors </li>
    </ul>
</p>
     
<p> 
    In the <code class="highlighter-rouge">App\Http\HomeController</code>  you can see how data gets added to the page . "published", "publishedUntilToday" and "showHomepage" are local scopes and can be found in Article model. "userIsAuthor" is also a local scope which can be found in the User model.
</p>
<figure class="highlight"><pre><code class="language-html" data-lang="html">
public function index()
{
    $featured_articles = \App\Article::published()->showHomepage()->publishedUntilToday()->take(4)->get();
    $latest_articles = \App\Article::published()->publishedUntilToday()->orderBy('publish_date', 'desc')->take(3)->get();
    $authors = \App\User::userIsAuthor()->take(4)->get();

    return view('blog.home', compact(['featured_articles', 'latest_articles', 'authors']));
}
</code></pre></figure>
<p>The <code class="highlighter-rouge">Resources\Views\Blog\Home.blade.php</code> holds the information about featured articles, latest articles and top authors.</p>
<figure class="highlight"><pre><code class="language-html" data-lang="html">
&#x3C;h2 class=&#x22;title&#x22;&#x3E;{{ __(&#x27;Featured Articles&#x27;) }}&#x3C;/h2&#x3E;
  &#x3C;div class=&#x22;card card-plain card-blog&#x22;&#x3E;
    @foreach ($featured_articles as $article)
        @include(&#x27;blog._partials.featured_articles&#x27;)
    @endforeach
  &#x3C;/div&#x3E;
&#x3C;/h2&#x3E;

&#x3C;div class=&#x22;section&#x22;&#x3E;
  &#x3C;h2 class=&#x22;title text-center&#x22;&#x3E;{{ __(&#x27;Latest articles&#x27;) }}&#x3C;/h2&#x3E;
  &#x3C;div class=&#x22;row justify-content-center&#x22;&#x3E;
    @foreach ($latest_articles as $article)
        @include(&#x27;blog._partials.latest_articles&#x27;)
    @endforeach
    &#x3C;a href=&#x22;{{ route(&#x27;blog.article.index&#x27;) }}&#x22; class=&#x22;btn btn-rose btn-raised btn-round&#x22;&#x3E;
      {{ __(&#x27;View All&#x27;) }}
    &#x3C;/a&#x3E;
  &#x3C;/div&#x3E;
&#x3C;/div&#x3E;
@include(&#x27;blog._partials.authors&#x27;)
</code></pre></figure>

### All articles page
<p>
    In this page you will find all the published articles in paginated display. To access the page you can click the button "ALL ARTICLES" from the category nav bar or you can access it directly by typing '/all_articles' in the URL.
</p>
<p>
  For more information about this feature see <code class="highlighter-rouge">App\Http\Blog\ArticleController.php</code>.
</p>
<figure class="highlight"><pre><code class="language-html" data-lang="html">
public function index(Article $model)
{
    $articles = $model->with(['tags', 'category'])->paginate(10);
    return view('blog.all_articles', ['articles' => $articles]);
}
</code></pre></figure>
<p>In the <code class="highlighter-rouge">Resources\Views\Blog\All_articles.blade.php</code> you can find all the articles.</p>
<figure class="highlight"><pre><code class="language-html" data-lang="html">
&#x3C;div class=&#x22;container&#x22;&#x3E;
  &#x3C;div class=&#x22;section&#x22;&#x3E;
    &#x3C;div class=&#x22;container&#x22;&#x3E;
      &#x3C;div class=&#x22;row&#x22;&#x3E;
        &#x3C;div class=&#x22;col-md-10 ml-auto mr-auto&#x22;&#x3E;
          &#x3C;h2 class=&#x22;title&#x22;&#x3E;{{ __(&#x27;All Articles&#x27;) }}&#x3C;/h2&#x3E;
          @include(&#x27;blog._partials.article_full&#x27;)
          @include(&#x27;blog._partials.pagination&#x27;)
        &#x3C;/div&#x3E;
      &#x3C;/div&#x3E;
    &#x3C;/div&#x3E;
  &#x3C;/div&#x3E;
&#x3C;/div&#x3E;
</code></pre></figure>

### Article page

<p>
    The article page has, besides the article content, a comment section and a more articles section.
</p>
<p>
    In order to comment, you have to be logged in with a member type account. You can either go to the top menu and click on the "Login as" button or go directly to /login?role=1. To log in as a member, use the following credentials:
    <ul>
      <li> member type - <b>member@material.com</b> with the password <b>secret</b> </li>
    </ul>
</p>
<p>The <code class="highlighter-rouge">App\Http\Controllers\Blog\ArticleController</code> holds the <strong>show</strong>  function, which handles the details about the article, the comments and the more articles section.</p>
<figure class="highlight"><pre><code class="language-html" data-lang="html">
public function show(Article $article, Comment $modelComment)
{
    $article = \App\Article::find($article->id);
    $moreArticles = \App\Article::published()->publishedUntilToday()->category($article->category_id)->orderBy('publish_date', 'desc')->take(3)->get();
    
    return view('blog.show', compact(['article', 'moreArticles']));
}
</code></pre></figure>
<p>The <code class="highlighter-rouge">Resources\Views\Blog\Show.blade.php</code> holds the information about handling the details about the article, the comments and the more articles section.
</p>



### List articles by author

<p>
    You can filter pages by their author. You can find this functionality in <code class="highlighter-rouge">App\Http\Blog\ArticlesAuthorController.php</code>.
</p>
<figure class="highlight"><pre><code class="language-html" data-lang="html">
public function index(Request $request, User $user)
{
    $author = User::find($user->id);
    $articles = \App\Article::published()->publishedUntilToday()->author($user->id)->paginate(10);

    return view('blog.articles_author', compact(['articles', 'author']));
}
</code></pre></figure>
<p>The <code class="highlighter-rouge">Resources\Views\Blog\Articles_author.blade.php</code> gets and shows the articles written by an author.</p>
<figure class="highlight"><pre><code class="language-html" data-lang="html">
&#x3C;div class=&#x22;row&#x22;&#x3E;
  &#x3C;div class=&#x22;col-md-9 text-left&#x22;&#x3E;
    &#x3C;h2 class=&#x22;card-title&#x22;&#x3E;&#x3C;span class=&#x22;font-weight-light&#x22;&#x3E;{{ __(&#x27;Articles by &#x27;) }}&#x3C;/span&#x3E;{{ $user-&#x3E;name }}&#x3C;/h2&#x3E;
    &#x3C;h4&#x3E;{{ $user-&#x3E;about }}&#x3C;/h4&#x3E;
  &#x3C;/div&#x3E;
  &#x3C;div class=&#x22;col-md-3&#x22;&#x3E;
    &#x3C;div class=&#x22;card-avatar&#x22;&#x3E;
      &#x3C;a href=&#x22;{{ route(&#x27;blog.author&#x27;, $user-&#x3E;slug) }}&#x22;&#x3E;
        &#x3C;img class=&#x22;img&#x22; src=&#x22;{{ $user-&#x3E;profilePicture() }}&#x22;&#x3E;
      &#x3C;/a&#x3E;
      &#x3C;div class=&#x22;ripple-container&#x22;&#x3E;&#x3C;/div&#x3E;
    &#x3C;/div&#x3E;
  &#x3C;/div&#x3E;
&#x3C;/div&#x3E;
</code></pre></figure>


### List articles by category

<p>
    The blog comes with a category filter option. To access this simply click on the category label next to a category.
</p>
<p>
    You can find this functionality in <code class="highlighter-rouge">App\Http\Blog\ArticlesCategoryController.php</code>.
</p>
<figure class="highlight"><pre><code class="language-html" data-lang="html">
public function index(Category $category)
{
    $articles = \App\Article::published()->publishedUntilToday()->category($category->id)->paginate(10);

    return view('blog.articles_category', compact(['articles', 'category']));
}
</code></pre></figure>
<p>The <code class="highlighter-rouge">Resources\Views\Blog\Articles_category.blade.php</code> gets and shows the articles that belong to a category.</p>
<figure class="highlight"><pre><code class="language-html" data-lang="html">
&#x3C;div class=&#x22;row&#x22;&#x3E;
  &#x3C;div class=&#x22;col-md-10 ml-auto mr-auto&#x22;&#x3E;
    &#x3C;h2 class=&#x22;title&#x22;&#x3E;
      {{ $category-&#x3E;name }}
    &#x3C;/h2&#x3E;
    &#x3C;h4&#x3E;
      {{ $category-&#x3E;description }}
    &#x3C;/h4&#x3E;
    @include(&#x27;blog._partials.article_full&#x27;)
    @include(&#x27;blog._partials.pagination&#x27;)
  &#x3C;/div&#x3E;
&#x3C;/div&#x3E;
</code></pre></figure>


### List articles by tags

<p>
    There is also an option to filter articles by tag. To access this simply click a tag label next to an article and it will take you to the articles filtered by tag page.
</p>
<p>
    You can find this functionality in <code class="highlighter-rouge">App\Http\Blog\ArticlesTagsController.php</code>.
</p>
<figure class="highlight"><pre><code class="language-html" data-lang="html">
public function index(Tag $tag)
{
    $articles = \App\Article::published()->publishedUntilToday()->tag($tag->id)->paginate(10);

    return view('blog.articles_tag', compact(['articles', 'tag']));
}
</code></pre></figure>
<p>The <code class="highlighter-rouge">Resources\Views\Blog\Articles_category.blade.php</code> gets and shows the articles that have a specified tag.</p>
<figure class="highlight"><pre><code class="language-html" data-lang="html">
&#x3C;div class=&#x22;row&#x22;&#x3E;
  &#x3C;div class=&#x22;col-md-10 ml-auto mr-auto&#x22;&#x3E;
    &#x3C;h2 class=&#x22;card-title&#x22;&#x3E;{{ __(&#x27;Articles by&#x27;) }}&#x3C;span style=&#x22;background-color: {{ $tag-&#x3E;color }}; margin-left: 20px;&#x22; class=&#x22;btn btn-round&#x22;&#x3E;{{ $tag-&#x3E;name }}&#x3C;/span&#x3E;&#x3C;/h2&#x3E; 
    @include(&#x27;blog._partials.article_full&#x27;)
    @include(&#x27;blog._partials.pagination&#x27;)
  &#x3C;/div&#x3E;
&#x3C;/div&#x3E;
</code></pre></figure>


## Table of Contents

* [Versions](#versions)
* [Demo](#demo)
* [Documentation](#documentation)
* [File Structure](#file-structure)
* [Browser Support](#browser-support)
* [Resources](#resources)
* [Reporting Issues](#reporting-issues)
* [Licensing](#licensing)
* [Useful Links](#useful-links)

## Versions

[<img src="https://github.com/creativetimofficial/public-assets/blob/master/logos/laravel_logo.png?raw=true" width="60" height="60" />](https://material-blog-pro-laravel.creative-tim.com/?ref=mbl-readme)

| LARAVEL |
| --- |
| [![Material Dashboard Pro Laravel](https://s3.amazonaws.com/creativetim_bucket/products/158/thumb/opt_mdp_laravel_thumbnail.jpg)](https://material-blog-pro-laravel.creative-tim.com/?ref=mbl-readme)

## Demo

| Home Page | Category Page | Article Page |
| --- | --- | ---  |
| [![Home Page](https://github.com/creativetimofficial/public-assets/blob/master/material-blog-pro-laravel/home.png)](https://material-blog-pro-laravel.creative-tim.com/profile?ref=mbl-readme)  | [![Category Page](https://github.com/creativetimofficial/public-assets/blob/master/material-blog-pro-laravel/category.png)](https://material-blog-pro-laravel.creative-tim.com/user?ref=mbl-readme) | [![Article Page](https://github.com/creativetimofficial/public-assets/blob/master/material-blog-pro-laravel/article.png)](https://material-blog-pro-laravel.creative-tim.com/table-list?ref=mbl-readme)
[View More](https://material-blog-pro-laravel.creative-tim.com/?ref=mbl-readme)

| Register | Login | Dashboard |
| --- | --- | ---  |
| [![Register](https://github.com/creativetimofficial/public-assets/raw/master/material-dashboard-pro-laravel/Register.png)](https://material-dashboard-pro-laravel.creative-tim.com/register?ref=mdl-readme)  | [![Login](https://github.com/creativetimofficial/public-assets/raw/master/material-dashboard-pro-laravel/Login.png)](https://material-dashboard-pro-laravel.creative-tim.com/login?ref=mdl-readme)  | [![Dashboard](https://github.com/creativetimofficial/public-assets/raw/master/material-dashboard-pro-laravel/Dashboard.png)](https://material-dashboard-pro-laravel.creative-tim.com/?ref=mdl-readme)

| Profile Page | Users Page | Tables Page  |
| --- | --- | ---  |
| [![Profile Page](https://github.com/creativetimofficial/public-assets/raw/master/material-dashboard-pro-laravel/Profile.png)](https://material-dashboard-pro-laravel.creative-tim.com/profile?ref=mdl-readme)  | [![Users Page](https://github.com/creativetimofficial/public-assets/raw/master/material-dashboard-pro-laravel/Users.png)](https://material-dashboard-pro-laravel.creative-tim.com/user?ref=mdl-readme) | [![Tables Page](https://github.com/creativetimofficial/public-assets/raw/master/material-dashboard-pro-laravel/Tables.png)](https://material-dashboard-pro-laravel.creative-tim.com/table-list?ref=mdl-readme)

## Documentation
The documentation for the Material Blog with Laravel is hosted at our [website](https://material-blog-pro-laravel.creative-tim.com/docs/material-blog-pro-laravel/docs/getting-started/laravel-setup.html?ref=mbl-readme).

## File Structure
```
 .
├── app
│   ├── Article.php
│   ├── Category.php
│   ├── Comment.php
│   ├── Console
│   │   └── Kernel.php
│   ├── Exceptions
│   │   └── Handler.php
│   ├── Http
│   │   ├── Controllers
│   │   │   ├── ArticleController.php
│   │   │   ├── Auth
│   │   │   │   ├── ForgotPasswordController.php
│   │   │   │   ├── LoginController.php
│   │   │   │   ├── RegisterController.php
│   │   │   │   ├── ResetPasswordController.php
│   │   │   │   └── VerificationController.php
│   │   │   ├── Blog
│   │   │   │   ├── ArticleController.php
│   │   │   │   ├── ArticlesAuthorController.php
│   │   │   │   ├── ArticlesCategoryController.php
│   │   │   │   ├── ArticlesSearchController.php
│   │   │   │   ├── ArticlesTagController.php
│   │   │   │   ├── BlogController.php
│   │   │   │   ├── CommentsController.php
│   │   │   │   └── NewsletterController.php
│   │   │   ├── CategoryController.php
│   │   │   ├── ComponentPagesController.php
│   │   │   ├── Controller.php
│   │   │   ├── ExamplePagesController.php
│   │   │   ├── FormPagesController.php
│   │   │   ├── HomeController.php
│   │   │   ├── MapPagesController.php
│   │   │   ├── ProfileController.php
│   │   │   ├── RoleController.php
│   │   │   ├── TablePagesController.php
│   │   │   ├── TagController.php
│   │   │   └── UserController.php
│   │   ├── Kernel.php
│   │   ├── Middleware
│   │   │   ├── Authenticate.php
│   │   │   ├── CheckForMaintenanceMode.php
│   │   │   ├── EncryptCookies.php
│   │   │   ├── RedirectIfAuthenticated.php
│   │   │   ├── TrimStrings.php
│   │   │   ├── TrustProxies.php
│   │   │   └── VerifyCsrfToken.php
│   │   └── Requests
│   │       ├── ArticleRequest.php
│   │       ├── CategoryRequest.php
│   │       ├── CommentsRequest.php
│   │       ├── NewsletterRequest.php
│   │       ├── PasswordRequest.php
│   │       ├── ProfileRequest.php
│   │       ├── RoleRequest.php
│   │       ├── TagRequest.php
│   │       └── UserRequest.php
│   ├── Observers
│   │   ├── ArticleObserver.php
│   │   └── UserObserver.php
│   ├── Policies
│   │   ├── ArticlePolicy.php
│   │   ├── CategoryPolicy.php
│   │   ├── RolePolicy.php
│   │   ├── TagPolicy.php
│   │   └── UserPolicy.php
│   ├── Providers
│   │   ├── AppServiceProvider.php
│   │   ├── AuthServiceProvider.php
│   │   ├── BroadcastServiceProvider.php
│   │   ├── EventServiceProvider.php
│   │   └── RouteServiceProvider.php
│   ├── Role.php
│   ├── Rules
│   │   └── CurrentPasswordCheckRule.php
│   ├── Tag.php
│   └── User.php
├── artisan
├── bootstrap
│   ├── app.php
│   └── cache
│       ├── config.php
│       ├── .gitignore
│       ├── packages.php
│       ├── routes.php
│       └── services.php
├── changelog.md
├── composer.json
├── composer.lock
├── config
│   ├── app.php
│   ├── articles.php
│   ├── auth.php
│   ├── broadcasting.php
│   ├── cache.php
│   ├── database.php
│   ├── filesystems.php
│   ├── hashing.php
│   ├── logging.php
│   ├── mail.php
│   ├── newsletter.php
│   ├── queue.php
│   ├── services.php
│   ├── session.php
│   └── view.php
├── database
│   ├── factories
│   │   └── UserFactory.php
│   ├── .gitignore
│   ├── migrations
│   │   ├── 2014_10_12_100000_create_password_resets_table.php
│   │   ├── 2019_01_15_100000_create_roles_table.php
│   │   ├── 2019_01_15_110000_create_users_table.php
│   │   ├── 2019_01_17_121504_create_categories_table.php
│   │   ├── 2019_01_21_130422_create_tags_table.php
│   │   ├── 2019_01_21_163402_create_article_table.php
│   │   ├── 2019_01_21_163414_create_article_tag_table.php
│   │   └── 2019_09_09_085054_create_comments_table.php
│   └── seeds
│       ├── ArticlesTableSeeder.php
│       ├── CategoriesTableSeeder.php
│       ├── DatabaseSeeder.php
│       ├── RolesTableSeeder.php
│       ├── TagsTableSeeder.php
│       └── UsersTableSeeder.php
├── .editorconfig
├── .env
├── .env.example
├── .git
│   ├── branches
│   ├── COMMIT_EDITMSG
│   ├── config
│   ├── description
│   ├── FETCH_HEAD
│   ├── HEAD
│   ├── hooks
│   │   ├── applypatch-msg.sample
│   │   ├── commit-msg.sample
│   │   ├── fsmonitor-watchman.sample
│   │   ├── post-update.sample
│   │   ├── pre-applypatch.sample
│   │   ├── pre-commit.sample
│   │   ├── prepare-commit-msg.sample
│   │   ├── pre-push.sample
│   │   ├── pre-rebase.sample
│   │   ├── pre-receive.sample
│   │   └── update.sample
│   ├── index
│   ├── info
│   │   └── exclude
│   ├── logs
│   │   ├── HEAD
│   │   └── refs
│   │       ├── heads
│   │       │   ├── demo
│   │       │   ├── develop
│   │       │   └── master
│   │       ├── remotes
│   │       │   └── origin
│   │       │       ├── demo
│   │       │       ├── develop
│   │       │       ├── HEAD
│   │       │       └── master
│   │       └── stash
│   ├── objects
│   │   ├── info
│   │   └── pack
│   ├── ORIG_HEAD
│   ├── packed-refs
│   └── refs
│       ├── heads
│       │   ├── demo
│       │   ├── develop
│       │   └── master
│       ├── remotes
│       │   └── origin
│       │       ├── demo
│       │       ├── develop
│       │       ├── HEAD
│       │       └── master
│       ├── stash
│       └── tags
├── .gitattributes
├── .gitignore
├── ISSUE_TEMPLATE.md
├── package.json
├── phpunit.xml
├── public
│   ├── css
│   │   └── app.css
│   ├── docs
│   │   └── documentation.html
│   ├── favicon.ico
│   ├── .htaccess
│   ├── index.php
│   ├── js
│   │   └── app.js
│   ├── material
│   │   ├── css
│   │   │   ├── material-dashboard.css
│   │   │   ├── material-dashboard.css.map
│   │   │   ├── material-dashboard.min.css
│   │   │   ├── material-kit.css
│   │   │   ├── material-kit.css.map
│   │   │   ├── material-kit.min.css
│   │   │   ├── partials
│   │   │   │   ├── dashboard
│   │   │   │   │   └── core
│   │   │   │   │       └── bootstrap
│   │   │   │   │           └── scss
│   │   │   │   │               ├── bootstrap.css
│   │   │   │   │               ├── bootstrap.css.map
│   │   │   │   │               ├── bootstrap-grid.css
│   │   │   │   │               ├── bootstrap-grid.css.map
│   │   │   │   │               ├── bootstrap-reboot.css
│   │   │   │   │               └── bootstrap-reboot.css.map
│   │   │   │   └── kit
│   │   │   │       └── core
│   │   │   │           └── bootstrap
│   │   │   │               └── scss
│   │   │   │                   ├── bootstrap.css
│   │   │   │                   ├── bootstrap.css.map
│   │   │   │                   ├── bootstrap-grid.css
│   │   │   │                   ├── bootstrap-grid.css.map
│   │   │   │                   ├── bootstrap-reboot.css
│   │   │   │                   └── bootstrap-reboot.css.map
│   │   │   ├── quill.core.css
│   │   │   └── quill.snow.css
│   │   ├── demo
│   │   │   ├── demo.css
│   │   │   ├── demo.js
│   │   │   ├── modernizr.js
│   │   │   ├── vertical-nav.css
│   │   │   └── vertical-nav.js
│   │   ├── .DS_Store
│   │   ├── img
│   │   │   ├── apple-icon.png
│   │   │   ├── arrow-left.cur
│   │   │   ├── arrow-left.png
│   │   │   ├── arrow-right.cur
│   │   │   ├── arrow-right.png
│   │   │   ├── bg0.jpg
│   │   │   ├── bg10.jpg
│   │   │   ├── bg11.jpg
│   │   │   ├── bg12.jpg
│   │   │   ├── bg2.jpg
│   │   │   ├── bg3.jpg
│   │   │   ├── bg5.jpg
│   │   │   ├── bg6.jpg
│   │   │   ├── bg7.jpg
│   │   │   ├── bg8.jpg
│   │   │   ├── bg9.jpg
│   │   │   ├── bg.jpg
│   │   │   ├── bg-pricing.jpg
│   │   │   ├── card-1.jpeg
│   │   │   ├── card-1.jpg
│   │   │   ├── card-2.jpeg
│   │   │   ├── card-2.jpg
│   │   │   ├── card-3.jpeg
│   │   │   ├── card-3.jpg
│   │   │   ├── cards-test.png
│   │   │   ├── city-profile.jpg
│   │   │   ├── clint-mckoy.jpg
│   │   │   ├── creative-logo.png
│   │   │   ├── Creative-TimLOGO2.png
│   │   │   ├── default-avatar.png
│   │   │   ├── dg10.jpg
│   │   │   ├── dg1.jpg
│   │   │   ├── dg2.jpg
│   │   │   ├── dg3.jpg
│   │   │   ├── dg6.jpg
│   │   │   ├── dg9.jpg
│   │   │   ├── example-pages
│   │   │   │   ├── ex-about-us.jpg
│   │   │   │   ├── ex-blog-post.jpg
│   │   │   │   ├── ex-blog-posts.jpg
│   │   │   │   ├── ex-contact.jpg
│   │   │   │   ├── ex-landing.jpg
│   │   │   │   ├── ex-login.jpg
│   │   │   │   ├── ex-pricing.jpg
│   │   │   │   ├── ex-product.jpg
│   │   │   │   ├── ex-profile.jpg
│   │   │   │   ├── ex-register.jpg
│   │   │   │   ├── harvard.jpg
│   │   │   │   ├── microsoft.jpg
│   │   │   │   ├── stanford.jpg
│   │   │   │   └── vodafone.jpg
│   │   │   ├── examples
│   │   │   │   ├── bg1.jpg
│   │   │   │   ├── bg2.jpg
│   │   │   │   ├── blog1.jpg
│   │   │   │   ├── blog2.jpg
│   │   │   │   ├── blog3.jpg
│   │   │   │   ├── blog4.jpg
│   │   │   │   ├── blog5.jpg
│   │   │   │   ├── blog6.jpg
│   │   │   │   ├── blog7.jpg
│   │   │   │   ├── blog8.jpg
│   │   │   │   ├── card-blog1.jpg
│   │   │   │   ├── card-blog2.jpg
│   │   │   │   ├── card-blog3.jpg
│   │   │   │   ├── card-blog4.jpg
│   │   │   │   ├── card-blog5.jpg
│   │   │   │   ├── card-blog6.jpg
│   │   │   │   ├── card-blog8.jpg
│   │   │   │   ├── card-product1.jpg
│   │   │   │   ├── card-product2.jpg
│   │   │   │   ├── card-product3.jpg
│   │   │   │   ├── card-product4.jpg
│   │   │   │   ├── card-profile1.jpg
│   │   │   │   ├── card-profile2.jpg
│   │   │   │   ├── card-profile4.jpg
│   │   │   │   ├── card-profile5.jpg
│   │   │   │   ├── card-profile6.jpg
│   │   │   │   ├── card-profile7.jpg
│   │   │   │   ├── card-project1.jpg
│   │   │   │   ├── card-project2.jpg
│   │   │   │   ├── card-project3.jpg
│   │   │   │   ├── card-project4.jpg
│   │   │   │   ├── card-project5.jpg
│   │   │   │   ├── card-project6.jpg
│   │   │   │   ├── city.jpg
│   │   │   │   ├── clark-street-merc.jpg
│   │   │   │   ├── clem-onojegaw.jpg
│   │   │   │   ├── clem-onojeghuo.jpg
│   │   │   │   ├── clem-onojeghuou.jpg
│   │   │   │   ├── color1.jpg
│   │   │   │   ├── color2.jpg
│   │   │   │   ├── color3.jpg
│   │   │   │   ├── cynthia-del-rio.jpg
│   │   │   │   ├── darren-coleshill.jpg
│   │   │   │   ├── dolce.jpg
│   │   │   │   ├── ecommerce-header.jpg
│   │   │   │   ├── ecommerce-tips2.jpg
│   │   │   │   ├── gucci.jpg
│   │   │   │   ├── mariya-georgieva.jpg
│   │   │   │   ├── office1.jpg
│   │   │   │   ├── office2.jpg
│   │   │   │   ├── office3.jpg
│   │   │   │   ├── office4.jpg
│   │   │   │   ├── office5.jpg
│   │   │   │   ├── olu-eletu.jpg
│   │   │   │   ├── product1.jpg
│   │   │   │   ├── product2.jpg
│   │   │   │   ├── product3.jpg
│   │   │   │   ├── product4.jpg
│   │   │   │   ├── profile_city.jpg
│   │   │   │   ├── slim-emcee-ug-the-poet-truth_from_africa_photography.jpg
│   │   │   │   ├── studio-1.jpg
│   │   │   │   ├── studio-2.jpg
│   │   │   │   ├── studio-3.jpg
│   │   │   │   ├── studio-4.jpg
│   │   │   │   ├── studio-5.jpg
│   │   │   │   ├── suit-1.jpg
│   │   │   │   ├── suit-2.jpg
│   │   │   │   ├── suit-3.jpg
│   │   │   │   ├── suit-4.jpg
│   │   │   │   ├── suit-5.jpg
│   │   │   │   ├── suit-6.jpg
│   │   │   │   └── tom-ford.jpg
│   │   │   ├── faces
│   │   │   │   ├── avatar.jpg
│   │   │   │   ├── camp.jpg
│   │   │   │   ├── card-profile1-square.jpg
│   │   │   │   ├── card-profile2-square.jpg
│   │   │   │   ├── card-profile4-square.jpg
│   │   │   │   ├── card-profile5-square.jpg
│   │   │   │   ├── card-profile6-square.jpg
│   │   │   │   ├── christian.jpg
│   │   │   │   ├── kendall.jpg
│   │   │   │   └── marc.jpg
│   │   │   ├── favicon.png
│   │   │   ├── features-5.jpg
│   │   │   ├── flags
│   │   │   │   ├── AD.png
│   │   │   │   ├── AE.png
│   │   │   │   ├── AG.png
│   │   │   │   ├── AM.png
│   │   │   │   ├── AR.png
│   │   │   │   ├── AT.png
│   │   │   │   ├── AU.png
│   │   │   │   ├── BE.png
│   │   │   │   ├── BF.png
│   │   │   │   ├── BG.png
│   │   │   │   ├── BO.png
│   │   │   │   ├── BR.png
│   │   │   │   ├── CA.png
│   │   │   │   ├── CD.png
│   │   │   │   ├── CG.png
│   │   │   │   ├── CH.png
│   │   │   │   ├── CL.png
│   │   │   │   ├── CM.png
│   │   │   │   ├── CN.png
│   │   │   │   ├── CO.png
│   │   │   │   ├── CZ.png
│   │   │   │   ├── DE.png
│   │   │   │   ├── DJ.png
│   │   │   │   ├── DK.png
│   │   │   │   ├── DZ.png
│   │   │   │   ├── EE.png
│   │   │   │   ├── EG.png
│   │   │   │   ├── ES.png
│   │   │   │   ├── FI.png
│   │   │   │   ├── FR.png
│   │   │   │   ├── GA.png
│   │   │   │   ├── GB.png
│   │   │   │   ├── GM.png
│   │   │   │   ├── GT.png
│   │   │   │   ├── HN.png
│   │   │   │   ├── HT.png
│   │   │   │   ├── HU.png
│   │   │   │   ├── ID.png
│   │   │   │   ├── IE.png
│   │   │   │   ├── IL.png
│   │   │   │   ├── IN.png
│   │   │   │   ├── IQ.png
│   │   │   │   ├── IR.png
│   │   │   │   ├── IT.png
│   │   │   │   ├── JM.png
│   │   │   │   ├── JO.png
│   │   │   │   ├── JP.png
│   │   │   │   ├── KG.png
│   │   │   │   ├── KN.png
│   │   │   │   ├── KP.png
│   │   │   │   ├── KR.png
│   │   │   │   ├── KW.png
│   │   │   │   ├── KZ.png
│   │   │   │   ├── LA.png
│   │   │   │   ├── LB.png
│   │   │   │   ├── LC.png
│   │   │   │   ├── LS.png
│   │   │   │   ├── LU.png
│   │   │   │   ├── LV.png
│   │   │   │   ├── MG.png
│   │   │   │   ├── MK.png
│   │   │   │   ├── ML.png
│   │   │   │   ├── MM.png
│   │   │   │   ├── MT.png
│   │   │   │   ├── MX.png
│   │   │   │   ├── NA.png
│   │   │   │   ├── NE.png
│   │   │   │   ├── NG.png
│   │   │   │   ├── NI.png
│   │   │   │   ├── NL.png
│   │   │   │   ├── NO.png
│   │   │   │   ├── OM.png
│   │   │   │   ├── PA.png
│   │   │   │   ├── PE.png
│   │   │   │   ├── PG.png
│   │   │   │   ├── PK.png
│   │   │   │   ├── PL.png
│   │   │   │   ├── PT.png
│   │   │   │   ├── PY.png
│   │   │   │   ├── QA.png
│   │   │   │   ├── RO.png
│   │   │   │   ├── RU.png
│   │   │   │   ├── RW.png
│   │   │   │   ├── SA.png
│   │   │   │   ├── SE.png
│   │   │   │   ├── SG.png
│   │   │   │   ├── SL.png
│   │   │   │   ├── SN.png
│   │   │   │   ├── SO.png
│   │   │   │   ├── SV.png
│   │   │   │   ├── TD.png
│   │   │   │   ├── TJ.png
│   │   │   │   ├── TL.png
│   │   │   │   ├── TR.png
│   │   │   │   ├── TZ.png
│   │   │   │   ├── UA.png
│   │   │   │   ├── US.png
│   │   │   │   ├── VE.png
│   │   │   │   ├── VN.png
│   │   │   │   └── YE.png
│   │   │   ├── header-doc.jpg
│   │   │   ├── image_placeholder.jpg
│   │   │   ├── laravel.svg
│   │   │   ├── loading-bubbles.svg
│   │   │   ├── lock.jpg
│   │   │   ├── login.jpg
│   │   │   ├── logo.png
│   │   │   ├── mask.png
│   │   │   ├── new_logo.png
│   │   │   ├── office2.jpg
│   │   │   ├── placeholder.jpg
│   │   │   ├── product1.jpg
│   │   │   ├── product2.jpg
│   │   │   ├── product3.jpg
│   │   │   ├── register.jpg
│   │   │   ├── responsive-nav.gif
│   │   │   ├── section-components
│   │   │   │   ├── coloured-card.jpg
│   │   │   │   ├── coloured-card-with-btn.jpg
│   │   │   │   ├── ipad-comments.jpg
│   │   │   │   ├── ipad-table.jpg
│   │   │   │   ├── laptop-basics.png
│   │   │   │   ├── pin-btn.jpg
│   │   │   │   ├── presentation-ipad.jpg
│   │   │   │   ├── responsive.png
│   │   │   │   ├── share-btn.jpg
│   │   │   │   ├── social-row.jpg
│   │   │   │   └── table.jpg
│   │   │   ├── sections
│   │   │   │   ├── b_1.jpg
│   │   │   │   ├── b_2.jpg
│   │   │   │   ├── b_3.jpg
│   │   │   │   ├── b_4.jpg
│   │   │   │   ├── f_1.jpg
│   │   │   │   ├── f_2.jpg
│   │   │   │   ├── f_3.jpg
│   │   │   │   ├── f_4.jpg
│   │   │   │   ├── f_5.jpg
│   │   │   │   ├── h_1.jpg
│   │   │   │   ├── h_2.jpg
│   │   │   │   ├── h_3.jpg
│   │   │   │   ├── h_4.jpg
│   │   │   │   ├── iphone2.png
│   │   │   │   ├── iphone.png
│   │   │   │   ├── m_1.jpg
│   │   │   │   ├── m_2.jpg
│   │   │   │   ├── p_1.jpg
│   │   │   │   ├── p_2.jpg
│   │   │   │   ├── p_3.jpg
│   │   │   │   ├── p_4.jpg
│   │   │   │   ├── p_5.jpg
│   │   │   │   ├── pro_1.jpg
│   │   │   │   ├── pro_2.jpg
│   │   │   │   ├── pro_3.jpg
│   │   │   │   ├── pro_4.jpg
│   │   │   │   ├── t_1.jpg
│   │   │   │   ├── t_2.jpg
│   │   │   │   ├── t_3.jpg
│   │   │   │   ├── team_1.jpg
│   │   │   │   ├── team_2.jpg
│   │   │   │   ├── team_3.jpg
│   │   │   │   ├── team_4.jpg
│   │   │   │   └── team_5.jpg
│   │   │   ├── sidebar-1.jpg
│   │   │   ├── sidebar-2.jpg
│   │   │   ├── sidebar-3.jpg
│   │   │   ├── sidebar-4.jpg
│   │   │   ├── test1.jpg
│   │   │   ├── test2.jpg
│   │   │   ├── test3.jpg
│   │   │   ├── tim-logo.png
│   │   │   └── world2.png
│   │   ├── index.html
│   │   ├── js
│   │   │   ├── application.js
│   │   │   ├── article.js
│   │   │   ├── core
│   │   │   │   ├── bootstrap-material-design.min.js
│   │   │   │   ├── jquery.min.js
│   │   │   │   └── popper.min.js
│   │   │   ├── material-dashboard.js
│   │   │   ├── material-dashboard.js.map
│   │   │   ├── material-dashboard.min.js
│   │   │   ├── material-kit.js
│   │   │   ├── material-kit.js.map
│   │   │   ├── material-kit.min.js
│   │   │   ├── plugins
│   │   │   │   ├── arrive.min.js
│   │   │   │   ├── bootstrap-datetimepicker.js
│   │   │   │   ├── bootstrap-datetimepicker.min.js
│   │   │   │   ├── bootstrap-notify.js
│   │   │   │   ├── bootstrap-selectpicker.js
│   │   │   │   ├── bootstrap-tagsinput.js
│   │   │   │   ├── chartist.min.js
│   │   │   │   ├── fullcalendar.min.js
│   │   │   │   ├── jasny-bootstrap.min.js
│   │   │   │   ├── jquery.bootstrap-wizard.js
│   │   │   │   ├── jquery.dataTables.min.js
│   │   │   │   ├── jquery.flexisel.js
│   │   │   │   ├── jquery-jvectormap.js
│   │   │   │   ├── jquery.sharrre.js
│   │   │   │   ├── jquery.tagsinput.js
│   │   │   │   ├── jquery.validate.min.js
│   │   │   │   ├── moment.min.js
│   │   │   │   ├── nouislider.min.js
│   │   │   │   ├── perfect-scrollbar.jquery.min.js
│   │   │   │   └── sweetalert2.js
│   │   │   └── quill.min.js
│   │   └── scss
│   │       ├── material-dashboard
│   │       │   ├── _alerts.scss
│   │       │   ├── _badges.scss
│   │       │   ├── bootstrap
│   │       │   │   └── scss
│   │       │   │       ├── _alert.scss
│   │       │   │       ├── _badge.scss
│   │       │   │       ├── bootstrap-grid.scss
│   │       │   │       ├── bootstrap-reboot.scss
│   │       │   │       ├── bootstrap.scss
│   │       │   │       ├── _breadcrumb.scss
│   │       │   │       ├── _button-group.scss
│   │       │   │       ├── _buttons.scss
│   │       │   │       ├── _card.scss
│   │       │   │       ├── _carousel.scss
│   │       │   │       ├── _close.scss
│   │       │   │       ├── _code.scss
│   │       │   │       ├── _custom-forms.scss
│   │       │   │       ├── _dropdown.scss
│   │       │   │       ├── _forms.scss
│   │       │   │       ├── _functions.scss
│   │       │   │       ├── _grid.scss
│   │       │   │       ├── _images.scss
│   │       │   │       ├── _input-group.scss
│   │       │   │       ├── _jumbotron.scss
│   │       │   │       ├── _list-group.scss
│   │       │   │       ├── _media.scss
│   │       │   │       ├── mixins
│   │       │   │       │   ├── _alert.scss
│   │       │   │       │   ├── _background-variant.scss
│   │       │   │       │   ├── _badge.scss
│   │       │   │       │   ├── _border-radius.scss
│   │       │   │       │   ├── _box-shadow.scss
│   │       │   │       │   ├── _breakpoints.scss
│   │       │   │       │   ├── _buttons.scss
│   │       │   │       │   ├── _caret.scss
│   │       │   │       │   ├── _clearfix.scss
│   │       │   │       │   ├── _float.scss
│   │       │   │       │   ├── _forms.scss
│   │       │   │       │   ├── _gradients.scss
│   │       │   │       │   ├── _grid-framework.scss
│   │       │   │       │   ├── _grid.scss
│   │       │   │       │   ├── _hover.scss
│   │       │   │       │   ├── _image.scss
│   │       │   │       │   ├── _list-group.scss
│   │       │   │       │   ├── _lists.scss
│   │       │   │       │   ├── _navbar-align.scss
│   │       │   │       │   ├── _nav-divider.scss
│   │       │   │       │   ├── _pagination.scss
│   │       │   │       │   ├── _reset-text.scss
│   │       │   │       │   ├── _resize.scss
│   │       │   │       │   ├── _screen-reader.scss
│   │       │   │       │   ├── _size.scss
│   │       │   │       │   ├── _table-row.scss
│   │       │   │       │   ├── _text-emphasis.scss
│   │       │   │       │   ├── _text-hide.scss
│   │       │   │       │   ├── _text-truncate.scss
│   │       │   │       │   ├── _transition.scss
│   │       │   │       │   └── _visibility.scss
│   │       │   │       ├── _mixins.scss
│   │       │   │       ├── _modal.scss
│   │       │   │       ├── _navbar.scss
│   │       │   │       ├── _nav.scss
│   │       │   │       ├── _pagination.scss
│   │       │   │       ├── _popover.scss
│   │       │   │       ├── _print.scss
│   │       │   │       ├── _progress.scss
│   │       │   │       ├── _reboot.scss
│   │       │   │       ├── _root.scss
│   │       │   │       ├── _tables.scss
│   │       │   │       ├── _tooltip.scss
│   │       │   │       ├── _transitions.scss
│   │       │   │       ├── _type.scss
│   │       │   │       ├── utilities
│   │       │   │       │   ├── _align.scss
│   │       │   │       │   ├── _background.scss
│   │       │   │       │   ├── _borders.scss
│   │       │   │       │   ├── _clearfix.scss
│   │       │   │       │   ├── _display.scss
│   │       │   │       │   ├── _embed.scss
│   │       │   │       │   ├── _flex.scss
│   │       │   │       │   ├── _float.scss
│   │       │   │       │   ├── _position.scss
│   │       │   │       │   ├── _screenreaders.scss
│   │       │   │       │   ├── _sizing.scss
│   │       │   │       │   ├── _spacing.scss
│   │       │   │       │   ├── _text.scss
│   │       │   │       │   └── _visibility.scss
│   │       │   │       ├── _utilities.scss
│   │       │   │       └── _variables.scss
│   │       │   ├── _buttons.scss
│   │       │   ├── cards
│   │       │   │   ├── _card-background.scss
│   │       │   │   ├── _card-blog.scss
│   │       │   │   ├── _card-collapse.scss
│   │       │   │   ├── _card-contact.scss
│   │       │   │   ├── _card-form-horizontal.scss
│   │       │   │   ├── _card-plain-extend.scss
│   │       │   │   ├── _card-plain.scss
│   │       │   │   ├── _card-pricing.scss
│   │       │   │   ├── _card-product.scss
│   │       │   │   ├── _card-profile.scss
│   │       │   │   ├── _card-rotate.scss
│   │       │   │   ├── _card-signup.scss
│   │       │   │   ├── _card-stats.scss
│   │       │   │   └── _card-testimonials.scss
│   │       │   ├── _cards.scss
│   │       │   ├── _checkboxes.scss
│   │       │   ├── _core-bootstrap.scss
│   │       │   ├── _dropdown.scss
│   │       │   ├── _example-pages.scss
│   │       │   ├── _fixed-plugin.scss
│   │       │   ├── _footers-extend.scss
│   │       │   ├── _footers.scss
│   │       │   ├── _forms-extend.scss
│   │       │   ├── _forms.scss
│   │       │   ├── _headers.scss
│   │       │   ├── _images.scss
│   │       │   ├── _info-areas.scss
│   │       │   ├── _input-group.scss
│   │       │   ├── _misc-extend.scss
│   │       │   ├── _misc.scss
│   │       │   ├── mixins
│   │       │   │   ├── _alert.scss
│   │       │   │   ├── _animations.scss
│   │       │   │   ├── _breakpoints.scss
│   │       │   │   ├── _buttons.scss
│   │       │   │   ├── _chartist.scss
│   │       │   │   ├── _colored-shadows.scss
│   │       │   │   ├── _drawer.scss
│   │       │   │   ├── _forms.scss
│   │       │   │   ├── _hover.scss
│   │       │   │   ├── _layout.scss
│   │       │   │   ├── _navbar-colors.scss
│   │       │   │   ├── _navs.scss
│   │       │   │   ├── _sidebar-color.scss
│   │       │   │   ├── _social-buttons.scss
│   │       │   │   ├── _transparency.scss
│   │       │   │   ├── _type.scss
│   │       │   │   ├── _utilities.scss
│   │       │   │   ├── _variables.scss
│   │       │   │   └── _vendor-prefixes.scss
│   │       │   ├── _mixins.scss
│   │       │   ├── _modal.scss
│   │       │   ├── _navbar.scss
│   │       │   ├── _pages.scss
│   │       │   ├── _pagination.scss
│   │       │   ├── _pills.scss
│   │       │   ├── plugins
│   │       │   │   ├── _animate.scss
│   │       │   │   ├── _chartist.scss
│   │       │   │   ├── _datatables.net.scss
│   │       │   │   ├── _fullcalendar.scss
│   │       │   │   ├── _jquery.jvectormap.scss
│   │       │   │   ├── _perfect-scrollbar.scss
│   │       │   │   ├── _plugin-bootstrap-select.scss
│   │       │   │   ├── _plugin-datetime-picker.scss
│   │       │   │   ├── _plugin-fileupload.scss
│   │       │   │   ├── _plugin-flexisel.scss
│   │       │   │   ├── _plugin-nouislider.scss
│   │       │   │   ├── _plugin-tagsinput.scss
│   │       │   │   ├── _sweetalert2.scss
│   │       │   │   └── _wizard-card.scss
│   │       │   ├── _popover.scss
│   │       │   ├── _popups.scss
│   │       │   ├── _progress.scss
│   │       │   ├── _radios.scss
│   │       │   ├── _responsive.scss
│   │       │   ├── _ripples.scss
│   │       │   ├── _rtl.scss
│   │       │   ├── _sidebar-and-main-panel.scss
│   │       │   ├── _social-buttons.scss
│   │       │   ├── _tables.scss
│   │       │   ├── _tabs.scss
│   │       │   ├── _timeline.scss
│   │       │   ├── _togglebutton.scss
│   │       │   ├── _tooltip.scss
│   │       │   ├── _type.scss
│   │       │   ├── variables
│   │       │   │   ├── _body.scss
│   │       │   │   ├── _bootstrap-material-design-base.scss
│   │       │   │   ├── _bootstrap-material-design.scss
│   │       │   │   ├── _brand.scss
│   │       │   │   ├── _buttons.scss
│   │       │   │   ├── _card.scss
│   │       │   │   ├── _code.scss
│   │       │   │   ├── _colors-map.scss
│   │       │   │   ├── _colors.scss
│   │       │   │   ├── _custom-forms.scss
│   │       │   │   ├── _drawer.scss
│   │       │   │   ├── _dropdown.scss
│   │       │   │   ├── _forms.scss
│   │       │   │   ├── _layout.scss
│   │       │   │   ├── _list-group.scss
│   │       │   │   ├── _menu.scss
│   │       │   │   ├── _modals.scss
│   │       │   │   ├── _nav.scss
│   │       │   │   ├── _pagination.scss
│   │       │   │   ├── _shadow.scss
│   │       │   │   ├── _snackbar.scss
│   │       │   │   ├── _spacing.scss
│   │       │   │   ├── _state.scss
│   │       │   │   ├── _tables.scss
│   │       │   │   ├── _tooltip.scss
│   │       │   │   └── _type.scss
│   │       │   └── _variables.scss
│   │       ├── material-dashboard.scss
│   │       ├── material-kit
│   │       │   ├── _alerts.scss
│   │       │   ├── _badges.scss
│   │       │   ├── bootstrap
│   │       │   │   └── scss
│   │       │   │       ├── _alert.scss
│   │       │   │       ├── _badge.scss
│   │       │   │       ├── bootstrap-grid.scss
│   │       │   │       ├── bootstrap-reboot.scss
│   │       │   │       ├── bootstrap.scss
│   │       │   │       ├── _breadcrumb.scss
│   │       │   │       ├── _button-group.scss
│   │       │   │       ├── _buttons.scss
│   │       │   │       ├── _card.scss
│   │       │   │       ├── _carousel.scss
│   │       │   │       ├── _close.scss
│   │       │   │       ├── _code.scss
│   │       │   │       ├── _custom-forms.scss
│   │       │   │       ├── _dropdown.scss
│   │       │   │       ├── _forms.scss
│   │       │   │       ├── _functions.scss
│   │       │   │       ├── _grid.scss
│   │       │   │       ├── _images.scss
│   │       │   │       ├── _input-group.scss
│   │       │   │       ├── _jumbotron.scss
│   │       │   │       ├── _list-group.scss
│   │       │   │       ├── _media.scss
│   │       │   │       ├── mixins
│   │       │   │       │   ├── _alert.scss
│   │       │   │       │   ├── _background-variant.scss
│   │       │   │       │   ├── _badge.scss
│   │       │   │       │   ├── _border-radius.scss
│   │       │   │       │   ├── _box-shadow.scss
│   │       │   │       │   ├── _breakpoints.scss
│   │       │   │       │   ├── _buttons.scss
│   │       │   │       │   ├── _caret.scss
│   │       │   │       │   ├── _clearfix.scss
│   │       │   │       │   ├── _float.scss
│   │       │   │       │   ├── _forms.scss
│   │       │   │       │   ├── _gradients.scss
│   │       │   │       │   ├── _grid-framework.scss
│   │       │   │       │   ├── _grid.scss
│   │       │   │       │   ├── _hover.scss
│   │       │   │       │   ├── _image.scss
│   │       │   │       │   ├── _list-group.scss
│   │       │   │       │   ├── _lists.scss
│   │       │   │       │   ├── _navbar-align.scss
│   │       │   │       │   ├── _nav-divider.scss
│   │       │   │       │   ├── _pagination.scss
│   │       │   │       │   ├── _reset-text.scss
│   │       │   │       │   ├── _resize.scss
│   │       │   │       │   ├── _screen-reader.scss
│   │       │   │       │   ├── _size.scss
│   │       │   │       │   ├── _table-row.scss
│   │       │   │       │   ├── _text-emphasis.scss
│   │       │   │       │   ├── _text-hide.scss
│   │       │   │       │   ├── _text-truncate.scss
│   │       │   │       │   ├── _transition.scss
│   │       │   │       │   └── _visibility.scss
│   │       │   │       ├── _mixins.scss
│   │       │   │       ├── _modal.scss
│   │       │   │       ├── _navbar.scss
│   │       │   │       ├── _nav.scss
│   │       │   │       ├── _pagination.scss
│   │       │   │       ├── _popover.scss
│   │       │   │       ├── _print.scss
│   │       │   │       ├── _progress.scss
│   │       │   │       ├── _reboot.scss
│   │       │   │       ├── _root.scss
│   │       │   │       ├── _tables.scss
│   │       │   │       ├── _tooltip.scss
│   │       │   │       ├── _transitions.scss
│   │       │   │       ├── _type.scss
│   │       │   │       ├── utilities
│   │       │   │       │   ├── _align.scss
│   │       │   │       │   ├── _background.scss
│   │       │   │       │   ├── _borders.scss
│   │       │   │       │   ├── _clearfix.scss
│   │       │   │       │   ├── _display.scss
│   │       │   │       │   ├── _embed.scss
│   │       │   │       │   ├── _flex.scss
│   │       │   │       │   ├── _float.scss
│   │       │   │       │   ├── _position.scss
│   │       │   │       │   ├── _screenreaders.scss
│   │       │   │       │   ├── _sizing.scss
│   │       │   │       │   ├── _spacing.scss
│   │       │   │       │   ├── _text.scss
│   │       │   │       │   └── _visibility.scss
│   │       │   │       ├── _utilities.scss
│   │       │   │       └── _variables.scss
│   │       │   ├── _buttons.scss
│   │       │   ├── cards
│   │       │   │   ├── _card-background.scss
│   │       │   │   ├── _card-blog.scss
│   │       │   │   ├── _card-carousel.scss
│   │       │   │   ├── _card-collapse.scss
│   │       │   │   ├── _card-contact.scss
│   │       │   │   ├── _card-form-horizontal.scss
│   │       │   │   ├── _card-login.scss
│   │       │   │   ├── _card-plain.scss
│   │       │   │   ├── _card-pricing.scss
│   │       │   │   ├── _card-product.scss
│   │       │   │   ├── _card-profile.scss
│   │       │   │   ├── _card-rotate.scss
│   │       │   │   └── _card-testimonials.scss
│   │       │   ├── _cards.scss
│   │       │   ├── _carousel.scss
│   │       │   ├── _checkboxes.scss
│   │       │   ├── _core-bootstrap.scss
│   │       │   ├── _custom-forms.scss
│   │       │   ├── _drawer.scss
│   │       │   ├── _dropdown.scss
│   │       │   ├── _example-pages-extend.scss
│   │       │   ├── _example-pages.scss
│   │       │   ├── _fileupload.scss
│   │       │   ├── _footers.scss
│   │       │   ├── _forms.scss
│   │       │   ├── _headers.scss
│   │       │   ├── _images.scss
│   │       │   ├── _info-areas.scss
│   │       │   ├── _input-group.scss
│   │       │   ├── _layout.scss
│   │       │   ├── _list-group.scss
│   │       │   ├── _media.scss
│   │       │   ├── _misc-extend.scss
│   │       │   ├── _misc.scss
│   │       │   ├── mixins
│   │       │   │   ├── _alert.scss
│   │       │   │   ├── _animations.scss
│   │       │   │   ├── _breakpoints.scss
│   │       │   │   ├── _buttons.scss
│   │       │   │   ├── _colored-shadows.scss
│   │       │   │   ├── _drawer.scss
│   │       │   │   ├── _forms.scss
│   │       │   │   ├── _hover.scss
│   │       │   │   ├── _layout.scss
│   │       │   │   ├── _navbar-colors.scss
│   │       │   │   ├── _navs.scss
│   │       │   │   ├── _type.scss
│   │       │   │   └── _utilities.scss
│   │       │   ├── _mixins.scss
│   │       │   ├── _modal-extend.scss
│   │       │   ├── _modal.scss
│   │       │   ├── _navbar.scss
│   │       │   ├── _nav.scss
│   │       │   ├── _pagination.scss
│   │       │   ├── _pills.scss
│   │       │   ├── plugins
│   │       │   │   ├── _plugin-bootstrap-select.scss
│   │       │   │   ├── _plugin-datetime-picker.scss
│   │       │   │   ├── _plugin-flexisel.scss
│   │       │   │   ├── _plugin-nouislider.scss
│   │       │   │   └── _plugin-tagsinput.scss
│   │       │   ├── _popover.scss
│   │       │   ├── _progress.scss
│   │       │   ├── _radios.scss
│   │       │   ├── _reboot.scss
│   │       │   ├── _responsive.scss
│   │       │   ├── _ripples.scss
│   │       │   ├── sections
│   │       │   │   ├── _blogs.scss
│   │       │   │   ├── _contactus.scss
│   │       │   │   ├── _features.scss
│   │       │   │   ├── _footers-extend.scss
│   │       │   │   ├── _headers-extend.scss
│   │       │   │   ├── _pricing.scss
│   │       │   │   ├── _projects.scss
│   │       │   │   ├── _social-subscribe-lines.scss
│   │       │   │   ├── _team.scss
│   │       │   │   └── _testimonials.scss
│   │       │   ├── _sections.scss
│   │       │   ├── _social-buttons.scss
│   │       │   ├── _switches.scss
│   │       │   ├── _tables.scss
│   │       │   ├── _tabs.scss
│   │       │   ├── _togglebutton.scss
│   │       │   ├── _tooltip.scss
│   │       │   ├── _type.scss
│   │       │   ├── variables
│   │       │   │   ├── _body.scss
│   │       │   │   ├── _bootstrap-material-design-base.scss
│   │       │   │   ├── _bootstrap-material-design.scss
│   │       │   │   ├── _brand.scss
│   │       │   │   ├── _buttons.scss
│   │       │   │   ├── _card.scss
│   │       │   │   ├── _carousel.scss
│   │       │   │   ├── _code.scss
│   │       │   │   ├── _colors-map.scss
│   │       │   │   ├── _colors.scss
│   │       │   │   ├── _custom-forms.scss
│   │       │   │   ├── _drawer.scss
│   │       │   │   ├── _dropdown.scss
│   │       │   │   ├── _forms.scss
│   │       │   │   ├── _layout.scss
│   │       │   │   ├── _list-group.scss
│   │       │   │   ├── _menu.scss
│   │       │   │   ├── _modals.scss
│   │       │   │   ├── _nav.scss
│   │       │   │   ├── _pagination.scss
│   │       │   │   ├── _shadow.scss
│   │       │   │   ├── _snackbar.scss
│   │       │   │   ├── _spacing.scss
│   │       │   │   ├── _state.scss
│   │       │   │   ├── _tables.scss
│   │       │   │   ├── _tooltip.scss
│   │       │   │   └── _type.scss
│   │       │   └── _variables.scss
│   │       └── material-kit.scss
│   ├── quill
│   │   └── dist
│   │       ├── quill.bubble.css
│   │       ├── quill.core.css
│   │       ├── quill.core.js
│   │       ├── quill.min.js
│   │       └── quill.snow.css
│   └── robots.txt
├── README.md
├── resources
│   ├── lang
│   │   └── en
│   │       ├── auth.php
│   │       ├── pagination.php
│   │       ├── passwords.php
│   │       └── validation.php
│   └── views
│       ├── auth
│       │   ├── login.blade.php
│       │   ├── passwords
│       │   │   ├── email.blade.php
│       │   │   └── reset.blade.php
│       │   ├── register.blade.php
│       │   └── verify.blade.php
│       ├── blog
│       │   ├── all_articles.blade.php
│       │   ├── app.blade.php
│       │   ├── articles_author.blade.php
│       │   ├── articles_category.blade.php
│       │   ├── articles_search.blade.php
│       │   ├── articles_tag.blade.php
│       │   ├── home.blade.php
│       │   ├── layouts
│       │   │   ├── footer.blade.php
│       │   │   ├── header.blade.php
│       │   │   └── navs
│       │   │       ├── nav.blade.php
│       │   │       └── nav_categories.blade.php
│       │   ├── _partials
│       │   │   ├── article_full.blade.php
│       │   │   ├── authors.blade.php
│       │   │   ├── comments.blade.php
│       │   │   ├── featured_articles.blade.php
│       │   │   ├── latest_articles.blade.php
│       │   │   ├── pagination.blade.php
│       │   │   └── reply_comments.blade.php
│       │   └── show.blade.php
│       ├── dashboard
│       │   ├── alerts
│       │   │   ├── errors.blade.php
│       │   │   ├── feedback.blade.php
│       │   │   └── migrations_check.blade.php
│       │   ├── articles
│       │   │   ├── create.blade.php
│       │   │   ├── edit.blade.php
│       │   │   └── index.blade.php
│       │   ├── categories
│       │   │   ├── create.blade.php
│       │   │   ├── edit.blade.php
│       │   │   └── index.blade.php
│       │   ├── errors
│       │   │   ├── 401.blade.php
│       │   │   ├── 403.blade.php
│       │   │   ├── 404.blade.php
│       │   │   ├── 419.blade.php
│       │   │   ├── 429.blade.php
│       │   │   ├── 500.blade.php
│       │   │   ├── 503.blade.php
│       │   │   └── layout.blade.php
│       │   ├── layouts
│       │   │   ├── app.blade.php
│       │   │   ├── footers
│       │   │   │   ├── auth.blade.php
│       │   │   │   └── guest.blade.php
│       │   │   ├── navbars
│       │   │   │   ├── navs
│       │   │   │   │   ├── auth.blade.php
│       │   │   │   │   └── guest.blade.php
│       │   │   │   └── sidebar.blade.php
│       │   │   ├── page_templates
│       │   │   │   ├── auth.blade.php
│       │   │   │   └── guest.blade.php
│       │   │   └── plugin
│       │   │       └── fixed-plugin.blade.php
│       │   ├── pages
│       │   │   ├── calendar.blade.php
│       │   │   ├── charts.blade.php
│       │   │   ├── components
│       │   │   │   ├── buttons.blade.php
│       │   │   │   ├── grid.blade.php
│       │   │   │   ├── icons.blade.php
│       │   │   │   ├── notifications.blade.php
│       │   │   │   ├── panels.blade.php
│       │   │   │   ├── sweet_alert.blade.php
│       │   │   │   └── typography.blade.php
│       │   │   ├── dashboard.blade.php
│       │   │   ├── example_pages
│       │   │   │   ├── error.blade.php
│       │   │   │   ├── language.blade.php
│       │   │   │   ├── lock.blade.php
│       │   │   │   ├── pricing.blade.php
│       │   │   │   └── timeline.blade.php
│       │   │   ├── forms
│       │   │   │   ├── form_extended.blade.php
│       │   │   │   ├── form_regular.blade.php
│       │   │   │   ├── form_validation.blade.php
│       │   │   │   └── form_wizard.blade.php
│       │   │   ├── maps
│       │   │   │   ├── maps_fullscreen.blade.php
│       │   │   │   ├── maps_google.blade.php
│       │   │   │   └── maps_vector.blade.php
│       │   │   ├── tables
│       │   │   │   ├── table_list.blade.php
│       │   │   │   ├── tables_datatable.blade.php
│       │   │   │   ├── tables_extended.blade.php
│       │   │   │   └── tables_regular.blade.php
│       │   │   ├── welcome.blade.php
│       │   │   └── widgets.blade.php
│       │   ├── profile
│       │   │   └── edit.blade.php
│       │   ├── roles
│       │   │   ├── create.blade.php
│       │   │   ├── edit.blade.php
│       │   │   └── index.blade.php
│       │   ├── tags
│       │   │   ├── create.blade.php
│       │   │   ├── edit.blade.php
│       │   │   └── index.blade.php
│       │   └── users
│       │       ├── create.blade.php
│       │       ├── edit.blade.php
│       │       └── index.blade.php
│       ├── home.blade.php
│       ├── page.html
│       └── vendor
│           └── pagination
│               ├── bootstrap-4.blade.php
│               ├── default.blade.php
│               ├── semantic-ui.blade.php
│               ├── simple-bootstrap-4.blade.php
│               └── simple-default.blade.php
├── routes
│   ├── api.php
│   ├── channels.php
│   ├── console.php
│   └── web.php
├── screens
│   ├── Dashboard.png
│   ├── Login.png
│   ├── Profile.png
│   ├── Register.png
│   ├── Tables.png
│   └── Users.png
├── server.php
├── storage
│   ├── app
│   │   ├── .gitignore
│   │   └── public
│   │       ├── .gitignore
│   │       ├── pictures
│   │       └── profile
│   ├── framework
│   │   ├── cache
│   │   │   ├── data
│   │   │   │   └── .gitignore
│   │   │   └── .gitignore
│   │   ├── .gitignore
│   │   ├── sessions
│   │   ├── testing
│   │   │   └── .gitignore
│   │   └── views
│   │       └── .gitignore
│   └── logs
│       ├── .gitignore
├── tests
│   ├── CreatesApplication.php
│   ├── Feature
│   │   └── ExampleTest.php
│   ├── TestCase.php
│   └── Unit
│       └── ExampleTest.php
├── .vscode
│   └── settings.json
├── webpack.mix.js
└── yarn.lock
```


## Browser Support

At present, we officially aim to support the last two versions of the following browsers:

<img src="https://github.com/creativetimofficial/public-assets/blob/master/logos/chrome-logo.png?raw=true" width="64" height="64"> <img src="https://raw.githubusercontent.com/creativetimofficial/public-assets/master/logos/firefox-logo.png" width="64" height="64"> <img src="https://raw.githubusercontent.com/creativetimofficial/public-assets/master/logos/edge-logo.png" width="64" height="64"> <img src="https://raw.githubusercontent.com/creativetimofficial/public-assets/master/logos/safari-logo.png" width="64" height="64"> <img src="https://raw.githubusercontent.com/creativetimofficial/public-assets/master/logos/opera-logo.png" width="64" height="64">


## Resources
- Demo: <https://material-blog-pro-laravel.creative-tim.com/?ref=mbl-readme>
- Download Page: <https://www.creative-tim.com/product/material-blog-pro-laravel?ref=mbl-readme>
- Documentation: <https://material-blog-pro-laravel.creative-tim.com/docs/material-blog-pro-laravel/docs/getting-started/laravel-setup.html?ref=mbl-readme>
- License Agreement: <https://www.creative-tim.com/license>
- Support: <https://www.creative-tim.com/contact-us>
- Issues: [Github Issues Page](https://github.com/creativetimofficial/ct-material-blog-pro-laravel/issues)
- **Dashboards:**

| LARAVEL |
| --- |
| [![Material Blog PRO Laravel](https://s3.amazonaws.com/creativetim_bucket/products/158/thumb/opt_mdp_laravel_thumbnail.jpg)](https://material-blog-pro-laravel.creative-tim.com/?ref=mbl-readme)

## Change log

Please see the [changelog](CHANGELOG.md) for more information on what has changed recently.

## Credits

- [Creative Tim](https://creative-tim.com/?ref=mbl-readme)
- [UPDIVISION](https://updivision.com)

## Reporting Issues

We use GitHub Issues as the official bug tracker for the Material Blog PROLaravel. Here are some advices for our users that want to report an issue:

1. Make sure that you are using the latest version of the Material Blog PRO Laravel. Check the CHANGELOG from your dashboard on our [website](https://www.creative-tim.com/?ref=mbl-readme).
2. Providing us reproducible steps for the issue will shorten the time it takes for it to be fixed.
3. Some issues may be browser specific, so specifying in what browser you encountered the issue might help.

## Licensing

- Copyright 2019 Creative Tim (https://www.creative-tim.com/?ref=mbl-readme)
- Creative Tim License (https://www.creative-tim.com/license).


## Useful Links

- [Tutorials](https://www.youtube.com/channel/UCVyTG4sCw-rOvB9oHkzZD1w)
- [Affiliate Program](https://www.creative-tim.com/affiliates/new) (earn money)
- [Blog Creative Tim](http://blog.creative-tim.com/)
- [Free Products](https://www.creative-tim.com/bootstrap-themes/free) from Creative Tim
- [Premium Products](https://www.creative-tim.com/bootstrap-themes/premium?ref=mbl-readme) from Creative Tim
- [React Products](https://www.creative-tim.com/bootstrap-themes/react-themes?ref=mbl-readme) from Creative Tim
- [Angular Products](https://www.creative-tim.com/bootstrap-themes/angular-themes?ref=mbl-readme) from Creative Tim
- [VueJS Products](https://www.creative-tim.com/bootstrap-themes/vuejs-themes?ref=mbl-readme) from Creative Tim
- [More products](https://www.creative-tim.com/bootstrap-themes?ref=mbl-readme) from Creative Tim
- Check our Bundles [here](https://www.creative-tim.com/bundles??ref=mbl-readme)

## Social Media

### Creative Tim:

Twitter: <https://twitter.com/CreativeTim?ref=mbl-readme>

Facebook: <https://www.facebook.com/CreativeTim?ref=mbl-readme>

Dribbble: <https://dribbble.com/creativetim?ref=mbl-readme>

Instagram: <https://www.instagram.com/CreativeTimOfficial?ref=mbl-readme>


### Updivision:

Twitter: <https://twitter.com/updivision?ref=mbl-readme>

Facebook: <https://www.facebook.com/updivision?ref=mbl-readme>

Linkedin: <https://www.linkedin.com/company/updivision?ref=mbl-readme>

Updivision Blog: <https://updivision.com/blog/?ref=mbl-readme>

## Credits

- [Creative Tim](https://creative-tim.com/?ref=mbl-readme)
- [UPDIVISION](https://updivision.com)
