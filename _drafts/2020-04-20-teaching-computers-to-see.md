---
layout: post
title:  "Teaching Computers to See Materials"
date:   2020-04-20 15:30:31 -0700
categories: research-summaries
mathjax: true
---

This post is a high-level overview of a research project from my PhD. If you want to get down into the nitty-gritty, then feel free to read the paper we're publishing on this research (DOI TO COME). You can also reach out to me with questions, or access our open-source code at github.com/ponl/m2py.

### Motivation and Background

Chemistry has changed drastically throughout history. As we gather more data and re-orient our perspectives, chemists and physicists have changed from semi-mystical, alchemic artists to rigorously and reproducibly hypothesis-driven researchers that draw on theory and, newly, simulations to guide our predictions.

Central to this progress is technology. The results of these discoveries now power and permeate just about every facet of our lives. Similarly, microscopes and computers have completely changed the world, both that of scientific exploration and day-to-day life. Mircoscopy has allowed unparalleled insight into compounds and material systems. We can count cells, look at fracturing mechanisms and stress dissipation, or observe molecular lattices and ordering. This scale of insight is enormouns, so, let's put it in perspective. A human hair is about 10 $\mu$m _wide_, or 10$^{-5}$ m. That's taking 1 hundred-thousandth of a meter stick. Now take that single, miniscule shred of wood and divide its width again into 10 million more pieces. One of those resulting pieces is now 1 picometer wide (10$^{-12}$ m), which is about how small humans can now see with techniques like scanning-tunneling electron microscopy (STEM). This lets researchers image atoms directly!

Here are just a few examples:

Quantum dot photo series
Graphene photo series
self-assembled structures

These techniques have completely revolutionized science alongside many other ground-breaking techniques all the other techniques that have been developed to probe chemicals, reactions, materials. Most recently, the advent of computer simulations has also begun to help develop theory and elucidate on experimental results. 

Scanning Probe Microscopy (SPM) is one imaging technique that I've used extensively in my research. It uses a physical, atomically sharp probe to map out the surface. These probes can tap the surface, drag along it, or otherwise interact with the surface, and are tracked with a laser. This allows the surface height (topography) to be mapped out, exactly like a topographical map of a mountain, just a lot smaller. What's more, using different types of interactions and different tip materials, scientists can use SPM to measure an ever-expanding range of material properties. So far, we can map out conductivity, chemical conductivity, stiffness, adhesiveness, and many other properties, and they can all be measured on scales ranging from atoms to millimeters.

Researchers have used these "topographical maps" (called micrographs) in a number of ways. In my own investigations, I have used SPM to see how well materials mix, how they crystallize, how they transport charges themselves, and more. These observations are recorded using descriptors and metrics, such as conductivity or stiffness. So, each pixel in the micrograph can be related to a physical value. This allows imaging of the properties to allow us to see different material domains.

Measuring the imaged domains and features of these micrographs consistently and accurately is a tedious and non-trivial task. The pixel values themselves are informative, but how many pixels show these values? How big are the clusters? Do they have any alignment? These are all features that humans can see easily, so the _modus operandi_ has been to measure these features by hand. This is... less than optimal. Not only is it time-consuming, it's somewhat subjective and, therefore, has not as descriptive as it could be. These micrographs have troves of information in them that we can see, but because we but cannot measure all of the features of all of the domains in the dozens of images a single project produces, we cannot quantitatively describe these morphologies. So, along with my collaborators, we have applied established computer vision and machine learning techniques to automatically recognize and label these morphological domains, and to then quantitatively measure their features.

# The Work We've Done

There is an ever-expanding arsenal of machine learning tools, each one specializing in certain tasks or data structures and lagging in others. So, which one should be used to algorithmically detect the domains? Honestly, I don't think that any one tool can achieve that, so we've built an open-sourced toolkit that facilitates their use in custom combinations and series. The workflow is divided into 3 main sections and use different tools for each one. 

### Step 1: Preprocessing and Feature Selection

First, data is loaded and the features we want our models to see are enhanced by outlier extraction. As all data scientists know: clean data produces good results. Z-score thresholding and fast Fourier transforms can be used to remove the background and outliers. Once these outliers and noise are removed, we use principle components analysis (PCA) to further enhance our features. PCA is a cool technique that's used to reduce the amount of data needed to communicate information given by the dataset. This is where knowing about the data a little more helps describe what we're doing. These SPM images are really just a stack of images, with each layer in the stack describing a different property of the image. So, just like a color display has 3 values for a single pixel (red, blue, green), our images can have many different values that help describe the pixel. PCA reduces the number of image channels. This means that it reduces data redundancy and size, while retaining the most important information. This also makes it easier to for the next step of our workflow to do its job.

### Step 2: Semantic Segmentation

The next section of our workflow is "semantic segmentation", which is to label each and every pixel in the image with a class label. The user specifies the number of classes, based on what they know about the system. For instance, with a binary mixture of P3HT:PC$_{60}$BM, I segmented into 2 classes, which correspond to the polymer-rich and fullerene-rich regions. If I also expected to see a mixed-phase region, then I could deconvolute the signals into 3 different phase labels. For this step, we use the Gaussian mixture model (GMM) GMM is easier to think about if we just look at one layer of our SPM data. Here's a modulus scan of a thin film OPV from my own research.

