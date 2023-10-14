# Robot Components API Documentation

[![License](https://img.shields.io/github/license/RobotComponents/RobotComponents-API-Documentation)]()
[![Closed Issues](https://img.shields.io/github/issues-raw/RobotComponents/RobotComponents-API-Documentation)]()
[![Open Issues](https://img.shields.io/github/issues-closed-raw/RobotComponents/RobotComponents-API-Documentation)]()
<a href="https://doi.org/10.5281/zenodo.5773814"><img src="https://zenodo.org/badge/DOI/10.5281/zenodo.5773814.svg" alt="DOI"></a>

This repository contains the content of [https://robotcomponents.github.io/RobotComponents-API-Documentation/index.html](https://robotcomponents.github.io/RobotComponents-API-Documentation/index.html). The API documentation page is built with [DocFX](https://dotnet.github.io/docfx/). 

Pull requests are welcome. If you want to build the site yourself, to test your changes before opening a pull request, then please check out the getting started guide below. 

## Getting started

We generate the API documentation with [docfx](https://dotnet.github.io/docfx/). The files are compiled and pushed manually. To update the docs, follow the next steps: 

1) Install docfx. You can find more information and the steps [here](https://dotnet.github.io/docfx/tutorial/walkthrough/walkthrough_create_a_docfx_project.html).
2) Pull the files from this repo via git. 
3) Update/replace the `.dll` and `.xml` files in the `src` folder. 
4) Use the command prompt and nagivate to the folder of this repo.
5) Build the new documentation pages by calling the command `docfx`. 
6) Push the updated files. 

## License
Copyright (c) 2018-2023 [The Robot Components authors and / or their affiliations](https://github.com/RobotComponents/RobotComponents/blob/master/AUTHORS.md)

Robot Components is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License version 3.0 as published by the Free Software Foundation. 

Robot Components is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with Robot Components; If not, see <http://www.gnu.org/licenses/>.

@license GPL-3.0 <https://www.gnu.org/licenses/gpl-3.0.html>
