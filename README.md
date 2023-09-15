[![PHPUnit tests](https://github.com/dompdf/php-font-lib/actions/workflows/phpunit.yml/badge.svg)](https://github.com/dompdf/php-font-lib/actions/workflows/phpunit.yml)

# PHP Font Lib

This library can be used to:
 * Read TrueType, OpenType (with TrueType glyphs), WOFF font files
 * Extract basic info (name, style, etc)
 * Extract advanced info (horizontal metrics, glyph names, glyph shapes, etc)
 * Make an Adobe Font Metrics (AFM) file from a font

This project was initiated by the need to read font files in the [DOMPDF project](https://github.com/dompdf/dompdf).

### This version
Version to temporary fix the `unpack(): Type n: not enough input, need 2, have 0`
issue descibed in https://github.com/dompdf/php-font-lib/issues/47: 


Add these lines to your `composer.json` to force replacing the original
php-font-lib with this one
```
  "repositories": [
        {
            "type": "vcs",
            "url": "https://github.com/bmellink/php-font-lib"
        },
        ...
    ],

    "require": {
        "bmellink/php-font-lib": "*",
        ...
    }
```


Usage Example
-------------

### Base font information
```
$font = \FontLib\Font::load('fontfile.ttf');
$font->parse();  // for getFontWeight() to work this call must be done first!
echo $font->getFontName() .'<br>';
echo $font->getFontSubfamily() .'<br>';
echo $font->getFontSubfamilyID() .'<br>';
echo $font->getFontFullName() .'<br>';
echo $font->getFontVersion() .'<br>';
echo $font->getFontWeight() .'<br>';
echo $font->getFontPostscriptName() .'<br>';
$font->close();
```

### Font Metrics Generation
```
$font = FontLib\Font::load('fontfile.ttf');
$font->parse();
$font->saveAdobeFontMetrics('fontfile.ufm');
```

### Create a font subset
```
$font = FontLib\Font::load('fontfile.ttf');
$font->parse();
$font->setSubset("abcdefghijklmnopqrstuvwxyz ABCDEFGHIJKLMNOPQRSTUVWXYZ.:,;' (!?)+-*/== 1234567890"); // characters to include
$font->reduce();
touch('fontfile.subset.ttf');
$font->open('fontfile.subset.ttf', FontLib\BinaryStream::modeReadWrite);
$font->encode(array("OS/2"));
$font->close();
```
