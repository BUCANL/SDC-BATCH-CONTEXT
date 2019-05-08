---
title: "Local Batch Processing using the batch_context Extension"
teaching: 0
exercises: 0
questions:
- "How can scripting procedures be facilitated from a graphical user interface in EEGLAB?"
objectives:
- "Efficiently build scripts that can be applied to multiple files from a GUI"
- "Modify the scripts so that parameters can be modified at run"
keypoints:
- "Easy batch processing"
---

## Running a batch process across files from a GUI in EEGLAB

#### **Building a history to batch later**

In order to run a process across multiple files in a batch a script file that contains the commands to execute is required. Further this script file should contain some form of ability to swap in strings so that things like files names or function parameters can be modified at run time.

The batch_context plugin for EEGLAB provides tools for generating these flexible script files from the EEG.history string.

To get started left do some simple processing from the EEGLAB to get something in the EEG.history string. This first example will simply load an existing data set, downsample the data and then save the result to a new renamed set file.

> ## Scripts almost always begin with a load function and end with a write function
> Because batch processing tends to run in an unsupervised manner (e.g. letting it run over night, or on a remote compute cluster) there is rarely any interaction with the processing. Because you will only accessing the results of the script functions at a later time you will need to access them from a file. 
>
> {: .source}
{: .callout}

![eeglab resamp menu]({{ page.root }}/fig/eeglab_resamp_menu_crop.png)

![resamp gui]({{ page.root }}/fig/resamp_gui.png)

Once that the file has been loaded, the resampling has been performed and then the EEG structure has been save to a new set file we can check the history in the EEGLAB Command Window.

![resamp history]({{ page.root }}/fig/eeglab_resamp_history.png)

In this history we see that the specific file names are stored in the load and save functions so something will need to be done in order to be able to execute this same process across other files.

#### **Saving the EEG.history to a formatted History Template Batching file (*.htb)**

Now that we have seen the history to execute the downsampling on a single named files we want to make the history string generic to any file and capable of running on a batch of files. To do this we use the batch_context extension to save a History Template Batching file.

![save htb]({{ page.root }}/fig/eeglab_savehtb_menu_crop.png)

![save htb browse]({{ page.root }}/fig/savehtb_browse.png)

#### **Edit the resamp.htb file**

Once that the EEG.history is saved to a formatted text file we can open it in any plain text editor to make any modifications that may be required.

![edit resamp htb]({{ page.root }}/fig/edit_resamp_htb.png)

In many cases no modifications are required before the htb file can be applied across multiple files. The write htb functions knows how to find the latest time that EEG was loaded into the workspace (to set the script starting point) and then it also knows that from the data load (or import) line it will be able to determine the file name and path of the files in the history and then replace the specific names with a generalized key string. Strings inside of "[]" in the htb file indicate key strings. These are strings that can be replaced by something else at the run time of the script. [batch_dfn] is a special key string that stands for the "batch data file name" and [batch_dfp] is another special key string that stands for "batch data file path". Whenever we run this in batch more on multiple files this string will execute for every file in a list. Each time that it runs it will replace the [batch_dfn] in the htb file with the current file name in the batch iteration. After it completes all of the string swapping for [batch_dfn] and [batch_dfp] the process will save the resulting string to a *.m file and then ask Matlab to execute it.

Notice that the save function in the htb file has a modified [batch_dfn] string that looks like [batch_dfn,.,-1], this means that the file name will be swapped in at this point but only up to the last ".".

#### Run a history batch template on multiple files

Now that we have a formatted history batch template file that performs the operation that we are interested in carrying out on multiple files we can call this script from the batch_context gui and provide it with a list of files to execute on.

![run resamp htb]({{ page.root }}/fig/eeglab_runhtb_menu_crop.png)

![run htb]({{ page.root }}/fig/runhtb_gui.png)


#### Add custom swap strings to the htb file for flexible execution of the scripts.

[batch_dfn] and [batch_dfp] are not the only strings that can be swapped in the script at run time. In fact, you can add any number of custom swap strings that can then be adjusted from the run htb GUI. Let's replace the target sampling in the resamp.htb script with a [newsamp] key string and the run the batch specifying the sampling rate in the "replace_string" file of the batch config.

![edit newsamp htb]({{ page.root }}/fig/edit_newsamp_htb.png)

![edit newsamp htb]({{ page.root }}/fig/runhtb_newsamp_gui.png)

