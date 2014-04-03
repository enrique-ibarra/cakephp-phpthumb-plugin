phpThumb() plugin for CakePHP
=============================

This CakePHP plugin uses phpThumb() to dynamically generate thumbnails of images.
It is configurable for using with images uploaded with MeioUpload.

Installation
------------

Clone in plugin directory :

    git clone git://github.com/msadouni/cakephp-phpthumb-plugin.git php_thumb

Or download an archive and extract in `plugins/php_thumb`

Configuration
-------------

Create `config/config.php` with the following code :

    <?php
    $config['PhpThumb']['thumbs_path'] = 'thumbs';

Load `config/config.php` :

    // config/bootstrap.php
    Configure::load('config');

Then create `webroot/thumbs` where the thumbs will be stored.
You might want to add that folder to your .gitignore.

You can also set configuration variables usually found in `phpThumb.config.php`:

    // Config/config.php
    $config['PhpThumb']['config'] = array(
        'allow_src_above_docroot' => true
    );

For a list of available variables, consult `vendors/phpThumb/phpThumb.config.php.default`.

MeioUpload configuration
------------------------

This is only if you want to use MeioUpload to take care of the upload part.

You only need the basic MeioUpload configuration to upload the original image,
since all the thumbnails stuff is done dynamically with this plugin.

In your model :

    var $actsAs = array('MeioUpload.MeioUpload' => array('image'));

Add a varchar image field to the model, create `webroot/uploads` with the write
permissions for you webserver, and you're done.
For more information on MeioUpload, check [the github page](https://github.com/jrbasso/MeioUpload).

This plugin works out of the box with the default MeioUpload configuration,
in which images are stored in `webroot/uploads`with a `model/field` structure.

If you use a different folder than `uploads`, then add

    $config['PhpThumb']['meioupload_path'] = 'my-folder';

to `config/config.php`.

Usage
-----

Load the helper in `AppController` or in each controller where you want to use the helper :

    var $helpers = array('PhpThumb.PhpThumb');

### Normal image

In the view, generate the thumbnail :

    echo $this->PhpThumb->thumbnail('img/image.jpg', array(
        'w' => 100, 'h' => 100, 'zc' => 1
    ));

In the `$options` array you can use any valid phpThumb() option. For detailed
information on the available options, check [phpThumb()'s readme](http://phpthumb.sourceforge.net/demo/docs/phpthumb.readme.txt)

You can also pass HTML options :

    echo $this->PhpThumb->thumbnail(
        'img/image.jpg',
        array('w' => 100, 'h' => 100, 'zc' => 1),
        array('alt' => "Alternative text")
    );

### MeioUpload'ed image

    // for a Post with an image field containing my-post.jpg
    echo $this->PhpThumb->thumbnail($post['Post']['image'], array(
        'model' => 'Post', 'field' => 'image', 'w' => 100, 'h' => 100, 'zc' => 1
    ));
    // will render a thumbnail for this post image in /uploads/post/image/my-post.jpg

This will also work :

    echo $this->PhpThumb->thumbnail('uploads/post/image/.$post['Post']['image'], array(
        'w' => 100, 'h' => 100, 'zc' => 1
    ));

### Image url

To get only the image url, use the `url` method :

    echo $this->PhpThumb->url('img/image.jpg', array(
        'w' => 100, 'h' => 100, 'zc' => 1
    ));

It's useful for the various Javascript gallery script, like Lightbox / Thickbox / Colorbox etc. :

    echo $this->Html->link(
        $this->PhpThumb->thumbnail(
            'img/image.jpeg', array('w' => 100, 'h' => 100, 'zc' => 1)
        ),
        $this->PhpThumb->url('img/image.jpeg', array('w' => 640, 'h' => 640)),
        array('escape' => false)
    );
