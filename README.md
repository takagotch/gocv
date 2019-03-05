### gocv
---
https://github.com/hybridgroup/gocv

```
```

```go
package main

import (
  "net/http"
  _ "net/http/pprof"
  "time"
  
  "gocv.io/x/gocv"
)

func leak() {
  gocv.NewMat()
}

func main() {
  go func() {
    ticker := time.NewTicker(time.Second)
    for {
      <-ticker.C
      leak()
    }
  }()
  
  http.ListenAndServe("localhost:6060", nil)
}

gocv.MatProfile.Count()

var b bytes.Buffer
gocv.MatProfile.WriteTo(&b, 1)
fmt.Print(b.String())


package main

import (
  "bytes"
  "fmt"
  
  "gocv.io/x/gocv"
)

func leak() {
  gocv.NewMat()
}

func main() {
  fmt.Printf("initial MatProfile count: %v\n", gocv.MatProfile.Count())
  leak()
  
  fmt.Printf("final MatProfile count: %v\n", gocv.MatProfile.Count())
  var b bytes.Buffer
  gocv.MatProfile.WriteTo(&b, 1)
  fmt.Print(b.String())
}


package main

import (
  "gocv.io/x/gocv"
)

func main() {
 webcam, _ := gocv.OpenVideoCapture(0)
 window := gocv.NewWindow("Hello")
 img := gocv.NewMat()
 
 for {
   webcam.Read(&img)
   window.IMShow(img)
   window.WaitKey(1)
 }
}


package main

import (
  "fmt"
  "image/color"
  
  "gocv.io/x/gocv"
)

func main() {
  deviceID := 0
  
    webcam, err := gocv.OpenVideoCapture(deviceID)
    if err != nil {
      fmt.Println(err)
      return
    }
    defer webcam.Close()
    
    window := gocv.NewWindow("Face Detect")
    defer window.Close()
    
    img := gocv.NewMat()
    defer img.Close()
    
    blud := color.RGBA(0, 0, 255, 0)
    
    classifier := gocv.NewCascadeClassfier()
    defer classifier.Close()
    
    if !classifier.Load("data/haarcascade_frontalface_default.xml") {
      fmt.Println("Error reading cascade file: data/haarcascade_frontalface_default.xml")
      return
    }
    
    fmt.Printf("start reading camera device: %v\n", deviceID)
    for {
      if ok := webcam.Read(&img); !ok {
        fmt.Printf("cannot read device %v\n", deviceID)
        return
      }
      if img.Empty() {
        continue
      }
      
      rects := classifier.DetectMultiScale(img)
      fmt.Printf("found %d faces\n", len(rects))
      
      for _, r := range rects {
        gocv.Rectangle(&img, r, blue, 3)
      }
      
      window.IMShow(img)
      window.WaitKey(1)
    }
}

```

```
go get -u -d gocv.io/x/gocv

cd $GOPATH/src/gocv.io/x/gocv
make install

gocv version: 0.17.0
opencv lib version: 4.0.0

cd $GOPATH/src/gocv.io/x/gocv
make deps
make download
make build
make sudo_install
cd $GOPATH/src/gocv.io/x/gocv
go run ./cmd/version/main.go
gocv version: 0.17.0
opencv lib version: 4.0.0
make clean
go install gocv.io/x/gocv
export CGO_CPPFLAGS="-I/usr/local/include"
export CGO_LDFLAGS="-L/usr/local/lib -lopencv_core -lopecv_face -lopencv_videoio -lopencv_imagproc -lo..."
go run -tags customenv ./cmd/version/main.go

go run -tags matprofile cmd/version/main.go

go install gocv.io/x/gocv

set CGO_CXXFLAGS="--std=c++11"
set CGO_CPPFLAGS=IC:\opencv/build\install\include
set CGO_LDFLAGS=-LC:\opencv\build\install\x64\mingw\lib -lopecv_core400 -lopencv_core400 -lopencv_face400 -lopencv_videoio400 -lopencv_imgproc400 -lopencv_highgui400 -lopencv_imgcides400 -lopencv_objectect400 -lopencv_features2d400 -lopencv_video -lopencv_dnn400 -lopencv_dnn400 -lopencv_xfeatures2d400 -lopencv_plot400 -lopencv_tracking400 -lopencv_img_hash400

chdir $GOPATH%\src\gocv.io\x\gocv
win_build_opencv.cmd

chdir %GOPATH%\src\gocv.io\x\gocv

go run cmd\version\main.go

gocv version: 0.17.0
opencv lib version: 4.0.0
```


