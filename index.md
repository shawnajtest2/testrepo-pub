---
title: Detailed Build and Installation Instructions for SST 6.0.x 
published: true
category: SSTDocumentation
SST-RELEASE_version: 6.0.x
SST-CORE_version: 6.0.x
SST-CORE-EXAMPLE_version: 6.0.0
SST-CORE-EXAMPLE-SHORT_version: 6.0
SST-ELEMENTS_version: 6.0.x
SST-ELEMENTS-EXAMPLE_version: 6.0.0
SST-ELEMENTS-EXAMPLE-SHORT_version: 6.0
BOOST_version: 1.56
---
# {{page.title}}

<!-- These are commonly used links below -->
[SiteBaseURL]:               {{site.baseurl}}/
[ReleaseNotes]:              {{site.baseurl}}/SSTPages/SSTmicroReleaseV6dot0dot0
[DetailedBuildInstructions]: {{site.baseurl}}/SSTPages/SSTBuildAndInstall6dot0dot0SeriesDetailedBuildInstructions
[AdditionalComponents]:      {{site.baseurl}}/SSTPages/SSTBuildAndInstall6dot0dot0SeriesAdditionalExternalComponents
[ElementSummaryInfo]:        {{site.baseurl}}/SSTPages/SSTDeveloperElementSummaryInfo
[ElementReleaseMatrix]:      {{site.baseurl}}/SSTPages/SSTElementReleaseMatrix
[DownloadsPage]:             {{site.baseurl}}/SSTPages/SSTMainDownloads
[ConfigureOptionsCore]:      {{site.baseurl}}/SSTPages/SSTBuildAndInstall6dot0dot0SeriesSSTConfigureOptionsCore
[ConfigureOptionsElements]:  {{site.baseurl}}/SSTPages/SSTBuildAndInstall6dot0dot0SeriesSSTConfigureOptionsElements
[BuildPrerequisites]:        {{site.baseurl}}/SSTPages/SSTUserSSTBuildPrerequisites
[MacOSXInstallOptions]:      {{site.baseurl}}/SSTPages/SSTUserBuildAndInstallMacOSXInstallOptions
[MacConfigAutotools]:        {{site.baseurl}}/SSTPages/SSTUserBuildAndInstallSSTOnMacOSXUsingCustomBuild
[MacConfigMacPorts]:         {{site.baseurl}}/SSTPages/SSTUserBuildAndInstallSSTOnMacOSXUsingMacPorts
[SSTUsers]:                  https://sst-simulator.org/mailman/listinfo/sst-users

{:toc .floatbox}
* TOC

<!-- UNCOMMENT BELOW TO MARK AS PRELIMINARY -->
<!--
**<span style="color:red"> (PRELIMINARY RELEASE DOCUMENTATION)</span>**
**<span style="color:red"> THIS DOCUMENTATION IS FOR RELEASE EVALUATION ONLY</span>**
-->

## Overview 
SST is an open source, cross platform simulation platform that provides a framework to connect multiple simulated hardware object including CPUs, network, memory, etc. Simulations using the toolkit can be run either single node, or run on multiple nodes via MPI. The toolkit provides a parallel discrete event core as well as several programming interfaces including classes to manage random number generation, statistics handling, simulation output and efficient memory pooling for simulation events. The most recent performance evaluation has shown that SST can scale to simulate beyond 1.5M objectss and operate efficiently on simulations up to 128 dual-processor nodes.

These instructions are intended to walk the user through a detailed set of steps to build and install SST.  It is intended for users with intermediate knowledge in the operation of  Unix/Linux/OSX environments.

**NOTE: The following instructions build a basic version of SST and its required external components.  Additional (optional) external components can be built to provide additional features.  Instructions for these additional components are available at  [Instructions for Additional External SST Components][AdditionalComponents].**

The SST [Release Notes][ReleaseNotes] identify what operating systems, compiler and external component combinations have been tested and proven to work with SST {{page.SST-RELEASE_version}}.

**NOTE: Using combinations other than what is identified in the [Release Notes][ReleaseNotes] may cause build failures and/or unexpected results.**

A detailed list of elements provided with the SST distribution are available at [SST Element Summary][ElementSummaryInfo] and  [SST Element Release Matrix][ElementReleaseMatrix].

If you encounter difficulties, go to the [SST Support]({{site.baseurl}}/SSTPages/SSTMainSupport) page.

**NOTE: Building SST can sometimes be cumbersome and error prone due to the sheer number of combinations of operating systems, compiler versions and external required components.  It is STRONGLY recommended that users closely follow these instructions.**

