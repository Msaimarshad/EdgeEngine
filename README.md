# EdgeEngine

[![CI Status](https://img.shields.io/travis/48580461/EdgeEngine.svg?style=flat)](https://travis-ci.org/48580461/EdgeEngine.git)
[![Version](https://img.shields.io/cocoapods/v/EdgeEngine.svg?style=flat)](https://github.com/Msaimarshad/EdgeEngine.git)
[![License](https://img.shields.io/cocoapods/l/EdgeEngine.svg?style=flat)](https://github.com/Msaimarshad/EdgeEngine.git)
[![Platform](https://img.shields.io/cocoapods/p/EdgeEngine.svg?style=flat)](https://github.com/Msaimarshad/EdgeEngine.git)

Visit our [landing page](https://www.edgestore.ai) to learn more about our product or directly visit our [store](https://store.edgestore.ai/) to browse various models. We support following domains in the current sdk version:
- Image Processing
- Text Processing
- Audio Processing
If your use case is not covered then please [contact us](https://www.edgestore.ai/about) or open an issue and we will prioritise it over next update.


## Requirements
If you need us to relax any requirement please open an issue or contact us. Current requirements:
- iOS version ≥ 12.0
- Tensorflow version ≥ 2.5.0 (if already installed)
## Installation

EdgeEngine is available through [CocoaPods](https://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod 'EdgeEngine'
```

## Quick Start

Visit the [Edge Store](https://store.edgestore.ai) and find a model that you like. In this tutorial we will be running the
amazing [Centernet Keypoints](https://store.edgestore.ai/home/model?model=centernet-keypoints) model. This model detects person's bounding box and keypoints.  Download the model and add the downloaded model.edgem file in your assets folder. edgem is our file extension for sdk compatible models. 

### Running the Model

Following is the code to run the model:


```swift

let modelPath:String = Bundle.main.path(forResource: "model", ofType: "edgem")!
let model =  try EdgeModelWrapper(edgemPath: modelPath)

let myUIImage = UIImage(named: //your input image)
let pixelBufferRef = pixelBuffer(from: myUIImage)

//Centernet keypoints only accepts single image as a pixelbuffer
let inputData:[Any] = [uiImage]
let results = edgeModel!.run(pixelBufferRef)
```
```Objective-C

NSString *modelPath = [[NSBundle mainBundle] pathForResource:@"model" ofType:@"edgem"];
EdgeModelWrapper* edgeModel = [[EdgeModelWrapper alloc]initWithedgemPath:modelPath];

// Convert image to pixelbuffer
UIImage *myUIImage = [UIImage imageNamed://your input image];
CGImageRef myImage = [myUIImage CGImage];
CVPixelBufferRef pixelBufferRef = [self pixelBufferFromCGImage:myImage];
//Centernet keypoints only accepts single image as a pixelbuffer
Results *results = [edgeModel run:pixelBufferRef];
```

And thats it! You have successfully run a complex AI model just in a few lines. Now it is time to
visualize the results.


### Visualizing the Results

To quickly visualize the results:

```swift
Results *results = [edgeModel run:pixelBufferRef];
let resultsUIImage = results.drawOnUIImage(image:uiImage)
```
```Objective-C
Results *results = [edgeModel run:pixelBufferRef];
[results drawResults:pixelBufferRef];
```

You can now add the resultsUIImage to your layout to quickly visualize the results. This will draw
location of person, confidence of detection, and keypoints on the resultsUIImage.


### Interpreting and Using Results

The output *results* variable has type Recognitions that is a type of List. All the useful
information the model provides is compacted into this variable. Not matter your use case the output of model will always be

For example if our model detects two persons in an image then results List will contain two
elements. Each element is of type Recognition and represents detection of each person


```swift
for person in results {
    // how sure we are that this is actually a person. Value between 0 and 1, represents probability of detection
    let confidence: CGFloat = person.confidence!
    // the pixel location of person's bounding box in the image
    let location: CGRect = person.location!
    // Keypoints detected on the person
    let keypoints: Keypoints = person.keypoints!
}
```
```Objective-C
for person in results {
    // how sure we are that this is actually a person. Value between 0 and 1, represents probability of detection
    CGFloat confidence = person->confidence;
    // the pixel location of person's bounding box in the image
    CGRect location = person->location/
    // Keypoints detected on the person
    Keypoints keypoints = person->keypoints;
}
```
## Tutorials

Here are a few tutorials to further explore the Edge Engine:
- [Object Detection](https://github.com/physxP/EdgeDocs/blob/main/use_cases/object-detection.md)
- [Human Pose Estimation](https://github.com/physxP/EdgeDocs/blob/main/use_cases/pose-detection.md)
- [Person and Object Segmentation](https://github.com/physxP/EdgeDocs/blob/main/use_cases/selfie-segmentation.md)


## Options
This section briefly discusses all the options that you can configure for the model. By default best options for common devices are already configured. Modify these if you have crashes or need to finetune performance.

### TensorFlow Lite Options

#### Use Accelerators

When you initialise model you can specify to use following accelerators:
- CoreML - Default On
- Metal - Default On
- XNNPack (Experimental) - Default Off
Turining on these options usually increase performance.

#### Num Threads

You can also specify the num of threads you want to use for the model. If you leave this option to default value of nil then we choose the best setting according to the device cpu.



## Author

[EdgeStore](https://edgestore.ai)


## License

EdgeEngine is available under the Edge Store EULA. See the LICENSE file for more info.

### License Summary

You can use this SDK in your apps for commercial purposes but modification / reverse engineering of SDK is not allowed.
