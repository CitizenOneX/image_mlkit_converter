Converts Dart Image Library `Image`s to `InputImage`s suitable for processing with `google_ml_kit` on Android and iOS.

## Features

Converts to BGRA8888 format on iOS, and NV21 format on Android, as required by the respective ML Kit implementations on each platform.
Allows the user to specify an orientation for the image, which is passed through to ML Kit as metadata.

## Getting started

Import the [Dart Image Library](https://pub.dev/packages/image) and one of the [google_ml_kit](https://pub.dev/packages/google_ml_kit) plugins.

## Usage

```dart
import 'package:image/image.dart' as img;
import 'package:google_mlkit_image_labeling/google_mlkit_image_labeling.dart';

// set up an image labeler (but we could use any google_ml_kit package)
final ImageLabelerOptions options = ImageLabelerOptions(confidenceThreshold: 0.5);
final imageLabeler = ImageLabeler(options: options);

// make an Image from a local jpg
img.Image? image = await img.decodeJpgFile(path);
// this jpg is oriented 90 degrees clockwise, so let ml kit know
ImageInput mlImage = ImageMlkitConverter.imageToMlkitInputImage(image!, InputImageRotation.rotation90deg);
// run ml kit labeler
final List<ImageLabel> labels = await imageLabeler.processImage(inputImage);

for (ImageLabel label in labels) {
  final String text = label.label;
  final int index = label.index;
  final double confidence = label.confidence;
}
```
