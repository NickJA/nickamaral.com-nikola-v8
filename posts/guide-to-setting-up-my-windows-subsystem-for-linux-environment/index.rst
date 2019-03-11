.. title: Guide to setting up my Windows Subsystem for Linux Environment
.. slug: guide-to-setting-up-my-windows-subsystem-for-linux-environment
.. date: 2019-03-10 22:10:12 UTC-04:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

Guide to setting up my Windows Subsystem for Linux Environment    
==============================================================

**Last Updated:** March 10, 2019

Overview
--------

In this guide, I will document my process to setup a fresh Windows 10 OS with the Windows Subsystem for Linux. 
I will be using Ubuntu as the Linux OS, installed from the app store.  I will be doing my development in Visual Studio Code.
Also, as part of this guide, I will setup my GitHub account with SSH key.  I reviewed a few guides, but I know this post_ by Dave was 
my go to.  Also, `these instructions from Microsoft are useful`_. 

.. _post: https://daverupert.com/2018/04/developing-on-windows-with-wsl-and-visual-studio-code/
.. _these instructions from Microsoft are useful: https://docs.microsoft.com/en-us/windows/wsl/install-win10

.. image:: /images/wsl_ubuntu_sm.png


Enable Windows WindowsOptionalFeature
-------------------------------------

One important dependency is that you are running a more recent windows build, build 16215 or later.  Once you have verified the build,
open PowerShell as Administrator and run the following: 

.. code:: PowerShell

    Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux

Install Ubuntu from the Windows store
-------------------------------------


-Nick