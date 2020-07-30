<!-----
NEW: Check the "Suppress top comment" option to remove this info from the output.

Conversion time: 2.723 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β29
* Sat Jul 25 2020 03:31:43 GMT-0700 (PDT)
* Source doc: Ceci n'est pas un Hayasaka
* Tables are currently converted to HTML tables.
* This document has images: check for >>>>>  gd2md-html alert:  inline image link in generated source and store images to your server. NOTE: Images in exported zip file from Google Docs may not appear in  the same order as they do in your doc. Please check the images!

----->


<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 0; ALERTS: 3.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p><a href="#gdcalert1">alert1</a>
<a href="#gdcalert2">alert2</a>
<a href="#gdcalert3">alert3</a>

<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>




<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.jpg). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image1.jpg "image_tooltip")


[Font](https://graphicdesign.stackexchange.com/questions/39653/what-is-an-approximate-font-for-the-painting-used-in-the-treachery-of-images), to some approximation

I perform transfer learning on the pre-trained Danbooru2019 Portraits Stylegan 2 model, to [best girl.](https://mywaifulist.moe/best)<sup>numbers don’t lie.</sup>


## Data Engineering

Download desired images off of Danbooru using [this tool](https://github.com/Bionus/imgbrd-grabber/releases/tag/v7.3.0) (it might be worth saving by %id%, in case metadata is desired later on).

Hayasaka has ~270 images on Danbooru, and even with higher accuracy, it would still be a pretty small dataset. I am turning to Pixiv as an adjunct solution. Pixiv is more saturated than Danbooru, and there is more variation in quality; top posts from Pixiv are reposted to Danbooru. But it’s possible to sort by number of bookmarks to find higher quality images, and I plan on increasing the dataset to >500 in this way. 

I use Pikax’s tool, filtering for images with 5100 bookmarks as a proxy for quality (It’s also possible to filter by views, but bookmarking is more selective, and thus a better metric). Used dupeGuru with strictness set to 50 in both pixiv and Danbooru folders. Then, the contents of the folders were combined, and dupeGuru was applied yet again. Finally, we spent 10 minutes deleting B&W or full page manga panels, wrong character, and no head images.

Given the sparseness of the current dataset, I am tempted to download images from the [100-500](https://www.pixiv.net/en/tags/%E6%97%A9%E5%9D%82%E6%84%9B/illustrations?order=popular_male_d&blt=100&bgt=499&s_mode=s_tag&type=illust) bookmarks range. The quality is variable, but filtering by popularity will help. The biggest issue with Pixiv is the amount of manual labour required to clean the data. The lack of tags, combined with the lower average quality, makes working with Pixiv not very pleasant. And I refuse to compromise the quality of generated _waifus_ by saturating the dataset with lower quality art. 

Instead, I manually go through the around ~300 images I already have, and perform some rotations and crops (for non-solo images). I take care not to delete any data which the cropper is working on (although I am very tempted to make an exception for one particular image (*cough* fireball666 *cough*). Background uniformity seems to be important for training GANs, and it’s imperative that such a small dataset have clean, denoised data. 

Coming into this project, my plan was to amass a large sample of diverse images. But training on a more selective subset of similar images produces very strong results (e.g. see[ n=20 images](https://www.gwern.net/Faces#ptilopsis) by Ganso!) Most of the transfer laerning examples on Gwern’s site are on the non-Portrait, earlier dataset, so the results are worse; but that alone doesnn’t explain the quality on Gsnso’s Ptilopsis. She is center-cropped in all of these images, and maintains a consistent hairstyle. The art style is pretty similar as well. However, this subset isn’t entirely devoid of diversity — different images have Ptilosis’s eyes at different angles, luminosity is non-constant, and there is _some_ variation around the top of her head. 

In contrast, my dataset has more rotations along both the azimuthal and polar planes; diverse art styles (at least 5-10 distinctive face types), and two distinct hairstyles. This makes me update more strongly in favour of getting rid of non-ponytail / non-scrunchie Hayasakas from the current data.

The cascading classifier based on Haar converts the image to B&W, equalizes the contrast over the image, and then looks for ROIs using _[haar features](https://en.wikipedia.org/wiki/Viola%E2%80%93Jones_object_detection_framework)_. The method was developed in 2001 by Viola & Jones, and it uses a few heuristics: (i) You can use integral images (i.e. small regions, vs all pixels) and (ii) you use black-white rectangular bounding box like matchers for different face regions.

For instance, one Haar block is based on the heuristic that eyes are higher in contrast than cheeks. 

Blushing images suffer considerably. As does winking, eye covered by hair, and overly small noises —  all of which are pretty common in fan art. Other usual suspects include skewed images, and the distribution of contrast and luminosity. I’ll expand on these in the data augmentation section.



<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image2.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image2.png "image_tooltip")


<p id="gdcalert3" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image3.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert4">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image3.png "image_tooltip")


Fig: histogram equalization in action. The image on the left is easier for the cascade classifier to work with, as contrast is spread out after equalization.

/home/tazik/Nextcloud/Code/lbpcascade_animeface/examples/hayasaka/0-3958927.png

Crop faces using nagadomi’s [face cropper](https://github.com/nagadomi/lbpcascade_animeface/). Crops are generally accurate, but there are many false negatives. This is not much of an issue when you’re working with the entire Danbooru dataset, but is an issue for smaller datasets when transfer learning (_especially_ if you choose anything but the most popular characters). [___](https://translate.google.com/translate?hl=&sl=auto&tl=en&u=https%3A%2F%2Fblog.csdn.net%2FDLW__%2Farticle%2Fdetails%2F104222546), reports the same issue, with 1659/2408 (69%) of his images being identified. 

_[minNeighbours ](https://stackoverflow.com/a/20805153/8773953)_determines the number of detections around a particular face

[scaleFactor](https://stackoverflow.com/a/55628240/8773953)_ _determines the resizing of the [scale pyramid ](https://en.wikipedia.org/wiki/Pyramid_(image_processing))at every iteration

I fiddled with the parameters in Nagadomi’s cropper.  I also changed the minimum size to (200,200), to avoid non-face and lower quality regions. It would be nice if I could set an upper limit on _y_ for constant x; rectangular mistakes would not be cropped.

More permissive scale pyramid leads to a rather high false positive / false negative ratio. Granted, feature engineering & image processing tricks (e.g. remove images beyond a certain height, limit the number of crops allowed in a single image, and perhaps even enforcing a certain confidence level with advanced usage of `detectMultiScale`; and if we’re up for even more overengineering, color segmentation based on face color (mostly constant) and a specific character’s hair and / or eye color … but we must draw the line somewhere, lest ADHD takes over completely) could be used to improve results, but even then, will the incremental increase in data size make that much of a difference? It would be more worthwhile to increase the dataset from external sources.

But then, for such a small dataset, one needs to manually QA the images anyway, and n  = 500 isn’t that different from n = 200 if you’re using a GUI image viewer; so the data is actually worth it. After deciding to do some manual labour, I wanted to test the effect of minNeighbours, which would reduce the number of edges in a region for it to be considered a ‘face’, given an already large scale pyramid (1.005) (reduce image by 0.5% at each step!). I found that minNeighbours = 3 and 5 were only marginally different. One produced around ~500 image, while the other flrited with 600, and yet, most of these were non-face regions. The only use case where it seemed to make a difference was in the few non-solo images, but those are but a few, so I did away with the data.

Now, with minNeighbours = 5, I try changing scale to 0.25%. My image count goes up by 150 (to 650), but the differential images are mostly false positives. It seems that min = 5, scale = 0.5% is pretty good, but I’m gonna try 2xing the scale factor to find the actual sweet spot. The last two observations were made by inspection. But using **dupeguru** is probably a better idea — since I already have a dataset from scale pyramid set to 5%, I can use it to automatically tell me which additional files are present in the 0.5%, saving a ton of otherwise manual labour. 

I change the bounding box rectangle to blue and increase the line width for easy visibility when viewed in bunches. Nomacs is used for multi-image viewing.

Since I’m working with a specific character, I can make amends for idiosyncrasies that may arise. 

Hayasaka by default sports a ponytail. Most images tend to have the ponytail in the right half of the image (i.e. left-facing Hayasaka). I thus increased the bounding box width, to capture the ponytail in a larger fraction of images. This shouldn’t affect the error rate too much, since the horizontal axis around the face tends to be background filler. I also increase the y_initial margin, noting that most images don’t have any artifacts above this region, and I’d prefer a full face  crop.

Full face cropping does mean I’ll be including partial hands in many images. But I expect that after a few what-do-you-call-them (runs? iterations?), I can use the method described [here](https://www.gwern.net/Faces#discriminator-ranking) — where after some level of training, I use the Discriminator (affectionately termed the _D_) to rank the (generated, I presmuse?) images it deems the least likely to be real. — and delete them, before moving on to later stages.


```
import cv2
import sys
import os.path

def detect(abs_filename, cascade_file = "../lbpcascade_animeface.xml"):#, mode="display"):
	if not os.path.isfile(cascade_file):
    	raise RuntimeError("%s: not found" % cascade_file)

	cascade = cv2.CascadeClassifier(cascade_file)
	image = cv2.imread(abs_filename, cv2.IMREAD_COLOR)
	height, width, channels = image.shape
	#image = image[0: int(h/2), 0: w]
	gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
	gray = cv2.equalizeHist(gray)
    
	faces = cascade.detectMultiScale(gray,
                                 	# detector options
                                 	scaleFactor = 1.05,
                                 	minNeighbors = 5,
                                 	minSize = (250, 250)
                                 	#,maxSize = (int(0.4*w), int(0.4*h))
                                 	#too inclusive ... i.e. it appears to be OR-ing the condition, not AND-ing.
                                 	)
	'''
	Don't overwrite images accidentally! non unique abs_filename for multi crops...
	'''
	tag = 0
	filename = os.path.basename(abs_filename)
	for (x, y, w, h) in faces:
    	#if h > height * 0.5:
    	#	continue

    	#cv2.rectangle(image, (int(x*0.85), int(y*0.1)), (x + int(w*1.5), y + h), (255, 0, 0), 50)
    	cropped = image[int(y*0.1): y + h, int(x*0.85): x + int(w*1.5)]
    	#cv2.imshow("AnimeFaceDetect", image)
    	#cv2.waitKey(0)
    	cv2.imwrite(str(filename[0:-4] + '_' + str(tag) + filename[-4:]), cropped)
    	tag += 1

	print(tag)

if len(sys.argv) != 2:
	sys.stderr.write("usage: detect.py <abs_filename>\n")
	sys.exit(-1)
detect(sys.argv[1])
```


 It seems that I can use the pre-trained model’s _D_ to rank images. Manual filtering should be much simpler now. I might also be able to better gauge what params determine the model’s confidence. 


## Upscaling, Conversion etc.

I use this CNN implementation of Waifu2x ([notebook](https://colab.research.google.com/drive/1Mz_oN6NNT5-siYG39HSE673OL2iuccGR#scrollTo=GWRFnbOolQ-X)), as the original is a pain to get up and running.  

convertPNGToJPG() { convert -quality 100 "$@" "$@".jpg && rm "$@"; }

export -f convertPNGToJPG

find jpg/ -type f -name "*.png" | parallel --progress convertPNGToJPG

find jpg/ -type f -name "*.png" | parallel --progress convertPNGToJPG

[tazik@xps 2x_20200717T013703Z-001]$ cat resize.sh

find jpg/ -type f | xargs --max-procs=16 -n 9000 \

	mogrify -resize 512x512\> -extent 512x512\> -gravity center -background black

find jpg/ -type f | xargs --max-procs=16 -n 9000 identify | \

	# remember the warning: images must be identical, square, and sRGB/grayscale:

	fgrep -v " JPEG 512x512 512x512+0+0 8-bit sRGB"| cut -d ' ' -f 1 | \

	xargs --max-procs=16 -n 10000 rm

Two notes: I have a small dataset, so I can afford to not compress my PNG’s into more lossy JPGs. Thus, I retain 100% quality. The last snippet — which is a check to delete potentially incompatible images, throws “rm: missing operand”, which I cn only assume means I don’t have any issues for it to remove.


## Training

Here, I will offer an ELI5-like explanation of GANs. If you would like a more rigorous treatment — vanilla GANs, see [here](https://medium.com/@jonathan_hui/gan-whats-generative-adversarial-networks-and-its-application-f39ed278ef09) and for an overview of StyleGAN, [here](https://medium.com/@jonathan_hui/gan-stylegan-stylegan2-479bdf256299)

Also, good overview at [https://www.gwern.net/Faces#background](https://www.gwern.net/Faces#background)

It’s not immediately obvious from Gwern’s guide what values to set to G_smoothing_kimg & D_repeats, so I do a text diff comparison of vanilla StyleGAN2 with [https://github.com/xunings/styleganime2](https://github.com/xunings/styleganime2). The only difference in the run_training file appears to be `resume_kimg`, which is set to `10000` for transfer learning. On the [training/training_loop.py front](https://github.com/xunings/styleganime2/blob/master/training/training_loop_kagura_waifu4x_20200227.py), he’s added some CLI support for minibatch configuration, I presume. Of course, these are not definitive, especially since I am unsure what Shao’s generated output is.

It might help to come up with a preliminary agenda of which parameters to test, and _how_ e.g.

Ideas



*   Increase data size
*   Make all backgrounds transparent
*   Spatial data augmentations (rotations, resizing)
*   Visual data augmentations (color variations, lighting etc. I don’t really know the terminology too well)
*   Remove non-ponytail Hayasakas
*   Also, Hayasakas with custom / unique accessories
*   (The default Hayasaka wears a ponytail with a blue scrunchie)
*   Our pretrained model may be the portraits one, but a lot of artifacting related to hands and backgrounds could be removed if we used a restrictive crop, a la Gwern’s TWDNEv1.
*   Since Gwern had a lot of rotations etc. in his transfer-learned Asuka and Holo, should I try transfer learning on _that_ model? Will have to think about how “progressive growing” and all of that works and understand it better before I can determine that

Of course, the above need not be performed only on the already filtered dataset; we might also look into other combinations.

Code:



*   [https://colab.research.google.com/drive/1USc6rqjBuw6osro_ccLexUG3saYYmOVL#scrollTo=fimqSTw_CSNX&uniqifier=2](https://colab.research.google.com/drive/1USc6rqjBuw6osro_ccLexUG3saYYmOVL#scrollTo=fimqSTw_CSNX&uniqifier=2)
*   


## Links



*   All pre-trained models can be found [here](https://www.gwern.net/Faces#anime-faces).
*   [Stylegan 2](https://github.com/ZKTKZ/stylegan2) [Notebook](https://colab.research.google.com/drive/1hqf2FDQu-etb2tRcd3zGf3-SMmDktsdS#scrollTo=4_s8h-ilzHQc)
*   [Waifu2X](https://github.com/nihui/waifu2x-ncnn-vulkan) [Notebook](https://colab.research.google.com/drive/1Mz_oN6NNT5-siYG39HSE673OL2iuccGR#scrollTo=zI9rQ-Em2XJH)
*   Gwern
    *   Transfer Learning [tips](https://www.gwern.net/Faces#transfer-learning)
    *   General [S1/S2](https://www.gwern.net/Faces#configuration) config
    *   Other, less important links are in _OneTab_
*   Non-Gwern Transfer Learning [Guide](https://translate.googleusercontent.com/translate_c?depth=1&pto=aue&rurl=translate.google.com&sl=auto&sp=nmt4&tl=en&u=https://blog.csdn.net/DLW__/article/details/104243506&usg=ALkJrhhoHTtIYatPPAcDceiOUVV9GUO-4w)
*   Data augmentation
    *   Gwern’s[ impl.](https://www.gwern.net/Faces#quality-checks-data-augmentation)
    *   Papers (see _Downloads_):
        *   _Image Augmentations for GAN Training _(2006.02595)
        *   _Training Generative Adversarial _(2006.06676)


## Tips



*   When choosing which character to transfer learn on, Danbooru tags can be a useful indicator of common markers in the dataset. This is not as obvious as choosing a character with large _n_, but nonetheless quite helpful in determining the “actual” usable data corpus
*   Additionally, using a Danbooru tagger like DeepDanbooru on Pixiv can vastly increase your dataset size. The fundamental issue with Pixiv is the lck of tags, so what we need is a DeepDanbooru to be run on Pixiv, allowing more customization of a larger dataset.


## Appendix

Cancel that [Google One subscription](https://play.google.com/store/account/subscriptions).

Gwern reports 90% accuracy with this OpenCV based [anime face cropper](https://github.com/nagadomi/lbpcascade_animeface/blob/master/lbpcascade_animeface.xml), but I’m getting at best 70%.... It probably has to do with the fact that Cynthia / Shirona has bangs covering half her face in many images.

_Mail Gwern with typos as an honorary way of thanking him._



*   “There should be no issues if all the images were thoroughly checked earlier, but should an images [image] crash it, they can be checked in more detail by identify.”
*   Interestingly, while Holo trained within [missing figure] GPU-hours
*   If one could, one could take an arbitrary image and encode it into the _z_ and by jittering _z_, generate many new version [versions] of it;
*   (Anything like fakes00000.png should not show up because [it] indicates beginning from scratch)
*   Thus, input files must be perfectly uniform, slowly converted to the .tfrecord formats [format] by the special dataset_tool.py
*   and I doubt other colorspacs [colorspaces] work at all
