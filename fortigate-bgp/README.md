# Overview

This lab guide is a practical notebook-style introduction to BGP on FortiGate. The goal is not to replace a full BGP course, but to provide simple, hands-on labs that help explain how iBGP and eBGP behave in real configurations.

The labs start with basic BGP peering and gradually build toward route advertisement, redistribution, path selection, and troubleshooting. Each lab focuses on a small part of BGP so the behavior is easy to observe, test, and compare.

Where useful, the same scenario is shown with both iBGP and eBGP. This makes it easier to understand which concepts are shared between them, and where their behavior starts to differ.

The examples are based on a FortiGate multi-VDOM lab topology, with each VDOM acting as a separate router. The setup is intended for learning, testing, and documenting BGP behavior in a controlled environment.

