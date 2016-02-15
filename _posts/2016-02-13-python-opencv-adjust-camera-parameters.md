---
layout: post
title: Python OpenCV Adjusting Camera Parameters
---
![Stereo Camera](https://cloud.githubusercontent.com/assets/3308051/13057389/e3730cde-d3e9-11e5-81c4-157b7c8f9a63.png "Stereo Camera")

I was working on a project for object localization using stereo camera. I was facing trouble in processing the active markers accurately. I was trying lot of things and didn't had much success, just by playing around with camera parameters I was able to get the job done. So here is the python code to set the parameters:

{% highlight python %}
#Camera Parameteres 
camera_port = 0
ramp_frames = 30

#Getting the camera references
cam_l = cv2.VideoCapture(1)
cam_r = cv2.VideoCapture(2)

#Camera Settings
gain=174
cam_l.set(cv2.cv.CV_CAP_PROP_GAIN,gain)
cam_r.set(cv2.cv.CV_CAP_PROP_GAIN,gain)


brightness=50
cam_l.set(cv2.cv.CV_CAP_PROP_BRIGHTNESS,brightness)
cam_r.set(cv2.cv.CV_CAP_PROP_BRIGHTNESS,brightness)

contrast=67
cam_l.set(cv2.cv.CV_CAP_PROP_CONTRAST,contrast)
cam_r.set(cv2.cv.CV_CAP_PROP_CONTRAST,contrast)

saturation=85
cam_l.set(cv2.cv.CV_CAP_PROP_SATURATION,saturation)
cam_r.set(cv2.cv.CV_CAP_PROP_SATURATION,saturation)

exposure=-9
cam_l.set(cv2.cv.CV_CAP_PROP_EXPOSURE,exposure)
cam_r.set(cv2.cv.CV_CAP_PROP_EXPOSURE,exposure)
{% endhighlight %}

Although the above code is for a stereo camera, if you are using just one camera you can omit one line for each. Also, here is a code that can help vary the parameters with the keys in order to just play around:



{% highlight python %}
import numpy as np
import cv2

properties=["CV_CAP_PROP_FRAME_WIDTH",# Width of the frames in the video stream.
            "CV_CAP_PROP_FRAME_HEIGHT",# Height of the frames in the video stream.
            "CV_CAP_PROP_BRIGHTNESS",# Brightness of the image (only for cameras).
            "CV_CAP_PROP_CONTRAST",# Contrast of the image (only for cameras).
            "CV_CAP_PROP_SATURATION",# Saturation of the image (only for cameras).
            "CV_CAP_PROP_GAIN",# Gain of the image (only for cameras).
            "CV_CAP_PROP_EXPOSURE"]






cap = cv2.VideoCapture(1)
for prop in properties:
    val=cap.get(eval("cv2.cv."+prop))
    print prop+": "+str(val)

gain=0
cap.set(cv2.cv.CV_CAP_PROP_GAIN,gain)

brightness=60
cap.set(cv2.cv.CV_CAP_PROP_BRIGHTNESS,brightness)

contrast=20
cap.set(cv2.cv.CV_CAP_PROP_CONTRAST,contrast)

saturation=20
cap.set(cv2.cv.CV_CAP_PROP_SATURATION,saturation)

exposure=-1
cap.set(cv2.cv.CV_CAP_PROP_EXPOSURE,exposure)

while(True):
    # Capture frame-by-frame
    ret, frame = cap.read()

    # Our operations on the frame come here
    #rgb = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
    rgb=frame
    
    print "\n\n"
    for prop in properties:
        val=cap.get(eval("cv2.cv."+prop))
        print prop+": "+str(val)
    # Display the resulting frame
    cv2.imshow('frame',rgb)
    key=cv2.waitKey(1)
    if key == ord('x'):
        break
    elif key == ord('w'):
        brightness+=1
        cap.set(cv2.cv.CV_CAP_PROP_BRIGHTNESS,brightness)
    elif key == ord('s'):
        brightness-=1
        cap.set(cv2.cv.CV_CAP_PROP_BRIGHTNESS,brightness)
    elif key == ord('d'):
        contrast+=1
        cap.set(cv2.cv.CV_CAP_PROP_CONTRAST,contrast)
    elif key == ord('a'):
        contrast-=1
        cap.set(cv2.cv.CV_CAP_PROP_CONTRAST,contrast)
    elif key == ord('e'):
        saturation+=1
        cap.set(cv2.cv.CV_CAP_PROP_SATURATION,saturation)
    elif key == ord('q'):
        saturation-=1
        cap.set(cv2.cv.CV_CAP_PROP_SATURATION,saturation)
    elif key == ord('z'):
        exposure+=1
        cap.set(cv2.cv.CV_CAP_PROP_EXPOSURE,exposure)
    elif key == ord('c'):
        exposure-=1
        cap.set(cv2.cv.CV_CAP_PROP_EXPOSURE,exposure)
            


    
# When everything done, release the capture
cap.release()
#cv2.destroyAllWindows()
{% endhighlight %}

Instructions for varying:<br />
<ul style="text-align: left;">
<li>x-break&nbsp;</li>
<li>w - increase brightness&nbsp;</li>
<li>s - decrease brightness&nbsp;</li>
<li>d - increase contrast&nbsp;</li>
<li>a - decrease contrast&nbsp;</li>
<li>e - increase saturation&nbsp;</li>
<li>q - decrease saturation&nbsp;</li>
<li>z - increase exposure&nbsp;</li>
<li>c - decrease exposure&nbsp; </li>
</ul>
Lastly, I would like to acknowledge my partner in the project <a href="https://www.facebook.com/junkfunkydude">Ben Province</a>.
