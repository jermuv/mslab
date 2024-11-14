# Assumptions

- there is mslab prereqs run earlier (see [DiskHydration](1_DiskHydration.md))
- there is some "scenario" file available (see [LabScenarios](SCENARIOS/LabScenarios.md))

Note: this mslab works even with ASR rules set on!

# Quick and dirty

1. download a fresh mslab.zip file from https://aka.ms/mslab
2. extract content somewhere
   - for example c:\mslabs\lab1
3. edit scenario file
   - LabConfig.ps1
4. run mslab.zip scripts in following order
   1. 1_prereq.ps1
   2. copy windows.vhdx files under "parentdisks" folder that was just created with previous script
   3. modify labconfig if that was not done on earlier steps
   4. run 2_createparentdisks.ps1
   5. run deploy.ps1 (3_deploy was renamed previous step)