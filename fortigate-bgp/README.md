# Overview

## Introduction

This lab guide is a practical, notebook-style introduction to BGP on FortiGate. It is not intended to replace a complete BGP course. Instead, it provides simple hands-on labs that show how iBGP and eBGP behave in real configurations.

The labs start with basic BGP peering and gradually build toward route advertisement, redistribution, path selection, and troubleshooting. Each lab focuses on a small part of BGP, making the behavior easier to observe, test, and compare.

Where useful, the same scenario is demonstrated with both iBGP and eBGP. This helps highlight which concepts are shared between them and where their behavior starts to differ.

The examples are based on a FortiGate multi-VDOM lab topology, where each VDOM acts as a separate router. This setup is designed for learning, testing, and documenting BGP behavior in a controlled environment.

Before starting with the labs, I recommend reviewing the **Lab Baseline** and **Lab Tips & Tricks** chapters. After that, you can dive directly into the individual labs.

### Requirements

The lab can run on almost any FortiGate model, either as physical hardware or as a virtual FortiGate appliance.

The main requirement is support for multiple VDOMs. The baseline topology uses ten VDOMs, with each VDOM acting as a separate router. When using a virtual FortiGate, make sure your license supports at least ten VDOMs.

Basic CLI access is recommended, since most labs use CLI commands to configure BGP, verify routing tables, and troubleshoot neighbor states.
