# Hardware Semantic Versioning

In the software development process, continuous integration tools and package managers,
many different versioning schemes are used. One from them is the Semantic Versioning standard
available at http://semver.org/spec/v2.0.0.html. It clearly defines how to maintain version
numbers when changes and patches are introduced. It also integrates easily with the GitFlow
workflow.

After some time using both the SemVer and GitFlow, I decided to try to apply similar rules
to hardware development, mainly for system components and PCBs revision marking. Some of the
reasons behind this are:

  * there should be a common and standard way of marking board versions/revisions instead of
    a multitude of different styles like rev1, rev.1, rev1.1, revA, revA.1
  * it should be possible to check if two components of a system are compatible with each other
    (i.e. if a component is broken, if we are able to replace it with a different one)
  * it should be possible to check if a firmware version is compatible with a given component
    or PCB revision


## Adopting the SemVer 2.0.0 standard for hardware

The whole SemVer standard applies with all of its formatting requirements. There are some additional
notes covering the hardware aspects:

  * the major version number is increased when an incompatible change is introduced in the system.
    It means that components with different major versions cannot be used interchangeably without
    making changes to the rest of the system. Examples of such incompatible changes are missing or
    removed (deprecated) features, which are not detectable by the rest of the system at runtime,
    missing or different connectors and pinouts (which is roughly equivalent to a different software
    API), different board size, incompatible mounting hole positions, much larger power requirements,
    etc. In other words, one should be able to insert a component with the same major version and
    the same or higher minor version in place of an old (broken) component without any problems.

  * the minor version number is increased when a new feature is added to the component like more inputs,
    more outputs, low power mode capability, more indicator LEDs, more protection circuits, etc. These
    are all changes that were not required in the original design but were later added to increase the
    component capabilities. The system may depend on a non-zero minor version if a given featue is
    required. A system requiring lower minor version must be able to work with a component with a higher
    minor version (and the same major version), optionally without using the new features.

  * the patch version number is increased when a small fix is added to the component. This may include
    changing part revisions, footprints, board routing, etc. These changes usually provide no additional
    features. However they may increase the component stability, interference protection, lower emissions,
    LCD readability, keyboard/button endurance, general robustness.

  * prerelease versions can be used to describe prototype components and boards. Usually they can be used
    in place of a release version, but the release version has precedence.
  
  * build metadata can be used to include the manufacturing date or batch number in the version. In each case,
    two components or boards which differs only in the build metadata field must be totally equal, although
    they may be made by different manufacturers or technological processes.

