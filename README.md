# Laravel PDF: PDF generator for Laravel 5.* and MPDF 7.*


> Easily generate PDF documents from HTML right inside of Laravel using this PDF wrapper.

## Contents
- [Installation Guide](#installation-guide)
- [Configuration](#configuration)
- [Basic Usage](#basic-usage)
- [Headers and Footers](#headers-and-footers)
- [Included Fonts](#included-fonts)
- [Custom Fonts](#custom-fonts)
- [Set Protection](#set-protection)
- [Documentation](#documentation)


### Installation Guide

Require this package in your `composer.json` or install it by running:

```
composer require Angujo/laravel-pdf
```

This version will automatically set the alias for latest versions of Laravel >=5.5

####Laravel 5.4 and older
To start using in Laravel 5.4 and older, add the Service Provider and the Facade to your `config/app.php`:

```php
'providers' => [
	// ...
	Angujo\LaravelPDF\PdfServiceProvider::class
]
```

```php
'aliases' => [
	// ...
	'PDF' => Angujo\LaravelPDF\Facades\PDF::class
]
```

### Configuration
The defaults configuration settings are set in `config/pdf.php`. Copy this file to your own config directory to modify the values. You can publish the config using this command:

    php artisan vendor:publish --provider="Angujo\LaravelPDF\PdfServiceProvider"


### Basic Usage

To use Laravel PDF add something like this to one of your controllers. You can pass data to a view in `/resources/views`.

```php
use PDF;

function generate_pdf() {
	$data = [
		'foo' => 'bar'
	];
	$pdf = PDF::::Make();
	$pdf->loadView('pdf.document', $data);
	return $pdf->Stream('document.pdf');
}
```
or

```php
use Angujo\LaravelPDF\PDF;

function generate_pdf() {
	$data = [
		'foo' => 'bar'
	];
	$pdf = new PDF();
	$pdf->loadView('pdf.document', $data);
	return $pdf->Stream('document.pdf');
}

```

If you want to generate from html content:
```php
    $content = "Hello this is first pdf file."
	$pdf->loadHTML($content);
	return $pdf->Stream('document.pdf');
```

If you want to generate from files:
```php
    $file = "file.txt"
	$pdf->loadFile($file);
	return $pdf->Stream('document.pdf');
```

If you want download pdf file:
```php
	return $pdf->Embed('document.pdf');
```

If you want to save pdf to server:
```php
	return $pdf->Save('with-complete-path/document.pdf');
```

If you want add pdf file as attachment to email:
```php
	return $pdf->Embed('document.pdf');
```

### Headers and Footers

If you want to have headers and footers that appear on every page, add them to your `<body>` tag like this:

```html
<htmlpageheader name="page-header">
	Your Header Content
</htmlpageheader>

<htmlpagefooter name="page-footer">
	Your Footer Content
</htmlpagefooter>
```

Now you just need to define them with the name attribute in your CSS:

```css
@page {
	header: page-header;
	footer: page-footer;
}
```

Inside of headers and footers `{PAGENO}` can be used to display the page number.

### Included Fonts

By default you can use all the fonts [shipped with mPDF](https://mpdf.github.io/fonts-languages/available-fonts-v6.html).

### Custom Fonts

You can use your own fonts in the generated PDFs. The TTF files have to be located in one folder, e.g. `/resources/fonts/`. Add this to your configuration file (`/config/pdf.php`):
```php
    return [
	    'custom_font_path' => base_path('/resources/fonts/'), // don't forget the trailing slash!
    ];
```
And then:
```php
    $font_data = array(
        'examplefont' => [
            'R' => 'ExampleFont-Regular.ttf',      // regular font
            'B' => 'ExampleFont-Bold.ttf',         // optional: bold font
            'I' => 'ExampleFont-Italic.ttf',       // optional: italic font
            'BI' => 'ExampleFont-Bold-Italic.ttf', // optional: bold-italic font
        ]
        // ...add as many as you want.
    );

	$pdf->addCustomFont($font_data, true);
	// If your font file is unicode and "OpenType Layout" then set true. Default value is false.
```

Now you can use the font in CSS:

```css
body {
	font-family: 'examplefont', sans-serif;
}
```

### Set Protection

To set protection, you just call the `SetProtection()` method and pass an array with permissions, an user password and an owner password.

The passwords are optional.

There are a few permissions: `'copy'`, `'print'`, `'modify'`, `'annot-forms'`, `'fill-forms'`, `'extract'`, `'assemble'`, `'print-highres'`.

```php
use PDF;

function generate_pdf() {
	$data = [
		'foo' => 'bar'
	];
	$pdf = PDF::Make();
	$pdf->SetProtection(['copy', 'print'], 'user_pass', 'owner_pass')
	$pdf->loadView('pdf.document', $data);

	return $pdf->Stream('document.pdf');
}
```

Find more information to `SetProtection()` here: https://mpdf.github.io/reference/mpdf-functions/setprotection.html

### Documentation

 Visit this link for more options and settings: https://mpdf.github.io/
