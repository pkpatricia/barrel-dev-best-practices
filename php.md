### Barrel Development Best Practices

PHP Best Practices
------------------

### Syntax

#### Short open tags

[PHP tags](http://www.php.net/manual/en/language.basic-syntax.phptags.php)

Typically, it is best practice to use the full `<?php` open tag verses the short open tag `<?`. This is because the abbreviated version requires `short_open_tag = On` in php.ini, and access to php.ini may not be available in the production environment.

Use of the short echo tag, `<?=`, reqires activation only in PHP instances < 5.4, and can be safely used on any server using PHP 5.4 or where `short_open_tag` is set to true.

#### Accidental whitespace

Files that are pure PHP should have `<?php` on line one and should not have any new lines or white space at the top of the file. It is also best practice to omit the closing PHP tag at the end of the file in order to avoid the output buffer from sending the output unintentially.

#### Templating with PHP

[Alternate syntax for control structures](http://us1.php.net/alternative_syntax)

When templating with PHP, it is best practice to use alternate syntax for the `if`, `while`, `for`, `foreach`, and `switch` control structures. Generic `}` closing tags are non-descriptive, and use of alternate syntax helps clarify the end of events. For example:
```
<section id="Blog">
  <?php if ($articles){ ?>
    <?php foreach ($articles as $article){ ?>
      <h3><?= $article['title']; ?></h3>
      <?= $article['content']; ?>
    <?php } ?>
    
    <?php if (!is_home()){ ?>
      <p><a href="#home" title="Home">Go home</a></p>
    <?php } ?>
  <?php } ?>
</section>
```
Is better written as:
```
<section id="Blog">
  <?php if ($articles): ?>
    <?php foreach ($articles as $article): ?>
      <h3><?= $article['title']; ?></h3>
      <?= $article['content']; ?>
    <?php endforeach; ?>
    
    <?php if (!is_home()): ?>
      <p><a href="#home" title="Home">Go home</a></p>
    <?php endif; ?>
  <?php endif; ?>
</section>
```









