# IFSE

## OverView

IFSE (**I**ntegrating **F**uzz Solving with **S**ymbolic **E**xecution) is an open-souce tool that incorporates various existing techniques to integrate fuzz solving with symbolic execution. As far as we know, IFSE is **the first open-source** SE tool which integrates fuzz solving to address the challenges arised by External Functions(EFs) in real-world applications.




## Tool Setup

We have packaged all the resources related to IFSE into Docker, including the source code of IFSE and the relevant resources required for reproducing the experiments. You can setup IFSE under the following instructions:

### 1. Install Docker

Docker provides tools for deploying applications within containers. Containers are (mostly) isolated from each other and the underlying system. This allows you to make a container, tinker with it and then throw it away when you’re done without affecting the underlying system or other containers.

A Docker container is built from a Docker image. A Docker image encapsulates an application, which in this case is KLEE. This application-level encapsulation is useful because it provides a “portable” and reproducible environment in which to run IFSE.

Follow these links for installation instructions on [Ubuntu](https://docs.docker.com/engine/install/ubuntu/), [OS X](https://docs.docker.com/desktop/install/mac-install/) and [Windows](https://docs.docker.com/desktop/install/windows-install/).

### 2. Pull docker image

We have packaged and pushed the docker image of IFSE to DockerHub, you can pull the docker image by following instruction:

```sh
docker pull achilles0425/ifse-image:stable
```

 This command builds a Docker image named `ifse-image`, which contains the complete runtime environment, source code and evaluation scripts. 

If the image is pulled successfully, you can use the following command to have a check. You should find that an image named `ifse-image` exists.

```
$ docker images
REPOSITORY                                     TAG       IMAGE ID       CREATED         SIZE
achilles0425/ifse-image                        stable    a78bcd0cfa67   2 months ago    11.3GB
```

### 3. Run docker image

To run the image, use the following command:

```sh
docker run -it ifse-image:stable
```



## Tool environment

The following items can be found in the docker container:

- `Coreutils-test` is the experiment directory, which contains:
  - `coreutils-9.4-bc`: All compiled byte code files of programs in CoreUtils-9.4 and  all scripts for reproducing evaluation.
  - `Coreutils-9.4-src`: Source code of all programs in CoreUtils-9.4.
  - `README.md`: Guide on how to reproduce our experiment.
- `Ifse` contains all source code and executable artifacts of our tool, including:
  - `build`: Compiled binary code of IFSE.
  - `klee`: The symbolic execution engine part in which we have extended over 4600 lines of C++ code.
  - `krpk`: A highly modularized fuzz solver that contains over 19000 lines of Rust code and can easily support complex theories like floating-point theory and different backend fuzzers.
  - `DEVELOPMENT.md`: Guide on how to recompile IFSE after modifying its source code.

TODO(根据打包出来的具体情况进行介绍)

## Experiment Detail

We evaluated IFSE on 79 programs in CoreUtils, a a widely used open-source core tool program collection in Unix-like operating system, to demonstrate IFSE's effectiveness when facing real-world applications. We compared IFSE with its baseline KLEE with 4 hours timeout and 8 seconds fuzz solver timeout and calculate the average branch coverage obtained in 10 repeated experiments. 

Due to limited space in the paper, we only presented the overall situation of the experiment and list some supplementary details as follows.

### Line Coverage

IFSE achieves a higher average line coverage for most of the programs (63 programs) ranging from relative 0.8\% to 217.2\% over KLEE and achieves an average line coverage of 54.8\% (while KLEE averaged 42.7\%), which demonstrates the path exploration ability of IFSE. Meanwhile, IFSE achieves relative higher branch coverage of 12.3\% over KLEE. The coverage details of all programs are as follows:

|      |      |      |
| ---- | ---- | ---- |
|      |      |      |
|      |      |      |
|      |      |      |

TODO

### Branch Coverage

As for branch coverage, IFSE achieves a higher average line coverage for most of the programs (51 programs) ranging from relative TODO\% to TODO\% over KLEE and achieves an average line coverage of 64.2\% (while KLEE averaged 57.7\%), which demonstrates the branch exploration ability of IFSE, the details are as follows:

TODO

### Optimizations

As an open-source tool, IFSE employs various optimization strategies to enhance its usability.  Among these strategies, the `splitter` and `predictor` hold relatively significant importance. The former focuses on identifying constraints that are likely to be unsolvable and immediately returning results, thus reducing unnecessary solving. The latter focuses on removing parts of the constraints that do not affect the solving result, thus reducing the search space for solving.

To study their impact on the performance of IFSE, we also conducted ablation experiments with four configurations: IFSE with neither, IFSE with predictor, IFSE with splitter and IFSE with both. In evaluating the 79 CoreUtils programs, the  results show that `splitter` improve the average branch coverage by relative 15.8\% and `predictor` improve the average branch coverage by 3.5\%. Using them together enhances the coverage by relative 23.5\%, indicating that the two optimazations are complementary as the `predictor` may assess the satisfiability of large constraints more accurately when these constraints are scaled down first by the `splitter`.   The following figure shows the branch coverage of 12 programs in CoreUtils with the largest coverage improment  in Table 2 under different configurations. Other programs shows similar trend.

![alt text](images/opt_cmp.jpg)

### Experiment Reproduction

You can refer to the `README.md` in `coreutils-test` to reproduce our experiment.