# Setting Up Jupyter with Berkeley AMI

To get Jupyter Notebook to work on the Berkeley AMI, I had to do the following:

## Install Anaconda3
Instructions [here](https://conda.io/docs/user-guide/install/linux.html).

## Install New Environment with Python 2.7
This is important because the version of Spark on the AMI is only compatible with Python 2.

```bash
$ conda create -n yourenvname python=2.7 anaconda
```

## Activate Your Virtual Environment

```bash
$ source activate yourenvname
```

Note: You'll have to do this each time that you launch Jupyter Notebook, so that you are launching with the correct Kernel.

## Configure Jupyter Notebook Server

Instructions [here](http://jupyter-notebook.readthedocs.io/en/stable/public_server.html). <br>
I only used the section titled "Running a public notebook server". The other parts are really just for making it more secure, which is probably good to know, but I didn't feel like it was necessary for now.

## Open Ports in AWS Managment Console

If you recall from Lab 1, we had to open up some TCP/IP ports for incoming traffic in the AWS Management Console. Make sure that whatever setting that you have for "c.NotebookApp.port = " (from the above instructions) matches which ports you have forwarded for Jupyter.

## Install py4j package
With the python 2.7 environment activated, install py4j package using conda. Spark is dependent on this library.

```bash
$ conda install py4j
```

## Launch Jupyter Notebook and Connect
Make sure that you are in your python 2.7 environment and launch the notebook. Note that the root of your Jupyter Notebook starts at the current directory of your bash shell. Make sure you launch it from the right place.

```bash
$ jupyter notebook
```

To connect to your Notebook from your local browser, go to:
http://<your-ec2-domain>:<port>
<br>
<port> should be the port that you setup in the jupyter configuration (and AWS port forwarding)

## Configuring Spark in Jupyter
You need to run a few lines of code to make sure that python can find pyspark that is installed on the AMI. There is also a way to set up a config file to run this automatically every time for Jupyter, but I didn't bother for now.

```python
# notebook setup
import os, sys
sys.path.append('/data/spark15/python')

import pandas as pd

from pyspark import SparkContext
from pyspark.sql import SQLContext
from pyspark.sql import HiveContext
from pyspark.sql.types import *
import pyspark.sql.functions

sc = SparkContext("local", "hospitals_eda")
sqlContext = SQLContext(sc)
hc = HiveContext(sc)

# from IPython.core.interactiveshell import InteractiveShell
# InteractiveShell.ast_node_interactivity = "all"
```

You can import libraries however you want, but the important part here is that you append the correct path using sys.path.append('path/to/spark/python'). The Spark and Hive context are also important to execute.
