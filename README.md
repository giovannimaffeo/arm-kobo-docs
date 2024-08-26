# arm-kobo-docs

This repository contains detailed documentation and the source codes of the KoboToolbox repositories adapted for the ARM architecture, allowing the application to run on devices such as the Raspberry Pi 4. The solution was developed to provide a low-cost and efficient alternative for hardware-restricted environments while maintaining all the functionalities of the KoboToolbox.

## About KoboToolbox

KoboToolbox is an open-source platform widely used by humanitarian organizations, research institutes, and government institutions for field data collection, management, and analysis. Designed for environments with limited connectivity, the application supports both offline and online data collection, enabling the management of large volumes of information efficiently. Its main modules include tools for form creation, web interface data entry (Enketo), and integration with mobile applications (KoBoCollect). More information can be found in the [official KoboToolbox repository](https://github.com/kobotoolbox).

The adaptation of this solution for ARM aims to enable the platform to run on more accessible devices like the Raspberry Pi without compromising the robustness and functionality of KoboToolbox. This adaptation is particularly valuable in projects with limited resources or in contexts where traditional server usage might not be feasible.

## Overview

The objective of this repository is to provide a complete KoboToolbox environment with ARM architecture support. This includes the refactoring of various application modules and the recompilation of Docker images to ensure compatibility with ARM devices. Below is a list of the altered repositories and their respective functionalities.

### Altered Repositories and Modules

| Module             | Adapted ARM Repository                                             | Description                                                                                  |
|--------------------|----------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| kobo-install       | [arm-kobo-install](https://github.com/giovannimaffeo/arm-kobo-install) | Tool for configuring and orchestrating KoboToolbox containers.                              |
| enketo-express     | [arm-kobo-enketo-express](https://github.com/giovannimaffeo/arm-kobo-enketo-express) | Web interface for form filling.                                                             |
| kpi                | [arm-kobo-kpi](https://github.com/giovannimaffeo/arm-kobo-kpi)       | Module responsible for form creation and management.                                        |
| nginx-certbot      | [arm-kobo-nginx-certbot](https://github.com/giovannimaffeo/arm-kobo-nginx-certbot) | NGINX configuration with HTTPS support via Let's Encrypt.                                   |
| kobo-docker        | [arm-kobo-docker](https://github.com/giovannimaffeo/arm-kobo-docker) | Main repository with container and service configurations for KoboToolbox.                  |
| kobocat            | [kobocat (official repository)](https://github.com/kobotoolbox/kobocat) | Backend module responsible for storing responses. (No changes were necessary)               |

## Installation Instructions

### 3. Update Local Code

1. Create a directory named `kobo`.
2. Clone the adapted ARM repositories below:
   - [kobo-install](https://github.com/giovannimaffeo/arm-kobo-install)
   - [enketo-express](https://github.com/giovannimaffeo/arm-kobo-enketo-express)
   - [kpi](https://github.com/giovannimaffeo/arm-kobo-kpi)
   - [nginx-certbot](https://github.com/giovannimaffeo/arm-kobo-nginx-certbot)
   - [kobo-docker](https://github.com/giovannimaffeo/arm-kobo-docker)
   - [kobocat](https://github.com/kobotoolbox/kobocat) (official repository)

3. Within the Raspberry Pi, for each cloned repository, execute:
```bash
git checkout dev
```

### 4. Build ARM-Compatible Images

It will be necessary to compile the images of the following modules:
- enketo-express
- kpi
- kobocat

To build the images, run the commands below for each corresponding repository:
```bash
cd <repository>
docker build . -t <module-name>-arm
```

Ensure that the image names are exactly as shown to guarantee correct setup integration.

### 5. Configure KoboToolbox

1. Access the `kobo-install` directory:
```bash
cd kobo/kobo-install
```
2. Run the setup:
```bash
python3 run.py --setup
```

### Detailing Changes in KoboToolbox Modules

Some adaptations were necessary to ensure KoboToolbox runs on ARM architecture. These modifications primarily involve adjustments to Dockerfiles and Docker Compose files to ensure compatibility and stability of the application on devices like the Raspberry Pi. Below are the main changes made for each module.

#### 1. Enketo Express

In the Enketo Express module, responsible for the web interface for form filling, it was necessary to include the `chromium` package to ensure compatibility with the ARM architecture. This package is essential for running certain browser-based functionalities. The image shows the modification made in the module's Dockerfile, where the added line ensures the correct installation of this package.

![image 16](https://github.com/user-attachments/assets/bf00a24f-d610-4073-a4f0-c5f227ae17e3)

#### 2. Kobo Docker

In the main KoboToolbox repository, it was necessary to modify the Docker Compose files to replace the standard images with ARM-compatible images. The second and third images illustrate the change in image references for critical modules like `kobocat`, `kpi`, and `enketo-express` to their ARM versions.

![image 17](https://github.com/user-attachments/assets/6cf450ec-b210-44f1-8d52-aa28465ce0b9)

![image 18](https://github.com/user-attachments/assets/f98080b0-7b18-4a47-9754-aff1cb648f52)

#### 3. KPI

In the KPI module, used for creating and managing forms, it was necessary to manually add the installation of various packages to ensure the build would work correctly on ARM architecture. The fourth image highlights these additions to the KPI Dockerfile, ensuring all dependencies are correctly installed during the build process.

![image 19](https://github.com/user-attachments/assets/5f37881b-b951-4bd1-9654-133891976721)

#### 4. NGINX Certbot

For the NGINX Certbot module, which configures HTTPS support using Let’s Encrypt, adjustments were made to the initialization script to consider the subdomains configured for ARM architecture. The fifth image shows the adaptation of the settings to allow the application to function correctly in ARM environments.

![image 20](https://github.com/user-attachments/assets/ce343ff8-ae5b-4253-bb7d-0fc79bcbf817)

These changes ensure that all main KoboToolbox modules run stably on ARM devices, enabling their use in hardware-limited environments like the Raspberry Pi.

---

This repository provides a practical and efficient solution for those looking to run KoboToolbox on ARM architecture while maintaining the same functionality as the original architecture.
