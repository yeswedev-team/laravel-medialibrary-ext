# Associate files with Eloquent models

[![Source Code](https://img.shields.io/badge/source-okipa/laravel--medialibrary--ext-blue.svg)](https://github.com/Okipa/laravel-medialibrary-ext)
[![Latest Version](https://img.shields.io/packagist/v/okipa/laravel-medialibrary-ext.svg?style=flat-square)](https://packagist.org/packages/okipa/laravel-medialibrary-ext)
[![Total Downloads](https://img.shields.io/packagist/dt/okipa/laravel-medialibrary-ext.svg?style=flat-square)](https://packagist.org/packages/okipa/laravel-medialibrary-ext)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Build Status](https://img.shields.io/travis/Okipa/laravel-medialibrary-ext/master.svg?style=flat-square)](https://travis-ci.org/Okipa/laravel-medialibrary-ext)
[![Quality Score](https://img.shields.io/scrutinizer/g/Okipa/laravel-medialibrary-ext.svg?style=flat-square)](https://scrutinizer-ci.com/g/Okipa/laravel-medialibrary-ext)

This package provide extra features for the v7+ of [spatie/laravel-medialibrary](https://github.com/spatie/laravel-medialibrary) package.  

## Documentation

Find the complete package documentation here : https://docs.spatie.be/laravel-medialibrary/v7.

## Installation

This extension package is a fork from the original [spatie/laravel-medialibrary](https://github.com/spatie/laravel-medialibrary) one.  
As so, you should uninstall the original package if you installed it to avoid conflicts :
```bash
composer remove spatie/laravel-medialibrary
```

Then, install the extension package via composer :
```bash
composer require okipa/laravel-medialibrary-ext
```

Follow the original package installation instructions :
- https://github.com/spatie/laravel-medialibrary#installation
- https://docs.spatie.be/laravel-medialibrary/v7/installation-setup

Finally, you can publish the extension translation files if needed with :
```bash
php artisan vendor:publish --provider="Okipa\MediaLibraryExtension\MediaLibraryExtensionServiceProvider" --tag="translations"
```

## Extra features

- [Constraints](#constraints)
  - [Collection mime types constraint setup](#collection-mime-types-constraint-setup)
  - [Collection validation constraints rules generation](#collection-validation-constraints-rules-generation)
  - [Collection validation constraints legend generation](#collection-validation-constraints-legend-generation)
- [Global conversions queued status](#global-conversions-queued-status)
- [Useful extra public methods](#useful-additional-public-methods)

### Constraints

#### Collection mime types constraint setup
Addition of the `acceptsMimeTypes(array $mimeTypes): MediaCollection` which can be used with a media collection.  
Once declared, the mime types constraints will be used to trigger the `FileUnacceptableForCollection` exception if not respected, and will also be used to generate validation constraints and legends (see bellow).
```php
// example
public function registerMediaCollections()
{
    $this->addMediaCollection('images')->acceptsFile(function (File $file) {
        return $file->size <= 30000;
    })->acceptsMimeTypes(['image/jpeg', 'image/png']);
}
```

#### Collection validation constraints rules generation
Addition of the `validationConstraints(string $collectionName): string` method, which can be used with a model using the `HasMediaTrait`.  
```php
// in your user storing form request for example
public function rules()
{
    return [
        'avatar' => (new User)->validationConstraints('avatar'),
        // your other validation rules
    ];
}
```
Rendering example : `dimensions:min_width=60,min_height=20|mimetypes:image/jpeg,image/png`.

#### Collection validation constraints legend generation
Addition of the `constraintsLegend(string $collectionName): string` method, which can be used with a model using the `HasMediaTrait`.
```html
// in your HTML form
<label for="avatar">Choose a profile picture :</label>
<input type=" id="avatar" name="avatar" value="{{ $avatarFileName }}">
<small>{{ (new User)->constraintsLegend('avatar') }}</small>
```
Rendering example : `Min. width : 150 px / Min. height : 70 px. Accepted MIME Type(s) : image/jpeg, image/png.`

### Global conversions queued status
To manage the global conversions queued status, set the `config('medialibrary-extension.queuedConversions')` value. This will be the default queued status for all your defined conversions.  
You still will be able to manually define a [specific queued status for a conversion](https://docs.spatie.be/laravel-medialibrary/v7/converting-images/defining-conversions/#queuing-conversions). 

## Useful additional public methods
The following methods can also be used separately with a model using the `HasMediaTrait` :
- `dimensionValidationConstraints(string $collectionName): string` : Get a collection dimension validation constraints string from its name.
- `mimeTypesValidationConstraints(string $collectionName): string` : Get a collection mime types constraints validation string from its name.
- `dimensionsLegend($collectionName): string` : Get a collection dimensions constraints legend string from its name.
- `mimeTypesLegend($collectionName): string` : Get a collection mime types constraints legend string from its name.
- `collectionMaxSizes(string $collectionName): array` : Get registered collection max width and max height from its name.
- `getCollection(string $collectionName): ?MediaCollection` : Get a media collection object from its name.
- `getConversions(string $collectionName): array` : Get declared conversions from a media collection name.
- `shouldHaveDimensionConstraints(MediaCollection $collection): bool` : Check if the given media collection should have dimension constraints, according to its declared accepted mime types.

## Testing

``` bash
composer test
```

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information about what has changed recently.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Credits

- [Arthur LORENT](https://github.com/okipa)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
