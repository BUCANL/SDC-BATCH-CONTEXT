---
layout: lesson
root: .  # Is the only page that doesn't follow the pattern /:path/index.html
permalink: index.html  # Is the only page that doesn't follow the pattern /:path/index.html
---
The Batch Context extension for EEGLAB provides an interface for generating data processing pipelines and executing them on multiple data files either locally or on remote clusters. This extension also provides the tools needed to manage remote server addresses and facilitates working with remote environments.

> ## Prerequisites
> To begin this lesson, you will need to:
> * Understand the concepts of files and directories, and the concept of a “working directory”.
> * Know how to start up MATLAB, and access EEGLAB.
> * Have familiarity with a BASH shell using `ssh` and `scp`.
{: .prereq}

# Benefits
* Provides a simple front end interface for researchers to batch process scripted analysis pipelines on multiple data files
* Save script and configuration files that contain everything that is required to replicate an analysis pipeline across data files or studies
* Run jobs on clusters in highly parallel environments
* Write your own adapters for your own custom schedulers in a contained way
* Submit jobs with dynamic resource parameters that optimize resource allocation leading to lower queue times in competitive resource allocation environments

# Outside Refererences
The [Wiki](https://github.com/BUCANL/Batch-Context/wiki) page for Batch Context functions as a reference manual for the plugin and contains explanations for specific lower level functionality.