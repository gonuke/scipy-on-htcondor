Running python+numpy+scipy on HTCondor
======================================

Author:  Steve Goldstein  sgoldstein@wisc.edu  July, 2014 

To run python with numpy or scipy on condor, you have to ensure that
the execute node has a version of python that includes those
libraries.   CHTC has solved that problem by building such a version of
python and providing a means to install that version on the execute
node.

The purpose of the scipy-on-htcondor project is to give you an easy way to
implement CHTC's solution that can be widely applied.  On the CHTC web
pages, you can read about some of the details and a more general
implementation. 

Step by step instructions:

1. Log on to the CHTC submit node:

   ```
   ssh <netID>@submit-3.chtc.wisc.edu
   ```

1. Make a directory for your python work.

   ```
   mkdir myProject
   cd myProject
   ```

1. Copy the pythonSubmit tar file to your workspace and extract the
archive.

   ```
   wget <URL here...>
   tar zxvf scipy-on-htcondor.tgz
   ```

1. Open a another terminal on your own local machine and transfer the program
   to the indir/ subdirectory.

    ```
    scp /path/to/my/myPythonProgram.py <netID>@submit-3.chtc.wisc.edu:myProject/indir
    ```
       
1. Transfer any other input files to the indir/ subdirectory.

   ```
   scp /path/to/other/necessary_input_files <netID>@submit-3.chtc.wisc.edu:myProject/indir
   ```

1. Now back on the terminal in which you are logged into the CHTC submit node,
   make sure the first line of the python program looks like this:

   ```
   #!/usr/bin/env python
   ```

   It should match the first line of the pythontest.py program:

   ```
   head -1 indir/*py
   ```

1. Edit the condor submit file, following the (albeit, sparse) directions in
   that file:

   ```
   nano queue/process.cmd
   ```

1. Submit the jobs to condor:

   ```
   condor_submit queue/process.cmd
   ```

1. When the jobs have finished, run the cleanup script:

   ```
   queue/cleanupCHTCPython.pl
   ```

Your output will be in the outdir directory.  Check the files in `chtcOutput/`
for errors.


