# github-theme-updater

This is a simple class that lets you use github releases in your repository to handle your theme updates.
A more robust solution would be to use the [github-updater](https://github.com/afragen/github-updater) plugin, but I simply refuse to include a 540kb library in my theme when I can accomplish what I need in less than 200 lines of code.  
Is it perfect? No. Does it get the job done? Yes.

## Implementing in the theme:

1. Download the php file from this repo and include it in your theme, or require it using composer:
	```bash
	composer require aristath/github-theme-updater
	```
2. Change the namespace on the top of the file to match the one you use in your theme (you are using namespaces, right?).
3. Include the updater class in your functions.php file:
	```php
	add_action( 'after_setup_theme', function() {
		get_template_part( 'inc/classes/Updater' );
	});
	```
4. At the bottom of the `Updater.php` file init the updater:
	```php
	new Updater(
		[
			'name' => 'Gridd',                     // Theme Name.
			'repo' => 'wplemon/gridd',             // Theme repository.
			'slug' => 'gridd',                     // Theme Slug.
			'url'  => 'https://wplemon.com/gridd', // Theme URL.
			'ver'  => 1.2                          // Theme Version.
		]
	);
	```

## Releasing an update on Github

When you want to release an update you can simply go to your repository > Releases and create a new release.  
* The version should either be formatted as `1.2`, `1.2.3`, `v1.2` or `v1.2.3` etc. The version comparison will remove the `v` and the script will be able to compare versions. However, if you call your release-tag `tomato` don't expect it to work.
* You **have to upload a zip file.**. When creating your release, upload a `.zip` file in there. The default packages created by github have the version number inside the folder-name, and that has the potential to break things on user sites when updating. **This updater script will only detect the file uploaded as an asset manually by you**.

## Advice

* Use at your own risk. If you don't follow the instructions above, things will break.
* This script is as simple as it can be. If you find a bug, pull-requests are more than welcomed.

## Why was this created?

This project was created out of necessity for a theme submitted on wordpress.org. The theme-review process on wordpress.org takes a long time...

* The initial review is usually a coupe of months.
* If all goes well the theme proceeds to the final review which takes 2-4 months at the time of this writing.
* If your theme is tagged as accessibility-ready, then the theme has to wait for a 3rd review, and I currently have no idea how long that will take.

Overall a theme review can easily take more than 6 months.

In the meantime, life goes on. If your business depends on your themes, then you may want to implement an alternative method to provide updates for your theme so people can start using it and you can start growing.

## Releasing your theme on w.org

Remember to remove the script when uploading an update on w.org. When your theme goes live you can remove it from the repository as well, this is only meant as a stepping stone.

Until your theme goes live you can ignore the class inclusion if you did it using `get_template_part` as in the example above. If the file doesn't exist nothing bad will happen.

Personally I prefer using a `.gitattributes` file to exclude the development files like `.sass`, `.map`, `.editorconfig` etc. If you use such a file on your project, you can just add `Updater.php export-ignore` in there and it won't be included in your theme export when you get your build ready for w.org

That's all. I hope you enjoy using this and it makes your life somewhat easier.