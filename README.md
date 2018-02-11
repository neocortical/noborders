# NoBorders
### Remove borders from screencapped images.
NoBorders is a Go library and command line tool designed to remove borders and status bars from screencapped images. Example: 

### Before:
<img width="250" src="doc/gopher_before.png" />

### Command:
```
$ noborders gopher_before.png gopher_after.png
```

### After:
<img width="182" src="doc/gopher_after.png" />

## Installation
```
go get -u github.com/neocortical/noborders
cd $GOPATH/src/github.com/neocortical/noborders/noborders
go install .
```

## Command Line Usage
```
$ noborders [options] <input_file> <output_file>
```

## Library Usage
```go
import (
    "image"

    "gihub.com/neocortical/noborders"
)

var img image.Image 

// load the image you want to crop...

// optional, can be nil
opts := noborders.Opts().
    SetEntropy(0.05).
    SetVariance(100000).
    SetMultiPass(true)
        
img, err := noborders.RemoveBorders(img, opts)
// handle errors, save output image, etc.
```

## Notes
Disclamer: I am not an image processing person. There is probably a lot of established science that would make this library better. If you know of any, please create an issue with the details. I would love to make things faster, more robust, and more selective when it comes to retaining low-entropy backgrounds like sky that are present in the target image.

I originally wrote this with a very naive algorithm that looked at the first pixel in a row or column and elimate the row/column if all of the other pixels are similar. That worked pretty well, but it's easy to create pathological failure cases and it was also very bad at removing mobile phone status bars, browser location bars, etc. So I moved on to use row/column entropy, which is super-good at removing status bars, but often fails to get rid of areas right around the target image due to aliasing and compression artifacts. As a solution, I added a variance calculation, which when coupled with entropy seems to work out pretty well.

This library functions pretty badly when the target image is lineart. I would love to hear solutions for this. 
