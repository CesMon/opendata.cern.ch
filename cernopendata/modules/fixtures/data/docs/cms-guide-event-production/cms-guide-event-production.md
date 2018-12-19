This is a guide on how to produce [CMSSW](https://twiki.cern.ch/twiki/bin/view/CMSPublic/WorkBookCMSSWFramework)
python configuration files that can be used to perform the generation, simulation and/or reconstruction of [Monte 
Carlo (MC)](/docs/cms-mc-production-overview) (or real) collision events.  These events can be used later to
perform physics analyses.

The figure below depicts a simplified scheme of 
the different stages for event generation in CMS. The simple tools and examples presented
here are the easiest way to deal with the processing of MC-generated collisions or
real LHC beam collisions.  Obviously, and since most of the Open Data released by CMS is ready for analysis, these
instructions are likely to be more useful for [MC production](/docs/cms-mc-production-overview).

<p align="center">
<img alt="CMS data flow overview" src="/static/docs/cms-mc-production-overview/diagram.png" width=75%>
</p>

The present directives are essentially a short summary of the more-detailed documentation found in Chapter 6 of the [CMS Offline Workbook](https://twiki.cern.ch/twiki/bin/view/CMSPublic/WorkBook), which is publicly available.

In this document the `cmsDriver` tool is first described.  This is the steering script, which is used to generate almost any configuration needed.  Then, simple examples for MC production and real data reconstruction are provided.


### The cmsDriver tool

The [cmsDriver](https://twiki.cern.ch/twiki/bin/view/CMSPublic/SWGuideCmsDriver) is a tool to create production-solid configuration files from minimal command line options.  Its code implementation, the [cmsDriver.py](https://github.com/cms-sw/cmssw/blob/master/Configuration/Applications/scripts/cmsDriver.py) script, is part of the CMSSW software.  

A summary of the `cmsDriver.py` wrapper's options, with a detailed message about each one, can be visualized by getting the help:

```
cmsDriver.py --help
```

The options list is divided into two sections according to the user's level of knowledge: Options and Expert Options. Here, only the former, the "standar" Options, are listed:

- `-s STEP, --step=STEP`: this option is useful to indicate what kind of steps the user wants to run and in which order. The most common possible values are: 

    - GEN : the generator plus the creation of GenParticles and GenJets
    - SIM : Geant4 simulation of the detector (energy deposits in the detector volumes)
    - DIGI : simulation of detector signal response to the energy deposits
    - L1: simulation of the L1 trigger
    - DIGI2RAW : data format conversion of the digi signals into the RAW format that will be provided in the online system
    - HLT : high level trigger,

  which are usually executed in a single job. Remaining building blocks are executed in a *step 2*:

    - RAW2DIGI : data format conversion of the RAW format into digi signals
    - RECO : full event reconstruction
    - ALCA : production of alignment and calibration streams
    - DQM : code run for DQM
    - VALIDATION : code run for validation

  For fast simulation, things are slightly different. As the fast simulation combines a lot of things, there are only two standard steps:

    - GEN
    - FASTSIM

- `--conditions=CONDITIONS`: this option concerns the alignment and calibration [conditions](docs/cms-guide-for-condition-database) the user wants to apply for producing the dataset.

- `--eventcontent=EVENTCONTENT`: the user can select what event content has to be written out in the output by making use of this option.  The user can choose among those available in the files in [Configuration/EventContent/python/](https://github.com/cms-sw/cmssw/tree/master/Configuration/EventContent/python) directory.

- `--filein=FILEIN`: specifying this option the user can indicate the infile name. For instance, if in the previous step the processing of events has been run up to the HLT, in the next step the reconstruction can be run. In this case the user has to specify what was the output file of the previous generation as infile for the new configuration file.

- `--fileout=FILEOUT`: this option can be used to customize the name for the output file to specify in the configuration file created by cmsDriver.py.

- `-n NUMBER, --number=NUMBER`: indicates the number of events to write out in the event content of the output file. The default is 1.

- `--mc`:  this option, if defined, imposes the processing of simulation. A default value is based on all other defined options.
- `--data`: this option, if defined, imposes the processing of real data. A default value is based on all other defined options.  If neither `--mc` option nor `--data` are specified, it will be determined that the user is requiring simulations.

- `--no_exec`: this option implies to prepare the full python configuration file without execute the *cmsRun* command 
at the end











