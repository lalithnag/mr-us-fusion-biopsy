
# MR-US Fusion Biopsy

MR-US Fusion Biopsy is an intra-operative procedure that involves elastic deformable multi-modal registration between 3D volumes. This project was implemented with MeVis Lab, MDL (MeVisLab Definition Language) & 3D Slicer. This is a Work In Progress, I will keep updating this repo with relevant stuff.

## Multi-modal registration

Without going into specific details about the project, let me talk about multi-modal 3D registration which is applicable to a wide variety of applications.

There are a few broad categories of registration based on the assumptions we make about the two volumes.

### Rigid Registration

When we assume that these volumes are rigid, then what we are doing is a simple *affine transform* of one of the volumes - which is essentially composed of elementary operations such as rotation, translation, scaling and shearing. This kind of registration basically moves the volume in space in a way that preserves lines and doesn't *warp* or *deform* the volume.

#### Implementation

Affine registration can be implemented through the modules **RegistrationManual** or **AffineTransformation3D** modules in MevisLab.

* [AffineTransformation3D](https://mevislabdownloads.mevis.de/docs/2.8/MeVisLab/Standard/Documentation/Publish/ModuleReference/AffineTransformation3D.html) - MeVisLab Module Documentation
* [Affine Transformations](https://mevislabdownloads.mevis.de/docs/2.4/MeVisLab/Resources/Documentation/Publish/SDK/GettingStarted/ch11s04.html) - MeVisLab Topic Documentation
* [Comprehensive MeVisLab Documentation](https://mevislabdownloads.mevis.de/docs/current/MeVisLab/Resources/Documentation/Publish/SDK/MeVisLabManual/index.html)

*There is a major caveat in terms of co-ordinate systems, which is crucial to performing registration in medical volumes. I will explain that in further sections.*

### Elastic Registration

*Elastic* or *Deformable* registration is more representative of how the two medical volumes are related. This is because, there are motion and breathing artefacts from the patient that require a non-uniform mapping between the images. In contrast to affine registration which is a global transform - meaning, the entire volume was transformed by the same parameters; deformable registration is a local transform that acts on local patches of the image. This is performed by computing a vector field. One way to do this is the Demon's algorithm, from Euler-Lagrangian based regularisation methods (🚨 **Hardcore math alert** 🚨).

Generally, when we have two medical volumes, rigid registration is applied first - to sort of understand the global transformation, after which it is made more accurate by elastically deforming the volume.

#### Implementation

Deformable registration can be implemented in MeVisLab using the **ImageWarp** module.

* [ImageWarp](https://mevislabdownloads.mevis.de/docs/3.0/FMEwork/ReleaseMeVis/Documentation/Publish/ModuleReference/ImageWarp.html) - MeVisLab Documentation
* [Comprehensive MeVisLab Documentation](https://mevislabdownloads.mevis.de/docs/current/MeVisLab/Resources/Documentation/Publish/SDK/MeVisLabManual/index.html)

### Co-ordinate Systems

This is a common pitfall when working with registration. If you aren't aware of the different co-ordinate systems you are most likely to spend (waste) a lot of time trying to figure out why things don't fit together. So it would do you a lot of good to equip yourself with this knowledge before-hand.

I am planning to write a separate blog-post outlining the problems I faced, but until then here is a great link that elucidates this concept in medical scenarios:

[Helpful co-ordinate guide](http://www.grahamwideman.com/gw/brain/orientation/orientterms.htm)

### Tips to use MeVisLab

MevisLab unlike other visualisation software has its own scripting language MDL. Out of all the available options such as SimpleITK with Python, 3DSlicer scripting, etc. I found MevisLab to be the most useful of all. This was because it has an extensive library of GUI modules which can be leveraged. We can interface these modules through MDL. Although there is no extensive documentation examples of using MDL, one huge advantage is that all the scripts of the developed modules are readily accessible. This went a long way in helping me design many elements of my algorithm and of the GUI! The forum is also an invaluable resource. 

## Authors

* [Lalith Nag](https://github.com/lalithnag). Drop me a line if you want to know more or you're stuck at some place! :smiley: I will put up more elaborate blog posts soon. Until then, you can email me at lalith.sharan@ovgu.de

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* A **huge** thanks to Dr. Marko Rak & Anneke Meyer :bow:, my project supervisors for guiding me through many tasks and patiently answering all my questions.
* This project was undertaken in the capacity of a *Student Research Assistant* at the *Computer Assisted Surgeries* group at the *Otto-von-Guericke University*, Magdeburg.
