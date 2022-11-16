# Phase2FastSimLog
track changes and errors log while implementing phase 2 tracker in FastSim

## CMSSW Enviroment
1. login from your local computer: ```ssh -XY alaha@lxplus8.cern.ch```
2. SCRAM ARCH for CMMSW_12_X_ is el8_amd64_gcc10: ```export SCRAM_ARCH=el8_amd64_gcc10```
3. Setting up CMSSW version: ```cmsrel CMSSW_12_4_10_patch1``` (Latest: Oct16,2022)
4. Setting up CMSSW environment: ```cmsenv```

```connect to forked cmssw git repo```

5. git cms-init
6. Getting FastSimulation Package: **git cms-addpkg FastSimulation** 
7. Compile: ```scram build -j32```

NB: Whenever edit any c++ file, do scram build to compile the modified code. python modules are built once.

## Getting WorkFlows
CMSSW repo maintains a list of workflows inside [Configuration/PyReleaseValidation/scripts](https://github.com/alaha999/cmssw/tree/CMSSW_12_4_0_patch1/Configuration/PyReleaseValidation/scripts). Execute the following command in your home area,
> runTheMatrix.py --interactive
 - This should open an interactive shell. You can search for available workflows using regular expression. See [readme](https://github.com/alaha999/cmssw/tree/CMSSW_12_4_0_patch1/Configuration/PyReleaseValidation/scripts#readme) to know how to use it.
> search \*D88.\*TTbar.\*
 - This will show all the available workflows using geometry D88 and for ttbar fragment. I'm only interested in tracking only workflows. All```####.1*(dot 1) workflows are tracking only WF.
 
 ```Phase2FastSim TrackingOnly Workflows```
  - 39434.1 TTbar_14TeV 2026D88_trackingOnly+TTbar_14TeV_TuneCP5_GenSimHLBeamSpot14+DigiTrigger+RecoGlobal+HARVESTGlobal
  - 39424.1 TTbar_13 2026D88_trackingOnly+TTbar_13TeV_TuneCUETP8M1_GenSimHLBeamSpot+DigiTrigger+RecoGlobal+HARVESTGlobal

### Workflow 39424.1 (FullSim)
39434.1 2026D88_trackingOnly+TTbar_14TeV_TuneCP5_GenSimHLBeamSpot14+DigiTrigger+RecoGlobal+HARVESTGlobal

[1]: cmsDriver.py TTbar_14TeV_TuneCP5_cfi  -s GEN,SIM -n 10 --conditions auto:phase2_realistic_T21 --beamspot HLLHC14TeV --datatier GEN-SIM --eventcontent FEVTDEBUG --geometry Extended2026D88 --era Phase2C17I13M9 --relval 9000,100 

[2]: cmsDriver.py step2  -s DIGI:pdigi_valid,L1TrackTrigger,L1,DIGI2RAW,HLT:@fake2 --conditions auto:phase2_realistic_T21 --datatier GEN-SIM-DIGI-RAW -n 10 --eventcontent FEVTDEBUGHLT --geometry Extended2026D88 --era Phase2C17I13M9 

[3]: cmsDriver.py step3  -s RAW2DIGI,RECO:reconstruction_trackingOnly,VALIDATION:@trackingOnlyValidation,DQM:@trackingOnlyDQM --conditions auto:phase2_realistic_T21 --datatier GEN-SIM-RECO,DQMIO -n 10 --eventcontent RECOSIM,DQM --geometry Extended2026D88 --era Phase2C17I13M9 

[4]: cmsDriver.py step4  -s HARVESTING:@trackingOnlyValidation+@trackingOnlyDQM --conditions auto:phase2_realistic_T21 --mc  --geometry Extended2026D88 --scenario pp --filetype DQM --era Phase2C17I13M9 -n 100  


### Potential Phase2 FastSim Command
cmsDriver.py TTbar_14TeV_TuneCP5_cfi --fast **--conditions auto:phase2_realistic_T21** -n 2 **--era Phase2C17I13M9** --beamspot HLLHC14TeV **--geometry Extended2026D88** --datatier GEN-SIM-RECO,DQMIO --eventcontent RECOSIM,DQM -s GEN,SIM,RECOBEFMIX,DIGI:pdigi_valid,DIGI2RAW,RECO:reconstruction_trackingOnly,VALIDATION:@trackingOnlyValidation,DQM:@trackingOnlyDQM --python_filename fastsim_phase2_12X.py --fileout fastsim_phase2_12X.root --pileup=NoPileUp --no_exec

## How to checkout my branch at CMSSW
```
cmsrel CMSSW_12_4_10_patch1
cd CMSSW_12_4_10_patch1/src/
cmsenv
git cms-init
git checkout -b phase2fastsimpixel
git cms-merge-topic alaha999:phase2fastsim_12X
scram b -j32
```
## Error Log

## Change Log
