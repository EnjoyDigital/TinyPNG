# TinyPng for .Net

| Platform | Status|
|---------|-------|
|Windows  | [![Build status](https://img.shields.io/appveyor/ci/soda-digital/tinypng.svg)](https://ci.appveyor.com/project/Soda-Digital/tinypng) |
|Linux/OSX| [![Build Status](https://img.shields.io/travis/ctolkien/TinyPNG.svg)](https://travis-ci.org/ctolkien/TinyPNG) |


![Version](https://img.shields.io/nuget/v/tinypng.svg)
[![license](https://img.shields.io/github/license/ctolkien/TinyPNG.svg?maxAge=2592000)]()


This is a .NET wrapper around the [TinyPng.com](http://tinypng.com) image compression service. This is not an official TinyPNG.com product.

* Supports .Net Core and full .Net Framework
* Non-blocking async turtles all the way down
* `Byte[]`, `Stream` and `File` API's available

## Installation

Install via Nuget

```
    Install-Package TinyPNG
```

## Quickstart
```csharp
using (var png = new TinyPngClient("yourSecretApiKey")) 
{
    await (await png.Compress("cat.jpg")).SaveImageToDisk("compressedCat.jpg");
}
```

## Compressing Images

```csharp
//create an instance of the TinyPngClient
using (var png = new TinyPngClient("yourSecretApiKey")) 
{
    //compress an image
    var compressResult = await png.Compress("pathToFile or byte array or stream");

    //get the image data as a byte array
    var bytes = await compressResult.GetImageByteData();

    //get a stream instead
    var stream  = await compressResult.GetImageStreamData()

    //or just save to disk
    await compressResult.SaveImageToDisk("pathToSaveImage");

}
```

Further details about the result of the compression are also available on the `Input` and `Output` properties. Some examples:
```csharp

    //old size
    compressResult.Input.Size;
    
    //new size
    compressResult.Output.Size;

    //URL of the compressed Image
    compressResult.Output.Url;

```

## Resizing Images

```csharp
using (var png = new TinyPngClient("yourSecretApiKey")) 
{
    var compressResult = await png.Compress("pathToFile or byte array or stream");
    
    var resizedImage = await png.Resize(compressResult, width, height, ResizeType);

    await resizedImage.SaveImageToDisk("pathToSaveImage");
}

```

### Resize Operations

There are certain combinations when specifying resize options which aren't compatible with
TinyPNG. We also include strongly typed resize operations, 
depending on the type of resize you want to do. 

```csharp
using (var png = new TinyPngClient("yourSecretApiKey")) 
{
    var compressResult = await png.Compress("pathToFile or byte array or stream");
    
    await png.Resize(compressResult, new ScaleWidthResizeOperation(width));
    await png.Resize(compressResult, new ScaleHeightResizeOperation(width));
    await png.Resize(compressResult, new FitResizeOperation(width, height));
    await png.Resize(compressResult, new CoverResizeOperation(width, height));
}

```

The same `Byte[]`, `Stream` and `File` path API's are available from the result of the `Resize()` method.

### Compression Count

You can get a read on the number of compression operations you've performed by inspecting the `CompressionCount` property
on the result of any operation you've performed. This is useful for keeping tabs on your API usage.

```csharp
var compressedCat = await png.Compress("cat.jpg");
compressedCat.CompressionCount;
```
