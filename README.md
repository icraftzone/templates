# Templates

[![BracketSpace Micropackage](https://img.shields.io/badge/BracketSpace-Micropackage-brightgreen)](https://bracketspace.com)
[![Latest Stable Version](https://poser.pugx.org/micropackage/templates/v/stable)](https://packagist.org/packages/micropackage/templates)
[![PHP from Packagist](https://img.shields.io/packagist/php-v/micropackage/templates.svg)](https://packagist.org/packages/micropackage/templates)
[![Total Downloads](https://poser.pugx.org/micropackage/templates/downloads)](https://packagist.org/packages/micropackage/templates)
[![License](https://poser.pugx.org/micropackage/templates/license)](https://packagist.org/packages/micropackage/templates)

## 🧬 About Templates

Templates micropackage is very simple WordPress template engine with multi-storage support. The templates are not parsed or cached like Blade or Twig templates. It's just good ol' file loader with variable support.

## 💾 Installation

``` bash
composer require micropackage/templates
```

## 🕹 Usage

Let's assume your template tree looks like this:

```
my-plugin/
├── admin/
│   └── templates/
│      ├── notice.php
│      └── settings.php
└── frontend/
    └── templates/
       ├── profile.php
       └── welcome.php
```

First, you need to define at least one template storage. In the above case we have two places with templates.

```php
Micropackage\Templates\Storage::add( 'admin', $plugin_dir . '/admin/templates' );
Micropackage\Templates\Storage::add( 'frontend', $plugin_dir . '/frontend/templates' );
```

Then you can easily render template:

```php
$template = new Micropackage\Templates\Template( 'frontend', 'profile', [
	'user_name' => $user_name,
	'posts'     => get_posts( [ 'author' => $user_id ] ),
] );

$template->render();
```

The template file could look like this:

```php+HTML
<p>Howdy, <?php $this->the( 'user_name' ); ?></p>

<p>See your posts:</p>

<ul>
	<?php foreach ( $this->get( 'posts' ) as $post ) : ?>
		<li><?php echo $post->post_title; ?></li>
	<?php endforeach; ?>
</ul>
```

### Accessing variables in the template file

In the template file, `$this` points to the template instance, which means you can access all the template methods.

The basic usage is:

```php
$this->the( 'var_name' ); // Prints the value.
$var_name = $this->get( 'var_name' ); // Gets the value.
```

But you can also use the shorthand closure methods:

```php
$the( 'var_name' ); // Prints the value.
$var_name = $get( 'var_name' ); // Gets the value.
```

### Available template methods

Template class methods.

| Method                                          | Description                       | Returns                                                      |
| ----------------------------------------------- | --------------------------------- | ------------------------------------------------------------ |
| ```get_path()```                                | Gets full path with extension     | *(string)*                                                   |
| ```get_rel_path()```                            | Gets relative path with extension | *(string)*                                                   |
| ```get_vars()```                                | Gets all variables                | *(array)*                                                    |
| ```clear_vars()```                              | Clears all variables              | `$this`                                                      |
| ```set( (sting) $var_name, (string) $value )``` | Sets the variable value           | `$this`                                                      |
| ```get( (sting) $var_name )```                  | Gets the variable value           | *(mixed\|null)*<br />Null if variable with given name wasn't set |
| ```the( (sting) $var_name )```                  | Prints the variable value         | void                                                         |
| ```remove( (sting) $var_name )```               | Removes the variable              | `$this`                                                      |
| ```render()```                                  | Renders the template              | void                                                         |
| ```output()```                                  | Outputs the template              | *(string)*                                                   |

### Template constructor params

```php
$template = new Micropackage\Templates\Template(
	$storage_name = 'frontend',
	$template_name = 'profile',
	$variables  = [
		'var_key' => $var_value,
	]
);
```

| Parameter            | Type         | Description                                                  |
| -------------------- | ------------ | ------------------------------------------------------------ |
| ```$storage_name```  | **Required** | Must match registered storage                                |
| ```$template_name``` | **Required** | Relative template path, example:<br />`user/section/profile` will be resolved to:<br />`$storage_path . '/user/section/profile.php'` |
| ```$variables```     | Optional     | Array of template variables in format:<br />`key => value`<br />Can be added later with `set()` method |


## 📦 About the Micropackage project

Micropackages - as the name suggests - are micro packages with a tiny bit of reusable code, helpful particularly in WordPress development.

The aim is to have multiple packages which can be put together to create something bigger by defining only the structure.

Micropackages are maintained by [BracketSpace](https://bracketspace.com).

## 📖 Changelog

[See the changelog file](./CHANGELOG.md).

## 📃 License

GNU General Public License (GPL) v3.0. See the [LICENSE](./LICENSE) file for more information.
