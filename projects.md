---
layout: page
title: Projects
link-state: projects

---

Listed in more or less reverse-chronological order.

# GN Hearing

## Package management for MATLAB
* Problem: MATLAB is the primary language and environment used by GN Hearing’s researchers. Unfortunately, it has no built-in package manager. Without one, it is difficult to practice code reuse, work with different versions of a tool, or reliably package up and transfer a setup from one machine to another.
* Approach: I designed a full package management system for MATLAB, based on the server-side on NPM, using a CouchDB database to store the package registry. I added an authentication wrapper to control access using our existing single sign-on system, and a daemon for backing up and restoring the registry with S3. I wrapped all of this in a Docker image. On the client side, I wrote a bootstrapping installer, and a full client for initializing, publishing, installing, and loading packages.
* Technologies used: AWS, Docker, Go, CouchDB, OAuth2, Java, MATLAB

## Multi-site data sharing and archiving
* Problems: GN Hearing conducts research across three countries and two continents. There was no good solution for efficiently storing and sharing large collections of data, however – nor for guaranteeing that retrieved data had not been altered since the previous experiment. This was an unacceptable situation for the research group.
* Approach: RAM, the Research Asset Manager. I designed a system for storing and sharing research data, with high guarantees of data integrity and reliability. I architected the overall system design, the use of a content-addressable store, and the HTTP APIs used for communication between servers and clients. I wrote clients for MATLAB, command-line use, and GUI desktop app use.
* Technologies used: Java, HTTP API design, API design, Java rich client (NetBeans platform), AWS S3.

## Acoustic measurement system
* Problem: To properly test hearing aid algorithms, it’s necessary to position a sound source at different positions relative to the listener – in front, behind, from the left side, above, and below. Moving the speaker manually, however, is error-prone, time-consuming, and difficult to replicate exactly. Scientists need a faster and more accurate solution.
* Approach: I developed a mechanical system that can position a speaker anywhere on a sphere of constant size, and a software system to control and use it. First I researched existing scholarly work and consulted with hearing scientists and mechanical engineers. I selected materials and designed a millimeter-accurate model of the setup using CAD software. I used an Arduino to communicate with the motor systems over UDP, and exposed a serial-port interface to MATLAB.
* Technologies used: Sketchup (CAD), mechanical design, t-slotted aluminum framing, acoustics, Arduino, UDP, API design.
* Notable: Was asked to develop a more refined and more powerful version several years later after first prototype was shipped to an academic partner.

## Psychoacoustic research hardware control library
* Problem: Scientists wrote very similar software for each experiment they ran. It was just different enough to prevent drop-in reuse.
* Approach: I consulted with hearing scientists to better understand their needs, and gained familiarity with the custom hardware and software used for multichannel audio presentation and acquisition. I determined the overlap between multiple codebases and built libraries that covered the common cases. As time went on, I responded to additional user needs.
* Technologies used: Java, threads, custom ActiveX API (TDT), MATLAB, API design, signal processing.
* Notable lifetime: 2010-present

## Soundbooth calibration
* Problem: For clinical trial data to be useful, the sound systems used need to be calibrated so that a hearing test always uses the same loudness, regardless of when and where it is administered.
* Approach: I consulted with domain experts to gain understanding of subject matter, and gained familiarity with the custom hardware and software used for calibration. I abstracted the necessary functionality, and built libraries with automated test-retest capabilities. I also built a cross-platform desktop app to aid in data visualization & analysis.
* Technologies used: Java, Java rich client (NetBeans platform), HTTP API design, custom serial port protocol (Larson Davis sound level meters).

## Audiology behavioral test suite
* Problem: Audiologists performed hearing tests for clinical trials manually, potentially leading to inconsistent results due to manual data entry & transfer.
* Approach: I interviewed audiologists to learn their workflows and needs. I designed a system to handle these hearing tests end-to-end. I built a Windows desktop client app that automated the tedious parts of administering these tests and stored results in a database, associated with patient and trial data. I observed audiologists using the application, obtained feedback, and iterated.
* Technologies used: Java, Java rich client (NetBeans platform), HTTP API design.
* Notable lifetime: 2008-2017.

## Miscellaneous
* Version Control: I started out using Subversion, but have used Git exclusively for the past five or six years. I use both GitHub and an internally-hosted GitLab instance.
* Unit Testing: For Java code, I use JUnit. For MATLAB code, I not only use the main xUnit-based framework, but also maintain it. See <https://github.com/psexton/matlab-xunit>
* Continuous Integration: I use Jenkins for running unit tests on every commit, and for running integration tests nightly.
* Deployment: I prefer to put servers and services in Docker containers, and run them on AWS EC2 instances. I use AWS IAM to give each service its own role with the minimum permissions required.

# Sandia National Labs

## Math libraries for high-performance computing
As a student, I developed linear algebra libraries that could automatically scale from a laptop with a single processor desktop to a supercomputer with thousands of nodes.
* Technologies used: C++ (including templates and STL), MPI, CVS.
* Website: <https://trilinos.org>
