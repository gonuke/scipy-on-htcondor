Running python+numpy+scipy on HTCondor
======================================

Author:  Steve Goldstein  sgoldstein@wisc.edu
July, 2014 

To run python with numpy or scipy on condor, you have to ensure that
the execute node has a version of python that includes those
libraries.   CHTC has solved that problem by building such a version of
python and providing a means to install that version on the execute
node.

The purpose of pythonSubmit.tgz is to give you an easy way to
implement CHTC's solution that can be widely applied.  On the CHTC web
pages, you can read about some of the details and a more general
implementation. 

Step by step instructions:

1. Log on to the CHTC submit node:

```
ssh <netID>@submit-3.chtc.wisc.edu
```

2. Make a directory for your python work.

```
mkdir myProject
cd myProject
```

3. Copy the pythonSubmit tar file to your workspace and extract the
archive.

```
wget <URL here...>
tar zxvf pythonSubmit.tgz
```

4. a. Copy your python program to the indir/ subdirectory.

```
scp <myLocalMachine>:bin/myPythonProgram.py indir/
```

   b. Copy any other input files to the indir/ subdirectory.

5. Make sure the first line of the python program looks like this:

```
#!/usr/bin/env python
```

It should match the first line of the pythontest.py program:

```
head -1 indir/*py
```

6. Edit the condor submit file, following the (albeit, sparse) directions in
   that file:

```
nano queue/process.cmd
```

7. Submit the jobs to condor:

```
condor_submit queue/process.cmd
```

8. When the jobs have finished, run the cleanup script:

```
queue/cleanupCHTCPython.pl
```

Your output will be in the outdir directory.  Check the files in `chtcOutput/`
for errors.