Image, histogram, GMM classification, and GMM classification histogram

When you look at all the values of the pixels and plot them as a histogram, then you can see that there are 2 primary "lumps" in the data, corresponding to pixels associated with the two phases. When I tell the GMM that I'm expecting 2 components in this film, it divides those signals into the 2 most closely Gaussian curves it can. You can see the results of that, and how the divided signals correspond to the actual pixel labels, on the right.

As I've alluded to, semantic segmentation does require a little bit of knowledge about the imaged system. In the paper linked at the top, I lay out some of the most important of these considerations with experimental data. That said, because GMM is unsupervised, it does not require training to segment, like a neural network would. So, it's more readily applicable to other materials and morphologies.

### Step 3: Instance Segmentation

So, now that each and every label has a phase label, the next step is to cluster pixels into morphological domains. As I said, there are a lot of different possible tools to use, and some are better at some things than others. This is especially true at the instance segmentation step. In the work we've done so far, the main consideration is anisotropy of the domains, which is how similar the features are in different directions. For example, long, skinny things have high anisotropy. Round things are very isotropic.

When we want to group pixels into isotropic grains, we use persistence watershed segmentation (PWS). Like water flowing downhill into creeks and over edges, the watershed segmentation method uses gradient decent of a single channel to identify edges and grain boundaries. Depending on the set threshold, the resulting domains can be over-segmented, but grouping the smaller clusters together into larger grains addresses that. To decide how much to merge, the PWS method has the additional ability to calculate the number of grains that would result from a range of different merging thresholds. The point where this graph has a sharp decrease in the number of grains is typically close to the point where all of the background noise and internal edges are merged together, leaving only the dominant edges between actual domains.

Now, we use height as the default channel for PWS, but in reality any channel could be used. In fact, it is often better to utilize one of the PCA channels that were extracted earlier even be better to pass one of the principle components from PCA to this method, so that only morphological boundaries are segmented over, rather than just topographical boundaries. This is a really powerful method that seems to do particularly well with highly symmetric morphologies. However, when the domains appear highly anisotropic, PWS tends to over-segment the domains into isotropic sub-domains, as when applied to P3HT nanowires or annealed P3HT:PC$_{61}$BM thin film.

When the image we are analyzing is highly anisotropic, connected components labeling performs far better. Rather than looking for boundaries in the image, this method looks for continuity between the GMM labels. Essentially, if a pixel is touching another pixel with the same GMM label, then are considered to be part of the same domain. So, because we are looking for continuity, rather than boundaries, the anisotropy of the P3HT:PC$_{61}$BM thin film and nanowires don't affect the segmenter, the highly irregular boundaries and high aspect ratios naturally emerge. However, connected components labeling doesn't do well with highly isotropic domains that are tangential– it connects those large domains that may actually be separate. Also, when there are a lot of small domains dispersed throughout a film, the likelihood that domains of the same material will touch each other increases. The result is that the small domains of the annealed P3HT film are over-connected and the morphology isn't correctly labeled. 

It's a good thing that we used such different material morphologies in developing m2py, because otherwise we would have used just a single instance segmenter and the toolkit wouldn't be nearly as versatile. Similarly, we also wanted to see how our model would perform on different types of SPM. So, we imaged P3HT nanowires with 2 other types of SPM that are really useful for polymers: amplitude-modulated, frequency-modulated force microscopy (AMFM) and conductive AFM (C-AFM). These two methods provide the models with even fewer property channels (4 and 2, respectively) than the QNM we've been looking at, so the data density is lower. To make things more difficult, the tips of the probes are also wider in diameter, meaning they have worse spatial resolution. Regardless, using the same m2py workflow as before, the domains of these nanowires were correctly labeled. As you can see below, despite losing resolution both spatially and informationally, the GMM semantic segmenter still picks out the nanowires from the background really easily. More impressive, though, is the instance segmenter.

# image of amfm and c-afm results


# What Next?

M2Py is meant to recognize and label ground-truth morphology in SPM images. Tthere are a lot of really good things we can do with supervised learning to corrolate chemical environments, processing conditions, and performance, but first we need some physical handle to link them. As a polymer engineer/scientist person, I believe that morphologhy is the best for this because it is affected by all of the inputs, and directly affects the output (performance in a device). But in order to get computers to link these inputs and outputs through morphology, it needs to be able to see the morphology. This work directly addresses that need. Now, we can take in SPM images and spit out endlessly analyzable morphology maps, which are limited only by experimental resolution.

The next step is to link morphology and performance. We also need to figure out ways to describe what's different between the morphologies. More on those efforts in another post.

### Acknowledgements

This work is the result of a phenomenal team. Diego Torrejon, Patrick O'Neil, and myself are all co-first authors of the linked paper. Anton Resing, Lucas Flagg, and Sara Holliday all helped me with the data collection, and Jon Onorato helped frame the story and edit the paper. Finally, my advisor at the University of Washington, Prof. Luscombe, also needs to be acknowledged for her patience and support. Thank you all.