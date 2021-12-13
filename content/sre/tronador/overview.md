# Overview

Tronador is a utility designed to provision and manage a dynamic testing environment for applications based on certain events such as a pull request on a git repo. In such a case, tronador should provision an environment in a specific namespace, and then deploy the application with the changes present in the branch to that namespace. This provides a way to test the application without having to manually deploy everything by yourself.

This allows the developer and tester to save valuable time, that would have been wasted on deploying everything manually.