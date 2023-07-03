# GCC Docker Image

This repository contains a Dockerfile to build a Docker image based on Alpine Linux with a specific version of GCC (GNU Compiler Collection). It provides a convenient way to set up a containerized development environment with the desired version of GCC installed.

## Usage

To use this Docker image, you can follow these steps:

1. Install Docker on your system if you haven't already.
2. Clone this repository to your local machine:

   ```shell
   git clone <repository-url>
   ```
3. Navigate to the cloned repository:

   ```shell
   cd gcc-docker-image
   ```

4. Build the Docker image using the following command, specifying the desired version of GCC using the --build-arg option:

   ```shell
   docker build --build-arg GCC_VERSION=<desired-version> -t gcc-image .
   ```
   Replace <desired-version> with the specific version of GCC you want to install. You can find the available versions on the official GCC FTP mirror: [GCC FTP Mirror](https://ftp.task.gda.pl/pub/gnu/gcc/).

   For example, to install GCC version 10.1.0, the command would be:

   ```shell
   docker build --build-arg GCC_VERSION=10.1.0 -t gcc-image .
   ```
   
   This command will build the Docker image based on the provided Dockerfile, passing the desired GCC version as a build-time argument, and tag it as gcc-image.

5. Run a Docker container based on the built image:

   ```shell
   docker run -it gcc-image
   ```

   This command will start a new container based on the gcc-image and provide an interactive shell (/bin/sh) for you to work with.

6. Once inside the container, you will have access to the specified version of GCC. You can compile and run your C/C++ programs using the available GCC compilers.

## Dockerfile Details

The Dockerfile performs the following steps:

1. Sets the base image to the latest version of Alpine Linux.
2. Installs the necessary dependencies for building GCC.
3. Sets the working directory to /tmp.
4. Downloads and extracts the specified version of GCC source code from the official GCC FTP mirror.
5. Downloads additional prerequisites using the provided download_prerequisites script.
6. Configures the build process with the desired options, such as installation prefix, language support, and library configurations. The build is configured using the configure script provided by GCC. The configure script accepts various options to customize the build process. In this case, the options used include --prefix=/usr/local/gcc-${GCC_VERSION} to specify the installation prefix, --disable-multilib to disable building multiple target libraries, --disable-nls to disable Native Language Support, --disable-bootstrap to disable building a bootstrap compiler, --disable-libsanitizer to disable building the libsanitizer library, and --enable-languages=c,c++ to enable building the C and C++ compilers.
7. Builds GCC using the make -j$(nproc) command. The -j$(nproc) option is used to utilize multiple cores for faster compilation.
Installs GCC using the make install command.
8. Cleans up temporary files and directories.
9. Sets the default command to /bin/sh for running the container.

Please note that this Dockerfile is based on the instructions found in the provided references. Adjustments may be required based on your specific requirements.

## Development Container

This project includes a devcontainer.json file that is compatible with Visual Studio Code's Remote - Containers extension. The devcontainer.json file provides configuration for the development container, allowing you to develop your C++ projects within a consistent and reproducible environment.

To use the development container, follow these steps:

1. Make sure you have Visual Studio Code installed on your system.

2. Install the Remote - Containers extension for Visual Studio Code.

3. Clone this repository to your local machine:

   ```shell
   git clone <repository-url>
   ```
4. Open the cloned repository in Visual Studio Code.

5. When prompted, click on the "Reopen in Container" button that appears at the bottom right corner of the window. If the prompt does not appear, you can open the command palette (Ctrl/Cmd + Shift + P) and search for the "Remote-Containers: Reopen in Container" command.

6. Visual Studio Code will rebuild the Docker image based on the provided Dockerfile (Dockerfile.gcc) and start a new container with the specified version of GCC installed.

7. Once the development container is up and running, you can start working on your C++ projects within the container.

8. The devcontainer.json file includes configurations for popular extensions such as ms-vscode.cmake-tools and ms-vscode.cpptools. These extensions provide CMake integration and C++ language support, respectively. You can further customize the extensions list in the devcontainer.json file to suit your specific requirements.

9. By default, the development container is configured with the CMake build type set to "Debug". You can modify this configuration in the devcontainer.json file under the settings.cmake.configureSettings section.

10. You can mount your local workspace folder into the container by configuring the workspaceMount property in the devcontainer.json file. This allows you to access and work with your project files within the container.

## Rebuilding the Development Container

If you make changes to the Dockerfile or the devcontainer.json file, you can rebuild the development container by following these steps:

1. Open the command palette in Visual Studio Code (Ctrl/Cmd + Shift + P).
2. Search for the "Remote-Containers: Rebuild and Reopen in Container" command and select it.
2. Visual Studio Code will rebuild the Docker image and recreate the development container based on the updated configurations.

Please note that the devcontainer.json file is designed to work with the Visual Studio Code Remote - Containers extension. Adjustments may be required if you are using a different development environment.

## References

- [Alpine Linux](https://alpinelinux.org/): The official website of Alpine Linux, a lightweight Linux distribution used as the base image for the Dockerfile.
- [GCC - Installing GCC](https://gcc.gnu.org/wiki/InstallingGCC): The official GCC documentation on installing GCC, providing details on the various configuration options and build process.

- [GCC FTP Mirror](https://ftp.task.gda.pl/pub/gnu/gcc/): The FTP mirror providing various versions of GCC for download.

- [Building GCC - Solarian Programmer](https://solarianprogrammer.com/2016/10/07/building-gcc-ubuntu-linux/): A tutorial on building GCC on Ubuntu Linux, which provides insights into the general build process of GCC.

- [Visual Studio Code Remote - Containers extension](https://code.visualstudio.com/docs/devcontainers/containers): The official documentation for the Visual Studio Code Remote - Containers extension, which allows you to develop within a containerized environment using Visual Studio Code.

Please note that the Alpine Linux base image used in the Dockerfile may have some differences compared to other Linux distributions. It is known for its lightweight nature and minimalistic package management system (apk). Therefore, some features or specific configurations that work on other Linux distributions may require additional adjustments or may not be applicable in the Alpine Linux environment.

## License

This repository is licensed under the MIT License. See the [LICENSE.md](LICENSE.md) file for more information.