{: #GeneralBuildandInstallInformation}
## General Build and Install Information 
SST does not require root/superuser privileges for installation. Prior to installation, the user will need to decide on directories where SST and its prerequisites can be built and installed. This location should have sufficient space and permissions for building and storage of the libraries and other binaries.

The user is expected to have an intermediate knowledge in the operation of Unix/Linux/OSX environments.  They should be able to modify their environment variables, and understand basic build/installation methods, and manipulate the file system.

**NOTE: SST and all of its external components should be built using the same compiler type and version.  Using different types or versions can create binary incompatibilities that may be difficult to diagnose.**

### Shell Used For Instructions  
These instructions are targeted for users running Bourne-compatible shells.  Users running different shells will need to adjust these instructions as necessary.

### Specific Instructions for Operating Systems and Compilers  
There are 2 main operating systems used for SST: Linux and Mac OSX.
  
 * In the Linux OS, gcc is the only supported compiler.  
 * In the Mac OSX OS, clang (XCode) is the only supported compiler

In the instructions that follow, the following icons are used to point out specific issues for an operating system or an operating system with a specific compiler.

<img src="/SSTImages/Logo_Linux.png" height="40" width="40"> - Represents Instructions specific to Linux Operating Systems.

<img src="/SSTImages/Logo_OSX.png" height="40" width="40"> - Represents Instructions specific to Mac OSX Operating Systems.

----

### Required system tools
Refer to [SST Build Prerequisites][BuildPrerequisites] for a list of required system tools necessary for building SST.

<img src="/SSTImages/Logo_Linux.png" height="40" width="40"> - Most Linux Systems contain all the necessary tools required to build SST (either pre-installed or available via the distro.), however some older versions may need their system tools updated.  
**NOTE** - Newer versions of components for SST (such as Boost) can cause incompatibilites.  It is recommended that the user use the versions defined in the SST [Release Notes][ReleaseNotes].    

<img src="/SSTImages/Logo_OSX.png" height="40" width="40"> - Mac OSX systems will need the following to be installed on the system before building SST see ([Mac OSX Install Options][MacOSXInstallOptions]):

   * XCode and XCode Command line tools 
      * XCode 6.x Includes only clang Compiler
      * Previous releases of XCode (before 5.1) included clang and gcc compilers
   * Autotools 
      * Built separately.  ([Configuring Autotools on Mac][MacConfigAutotools])
      * Installed via MacPorts (below)
   * MacPorts package manager ([Configuring Mac OSX for SST using MacPorts][MacConfigMacPorts])
      * gcc 4.8 from macports (optional)
      * Python  2.7 from macports
      * Autotools (automake, autoconf, libtool) from macports

----

### Example Build and Install Directories 
These instructions will use the following conventions (the user can adjust these as they see fit):

 * Download directory `$HOME/scratch`
   * This directory will contain downloaded source code packages for SST and its dependencies.
   * The following directories should be created on the users machine
     * `$HOME/scratch`
     * `$HOME/scratch/src`

 * Installation directory `$HOME/local`
   * This directory will be the installation directory for SST and its dependencies.
   * The following directories should be created on the users machine
     * `$HOME/local`
     * `$HOME/local/packages`

### Other Possible Tasks for Users 
For most users, following these instructions will get SST up and running.  However there may be rare circumstances that the user will have implement additional changes to their system to resolve problems.  

The user may have to possibly modify their system in the following possible ways:

 * Modifying the `$PATH` environment variable to include directories as needed
 * Modifying compiler options in makefiles so that the appropriate directories are searched during the build.
 * Modifying the `$LD_LIBRARY_PATH` environment variable to include directories that are searched for shared libraries at runtime (for .so files).
 * For Mac OSX, modifying the `$DYLD_LIBRARY_PATH` environment variable to include directories that are searched for shared libraries at runtime (for .dylib files).
 * Modifying the `$MANPATH` environment variable to include directories so that they are searched for man page documentation.

It is expected that the user has sufficent knowledge in Linux and/or Mac OSX operating systems to manage and understand these changes. 
 
----
----



## Building A Basic SST System 
SST requires 1 external component (Boost) and recommends 1 external component (OpenMPI) to be built, installed and available on the users machine.  After these components are available, then SST itself can be built and installed.

Additional external components can be built to provide additional features.  Instructions for these additional components are available at  [Instructions for Additional External SST Components][AdditionalComponents].

**NOTE: If OpenMPI is not available, the SST-Core must be configured with the `--disable-mpi` option (see configure instructions below).  With OpenMPI disabled, the user will be unable to run multi-node simulations.** 

### External Dependent Components 

#### OpenMPI 1.8.8 (Recommended) 
SST {{page.SST-RELEASE_version}} is regularly tested with OpenMPI 1.8.8 (not later versions), and this version is known to work with SST.  Installation of an MPI package from source code is typically unnecessary, since many Linux distributions provide OpenMPI as an optional installable package.  Instructions for building OpenMPI 1.8.8 follow.

   * **NOTE: Do not use the base release of OpenMPI 1.8 (openmpi-1.8.tar.gz)**  

OpenMPI can be obtained online at [http://www.open-mpi.org/software/ompi/v1.8/](http://www.open-mpi.org/software/ompi/v1.8/)

`1.` Download the OpenMPI archive file `openmpi-1.8.8.tar.gz` to `$HOME/scratch/src`

`2.` Unarchive the compressed tar file

```
$ cd $HOME/scratch/src
$ tar xfz openmpi-1.8.8.tar.gz
$ cd openmpi-1.8.8
```

`3.` Decide on an installation location.  For this example: `$HOME/local/packages/OpenMPI-1.8.8`. Note this location will contain `bin`, `lib`, `include`, and other directories for use of OpenMPI.

`4.` Set the home directory environment variable of the OpenMPI installation.  

```
$ export MPIHOME=$HOME/local/packages/OpenMPI-1.8.8 
```

`5.` Configure OpenMPI to be installed in `$MPIHOME`

``` 
$ ./configure --prefix=$MPIHOME
```

`6.` Build and install OpenMPI 

``` 
$ make all install
```

`7.` Update your `PATH` environment variable so that it contains the `bin` directory of the OpenMPI installation location.  Also set the `MPICC` and `MPICXX` variables to point to the correct MPI compiler options.  initialization file.

```
$ export PATH=$MPIHOME/bin:$PATH  
$ export MPICC=mpicc   
$ export MPICXX=mpicxx
```

`8.` OPTIONAL - In some cases, it may be necessary to specify the location of the OpenMPI library files for proper operation of SST.

``` 
$ export LD_LIBRARY_PATH=$MPIHOME/lib:$LD_LIBRARY_PATH
$ export DYLD_LIBRARY_PATH=$MPIHOME/lib:$DYLD_LIBRARY_PATH
$ export MANPATH=$MPIHOME/share/man:$DYLD_LIBRARY_PATH
```

`9.` To make the changes to the environment variables a permanent change, it may require editing of your login shell's initialization file.

----

#### Boost {{page.BOOST_version}} (**Required**)
Boost {{page.BOOST_version}} can be obtained online at [http://sourceforge.net/projects/boost/files/boost/1.56.0/](http://sourceforge.net/projects/boost/files/boost/1.56.0/)

SST {{page.SST-RELEASE_version}} makes use of Boost {{page.BOOST_version}}. If a Boost installation is in the default compiler search paths and it is not the Boost installation you wish to use, it's best to remove that copy of Boost if possible.

`1.` Download Boost archive file `boost_1_56_0.tar.gz` to `$HOME/scratch/src`

`2.` Unarchive the compressed tar file
 
```
$ cd $HOME/scratch/src
$ tar xfz boost_1_56_0.tar.gz
$ cd $HOME/scratch/src/boost_1_56_0
```

`3.` Decide on an installation location.  For this example: `$HOME/local/packages/boost-1.56`. Note this location will contain `lib`, `include`, and other directories for use of Boost.

`4.` Set the home directory environment variable of the Boost installation.  
 
```
$ export BOOST_HOME=$HOME/local/packages/boost-1.56 
```
`5.` Configure Boost to be installed in `$BOOST_HOME`
 
```
$ ./bootstrap.sh --prefix=$BOOST_HOME
```

`6.` Build and install Boost {{page.BOOST_version}} in `$BOOST_HOME` (NOTE: Boost MPI is no longer needed for SST {{page.SST-RELEASE_version}})
 
```
$ ./b2 install
```

`7.` OPTIONAL - In some cases, it may be necessary to specify the location of the Boost library files for proper operation of SST.  
 
```
$ export LD_LIBRARY_PATH=$BOOST_HOME/lib:$LD_LIBRARY_PATH
$ export DYLD_LIBRARY_PATH=$BOOST_HOME/lib:$DYLD_LIBRARY_PATH
```

`8.` To make the changes to the environment variables a permanent change, it may require editing of your login shell's initialization file.


----
----

   
### SST {{page.SST-RELEASE_version}} New Build Architecture 

**NOTE - SST {{page.SST-RELEASE_version}} is now made up of two separate packages:**

   * The first package is the SST-Core which contains the simulation engine and API interface to simulation elements.  
   * The second package is the SST-Elements which contain a number of simulation and support elements.
      * This new "independent core and elements" architecture allow for easier development and support for 3rd party elements.
      * The SST-Elements package is optional, however it does contain a number of support elements that can be used by 3rd party elements. 
      * The SST-Core **MUST** be built and **Installed first** before any elements can be built. 

----
----
  
{: #SSTCore}
### SST CORE {{page.SST-CORE_version}} Build and Installation

After the prerequisite Components (OpenMPI and Boost) have been successfully installed, the SST-Core can then be built and installed.

   * **NOTE: The following instructions assume SST-Core is being built from the tarfile distribution.**  
      * If desired, the latest stable development version of SST-Core can be acquired from [here](https://github.com/sstsimulator/sst-core).  See [What's Next](#WhatsNext) for more information.
   
`1.` Obtain the SST-Core {{page.SST-CORE_version}} source code and save it in `$HOME/scratch/src`. The tarfile can be downloaded from [here](https://github.com/sstsimulator/sst-core/releases).
 
   * Note: The following instructions assume SST-Core version {{page.SST-CORE-EXAMPLE_version}}.  The user will need to adjust the directory name to match the SST-Core version.
 
```
$ cd $HOME/scratch/src
$ tar xfz sst-core-{{page.SST-CORE-EXAMPLE-SHORT_version}}.tar.gz
$ cd $HOME/scratch/src/sst-core-{{page.SST-CORE-EXAMPLE-SHORT_version}}
```

`2.` Set the home directory environment variable to the SST-Core installation (i.e. where you want the SST-Core files installed).  
 
```
$ export SST_CORE_HOME=$HOME/local/sst-core-{{page.SST-CORE-EXAMPLE-SHORT_version}} 
```

`3.` Review prerequisite package installation locations and use this to construct a configure line for SST-Core.  For more information on SST-Core configuration click here or run  [$ ./configure --help][ConfigureOptionsCore] 

`4.` Configure SST-Core, being sure to make `configure` reference the location of SST-Core's local prerequisite packages.
 
```
$ ./configure --prefix=$SST_CORE_HOME --with-boost=$BOOST_HOME [other configure settings as needed]
```

`5.` Build and install SST-CORE
 
```
$ make all
$ make install
```

`6.` Update your `PATH` environment variable so that it contains the `bin` directory of the SST-CORE installation location. 
 
```
$ export PATH=$SST_CORE_HOME/bin:$PATH
```

`7.` To make the changes to the environment variables a permanent change, it may require editing of your login shell's initialization file.

----
----
  
{: #SSTElements}
### SST Elements {{page.SST-ELEMENTS_version}} Build and Installation

After the prerequisite Components (OpenMPI, Boost and [**SST-Core**](#SSTCore)) have been successfully installed, SST-Elements can then be built and installed.

   * **NOTE: The following instructions assume SST-Elements is being built from the tarfile distribution.**  
      * If desired, the latest stable development version of SST-Elements can be acquired from [here](https://github.com/sstsimulator/sst-elements).  See [What's Next](#WhatsNext) for more information.
   
`1.` Obtain the SST-Elements {{page.SST-ELEMENTS_version}} source code and save it in `$HOME/scratch/src`. The tarfile can be downloaded from [here](https://github.com/sstsimulator/sst-elements/releases).
 
   * Note: The following instructions assume SST-Elements version {{page.SST-ELEMENTS-EXAMPLE_version}}.  The user will need to adjust the directory name to match the SST-Elements version.
 
```
$ cd $HOME/scratch/src
$ tar xfz sst-elements-{{page.SST-ELEMENTS-EXAMPLE-SHORT_version}}.tar.gz
$ cd $HOME/scratch/src/sst-elements-{{page.SST-ELEMENTS-EXAMPLE-SHORT_version}}
```

`2.` Set the home directory environment variable to the SST-Elements installation (i.e. where you want the SST-Elements files installed).  
 
```
$ export SST_ELEMENTS_HOME=$HOME/local/sst-elements-{{page.SST-ELEMENTS-EXAMPLE-SHORT_version}} 
```

`3.` Review prerequisite package installation locations and use this to construct a configure line for SST-Elements.  For more information on SST-Elements configuration click here or run  [$ ./configure --help][ConfigureOptionsElements] 

`4.` Configure SST-Elements, being sure to make `configure` reference the location of SST-Elements's local prerequisite packages.
 
```
$ ./configure --prefix=$SST_ELEMENTS_HOME --with-sst-core=$SST_CORE_HOME [other configure settings as needed]
```

`5.` Build and install SST-ELEMENTS
 
```
$ make all
$ make install
```

`6.` Update your `PATH` environment variable so that it contains the `bin` directory of the SST-ELEMENTS installation location. 
 
```
$ export PATH=$SST_ELEMENTS_HOME/bin:$PATH
```

`7.` To make the changes to the environment variables a permanent change, it may require editing of your login shell's initialization file.



----
----

## Testing SST {{page.SST-RELEASE_version}} Functionality 
`1.` Run a very simple sanity test
 
```
$ sst --version
```

`2.` Run a very simple simulation
 
```
$ sst <Path to SST-Elements Source Directory>/src/sst/elements/simpleElementExample/tests/test_simpleRNGComponent_mersenne.py
```

----
----

{: #WhatsNext}
## What's Next... 
   * It is recommended that the user update their login shell scripts to include updated `$PATH`, `$LD_LIBRARY_PATH` and `$DYLD_LIBRARY_PATH` settings.  
   * Refer to the documentation at  [sst-simulator.org]({{site.baseurl}}/)  for more information on SST operation.  
   * Additional external components can be built to provide additional features.  Instructions for these additional components are available at  [Instructions for Additional External SST Components][AdditionalComponents].
   * Developers and researchers are encouraged to be part of SST Development.  Register to be part of the team at [SST User Registration][SSTUsers].
   * To clone the latest project source code on the devel branch (from GitHub):
      * Using Git
        * `git clone -b devel https://github.com/sstsimulator/sst-core.git`
        * `git clone -b devel https://github.com/sstsimulator/sst-elements.git`
      * The build process is the same as described above, except the user must perform a `$ ./autogen.sh` to create a configure file.  This must occur before running the configure script.
      * The source for SST-Core and SST-Elements are stored in two repositories ([SST-Core](https://github.com/sstsimulator/sst-core) and  [SST-Elements](https://github.com/sstsimulator/sst-elements)).  Each repository contains two primary development branches (devel and master):
         * The devel branch is under active development.  
            * Code is submitted to the devel branch via Pull Requests.  
            * The Pull Requests to both repositories are automatically tested (but not exhaustively) before merging.  Due to the non-exhaustive tests, the possibility exists that the code in the devel branch may be non-operational or contains bugs.
         * A full exhaustive test suite is run nightly on the devel branch of SST (Core and Elements).  When these tests pass, then the devel branches are merged into the appropriate master branches.
         * The master branch is exhaustively tested and will contain the latest stable version of SST-Core and SST-Elements
            * To retrieve the master branches:
               * `git clone -b master https://github.com/sstsimulator/sst-core.git`
               * `git clone -b master https://github.com/sstsimulator/sst-elements.git`

----
----

## Configuration selection for Internal Elements and Optional/Required External Components 

Below is a description of the Elements provided with SST {{page.SST-RELEASE_version}}.  Most elements are automatically included in the configuration.  Some elements are only enabled when the --with-"external component" is set.  For more information, see [Instructions for Additional External SST Components][AdditionalComponents]

 * **Boost**
   * **Required by SST-CORE Configure:** --with-boost="path to Boost Library"
 * Ariel
   * Enabled by SST-Elements Configure: --with-pin="path to Intel Pin"
 * cacheTracer
   * Always Enabled 
 * cassini
   * Always Enabled 
 * ember
   * Always Enabled 
 * event_test
   * Always Enabled 
 * firefly
   * Always Enabled 
 * hermes
   * Always Enabled 
 * memHierarchy
   * Always Enabled 
   * Optional Features Enabled by SST-Elements Configure: --with-dramsim="path to DRAMSim2"
   * Optional Features Enabled by SST-Elements Configure: --with-nvdimmsim="path to NVDimmSim"
   * Optional Features Enabled by SST-Elements Configure: --with-hybridsim="path to HybridSim"
 * merlin
   * Always Enabled 
 * miranda
   * Always Enabled 
 * prospero
   * Always Enabled 
 * qsimComponent
   * Enabled by Configure: --with-qsim="path to QSim"
 * scheduler
   * Always Enabled 
   * Optional Features Enabled by SST-Elements Configure: --with-glpk="path to GLPK"
   * Optional Features Enabled by SST-Elements Configure: --with-metis="path to METIS"
 * simpleElementExample
   * Always Enabled 
 * VaultSimC
   * Always Enabled 
 * zodiac
   * Always Enabled 
 * Zoltan
   * Enabled by SST-Core Configure: --with-zoltan="path to Zoltan"
  
----
----
