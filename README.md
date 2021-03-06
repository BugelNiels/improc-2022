# Simple Image Processing Framework for C

A simple image processing framework for C. Supports grayscale `.pgm` images and binary `.pbm` images.
The aim of this framework is to make it easy to save, load, view and manipulate images. The entire framework is located in the files `improc.h`, `improc.c` and `greyimviewer.c`.

## Before you start

To familiarize yourself with the framework, take a look at `improc.h`. This file contains the signatures of all the functions in the framework. You do not need to look at, or modify `improc.c` and `greyimviewer.c`. You will find a simple threshold demo in `main.c`, which demonstrates a basic usage of the framework.

> Important: When using the framework, do not access any of the struct values directly. Only use the provided getter/setter functions.

## Image Viewer

The framework comes with a built-in image viewer. This image viewer has the following functionality:

- `A` reset the aspect ratio
- `C` contrast stretch*
- `H` histogram equalization*
- `I` invert image
- `S` print statistics
- `T` threshold*
- `F` random image (false coloring)
- `O` show origin in red*
- `G` change back to default LUT*
- `R` reset viewer
- `Q` quit the viewer
- `scroll` scale image up/down
- `ctrl+scroll` increase/decrease threshold*

> \* only for the grayscale image viewer.

## Pre-requisites

To run, it needs the following:

- OpenGL
	```sh
	sudo apt-get install mesa-utils
	```
- GLUT
	```sh
	sudo apt-get install freeglut3-dev
	```

# Running

To compile and run the program, do the following:

```sh
make
./improc <file name>
```

# Overview

Below is a brief overview of all function signatures available in the framework. The documentation of each of these functions can be found in `improc.h`.

* [ImageDomain](#image-domains)
* [IntImage](#int-images)
* [RgbImage](#rgb-images)
* [Histogram](#histograms)

___

## Image Domains

```C
int getMinX(ImageDomain domain);
int getMaxX(ImageDomain domain);
int getMinY(ImageDomain domain);
int getMaxY(ImageDomain domain);
int getWidth(ImageDomain domain);
int getHeight(ImageDomain domain);
void getWidthHeight(ImageDomain domain, int *width, int *height);
void getImageDomainValues(ImageDomain domain, int *minX, int *maxX, int *minY, int *maxY);
```

___

## Int Images

**Allocation**

```C
IntImage allocateIntImageGrid(int minX, int maxX, int minY, int maxY, int minValue, int maxValue);
IntImage allocateIntImageGridDomain(ImageDomain domain, int minValue, int maxValue);
IntImage allocateIntImage(int width, int height, int minValue, int maxValue);
IntImage allocateDefaultIntImage(int width, int height);
IntImage allocateFromIntImage(IntImage image);
IntImage copyIntImage(IntImage image);
void freeIntImage(IntImage image);
```

**Getters + Setters**

```C
ImageDomain getIntImageDomain(IntImage image);
void getMinMax(IntImage image, int *minimalValue, int *maximalValue);
void getDynamicRange(IntImage image, int *minRange, int *maxRange);
int getIntPixel(IntImage image, int x, int y);
int getIntPixelI(IntImage image, int x, int y);

void setIntPixel(IntImage *image, int x, int y, int greyValue);
void setIntPixelI(IntImage *image, int x, int y, int greyValue);
void setAllIntPixels(IntImage *image, int greyValue);
void setDynamicRange(IntImage *image, int newMinRange, int newMaxRange);
```

**Printing + Viewing**

```C
void printIntBuffer(IntImage image);
void printIntImageLatexTable(IntImage image);
void printIntLatexTableToFile(FILE *out, IntImage image);
void displayIntImage(IntImage image, const char *windowTitle);
```

**Saving + Loading**

```C
IntImage loadIntImage(const char *path);
void saveIntImage(IntImage image, const char *path);
void saveIntImagePGMRaw(IntImage image, const char *path);
void saveIntImagePGMAscii(IntImage image, const char *path);
void saveIntImagePBMRaw(IntImage image, const char *path);
void saveIntImagePBMAscii(IntImage image, const char *path);
```

**General Operations**

```C
IntImage distanceTransform(IntImage image, int metric, int foreground);

IntImage maxIntImage(IntImage imageA, IntImage imageB);
IntImage minIntImage(IntImage imageA, IntImage imageB);
IntImage addIntImage(IntImage imageA, IntImage imageB);
IntImage subtractIntImage(IntImage imageA, IntImage imageB);
IntImage multiplyIntImage(IntImage imageA, IntImage imageB);
IntImage applyLutIntImage(IntImage image, int *LUT, int LUTSize);
```

**Transformations**

```C
IntImage padIntImage(IntImage image, int top, int right, int bottom, int left, int padValue);
void translateIntImage(IntImage *image, int x, int y);
void flipIntImageHorizontal(IntImage *image);
void flipIntImageVertical(IntImage *image);
```

___

## RGB Images

**Allocation**

```C
RgbImage allocateRgbImageGrid(int minX, int maxX, int minY, int maxY, int minValue, int maxValue);
RgbImage allocateRgbImageGridDomain(ImageDomain domain, int minValue, int maxValue);
RgbImage allocateRgbImage(int width, int height, int minValue, int maxValue);
RgbImage allocateDefaultRgbImage(int width, int height);
RgbImage allocateFromRgbImage(RgbImage image);
RgbImage copyRgbImage(RgbImage image);
void freeRgbImage(RgbImage image);
```

**Getters + Setters**

```C
ImageDomain getRgbImageDomain(RgbImage image);
void getRgbDynamicRange(RgbImage image, int *minRange, int *maxRange);
void getRgbPixel(RgbImage image, int x, int y, int *r, int *g, int *b);
void getRgbPixelI(RgbImage image, int x, int y, int *r, int *g, int *b);

void setRgbPixel(RgbImage *image, int x, int y, int r, int g, int b);
void setRgbPixelI(RgbImage *image, int x, int y, int r, int g, int b);
void setAllRgbPixels(RgbImage *image, int r, int g, int b);
```

**Printing + Viewing**

```C
void printRgbBuffer(RgbImage image);
void printRgbImageLatexTable(RgbImage image);
void printRgbLatexTableToFile(FILE *out, RgbImage image);
void displayRgbImage(RgbImage image, const char *windowTitle);
```

**Saving + Loading**

```C
RgbImage loadRgbImage(const char *path);
void saveRgbImage(RgbImage image, const char *path);
void saveRgbImagePPMRaw(RgbImage image, const char *path);
void saveRgbImagePPMAscii(RgbImage image, const char *path);
```

**General Operations**

```C
RgbImage maxRgbImage(RgbImage imageA, RgbImage imageB);
RgbImage minRgbImage(RgbImage imageA, RgbImage imageB);
RgbImage addRgbImage(RgbImage imageA, RgbImage imageB);
RgbImage subtractRgbImage(RgbImage imageA, RgbImage imageB);
RgbImage multiplyRgbImage(RgbImage imageA, RgbImage imageB);
RgbImage applyLutRgbImage(RgbImage image, int **LUT, int LUTsize);
```

**Transformations**

```C
RgbImage padRgbImage(RgbImage image, int top, int right, int bottom, int left, int r, int g, int b);
void translateRgbImage(RgbImage *image, int x, int y);
void flipRgbImageHorizontal(RgbImage *image);
void flipRgbImageVertical(RgbImage *image);
```

___

## Complex Images

**Allocation**

```C
ComplexImage allocateComplexImageGrid(int minX, int maxX, int minY, int maxY);
ComplexImage allocateComplexImageGridDomain(ImageDomain domain);
ComplexImage allocateComplexImage(int width, int height);
ComplexImage allocateFromComplexImage(ComplexImage image);
ComplexImage copyComplexImage(ComplexImage image);
void freeComplexImage(ComplexImage image);
```

**Getters + Setters**

```C
ImageDomain getComplexImageDomain(ComplexImage image);
void getComplexMinMax(ComplexImage image, double *minimalValue, double *maximalValue);
double complex getComplexPixel(ComplexImage image, int x, int y);
double complex getComplexPixelI(ComplexImage image, int x, int y);

void setComplexPixel(ComplexImage *image, int x, int y, double complex complexValue);
void setComplexPixelI(ComplexImage *image, int x, int y, double complex complexValue);
void setAllComplexPixels(ComplexImage *image, double complex complexValue);
```

**Printing + Viewing**

```C
void printComplexBuffer(ComplexImage image);
void printComplexImageLatexTable(ComplexImage image);
void printComplexLatexTableToFile(FILE *out, ComplexImage image);
void displayComplexImage(ComplexImage image, const char *windowTitle);
```

**Saving + Loading**

```C
void saveComplexImage(ComplexImage image, const char *path);
void saveComplexImagePGMRaw(ComplexImage image, const char *path);
void saveComplexImagePGMAscii(ComplexImage image, const char *path);
```

**General Operations**

```C
ComplexImage fft2D(IntImage image);
IntImage ifft2D(ComplexImage image);
ComplexImage fft2DDouble(DoubleImage image);
DoubleImage ifft2DDouble(ComplexImage image);
void fft2Dshift(ComplexImage *image);
void ifft2Dshift(ComplexImage *image);
ComplexImage multiplyComplexImage(ComplexImage imageA, ComplexImage imageB);
```
___

## Double Images

**Allocation**

```C
DoubleImage allocateDoubleImageGrid(int minX, int maxX, int minY, int maxY, double minValue, double maxValue);
DoubleImage allocateDoubleImageGridDomain(ImageDomain domain, double minValue, double maxValue);
DoubleImage allocateDoubleImage(int width, int height, double minValue, double maxValue);
DoubleImage allocateFromDoubleImage(DoubleImage image);
DoubleImage allocateDefaultDoubleImage(int width, int height);
DoubleImage copyDoubleImage(DoubleImage image);
void freeDoubleImage(DoubleImage image);
```

**Getters + Setters**

```C
ImageDomain getDoubleImageDomain(DoubleImage image);
void getDoubleDynamicRange(DoubleImage image, double *minRange, double *maxRange);
void getDoubleMinMax(DoubleImage image, double *minimalValue, double *maximalValue);
double getDoublePixel(DoubleImage image, int x, int y);
double getDoublePixelI(DoubleImage image, int x, int y);

void setDoublePixel(DoubleImage *image, int x, int y, double val);
void setDoublePixelI(DoubleImage *image, int x, int y, double val);
void setAllDoublePixels(DoubleImage *image, double val);
```

**Printing + Viewing**

```C
void printDoubleBuffer(DoubleImage image);
void printDoubleImageLatexTable(DoubleImage image);
void printDoubleLatexTableToFile(FILE *out, DoubleImage image);
// for viewing, you can convert it to an intImage first
```

**General Operations**

```C
DoubleImage int2DoubleImg(IntImage image);
IntImage double2IntImg(DoubleImage image);
```
___

## Histograms

**Creation**

```C
Histogram createEmptyHistogram(int minRange, int maxRange);
Histogram createHistogram(IntImage image);
void createRgbHistograms(RgbImage image, Histogram *redHist, Histogram *greenHist, Histogram *blueHist);
void freeHistogram(Histogram histogram);
```

**Operations**

```C
void getHistogramRange(Histogram histogram, int *minRange, int *maxRange);
int getHistogramFrequency(Histogram histogram, int pixelVal);

void setHistogramFrequency(Histogram *histogram, int pixelVal, int freq);
void incrementHistogramFrequency(Histogram *histogram, int pixelVal);

void printHistogram(Histogram histogram);
```

___