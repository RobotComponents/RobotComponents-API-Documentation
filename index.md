# **Robot Components API Documentation**

Robot Components is a plugin for intuitive robot programming for ABB robots inside of Rhinoceros Grasshopper. Robot Components offers a wide set of tools to create toolpaths, simulate robotic motion and generate RAPID code within Grasshopper. Some of the main features include:

- 40+ predefined ABB robot models
- Possibility to add your own robot models
- Support for external axes (both linear and rotational)
- Possibility to define custom strategies for all external axis values
- Support for work objects (including movable work objects)
- Efficient forward and inverse kinematics
- Possibility to add your own custom code lines
- Real-time connection with IRC5 and OmniCore controllers
- Robot Components API to develop your custom components using either Python or C#

## **Getting Started**
You can download the latest release directly from our [Github release page](https://github.com/RobotComponents/RobotComponents/releases) and from [Food4Rhino](https://www.food4rhino.com/app/robot-components). For installation instructions, we refer you to our other [documentation website](https://robotcomponents.github.io/RobotComponents-Documentation/) where the Grasshopper plugin is documented (the visual programming interface). The purpose of this documentation website is to give an overview of the API references and to give examples on how to use the Robot Components API with IronPython and C#. The Robot Components API depends on the [RhinoCommon API](https://developer.rhino3d.com/api/RhinoCommon/html/R_Project_RhinoCommon.htm). If you want to use the API outside Rhino and Grasshopper (e.g. [Unity](https://unity.com/)) you need to run [Rhino.Inside](https://www.rhino3d.com/features/rhino-inside/) to be able to use the Robot Components API.

## **Overview of the namespaces**

The Robot Components API consists of the following main namespaces:

**Actions:** This namespace contains all the classes to generate the RAPID program code. These are the different declarations and instructions to create a RAPID program. In the visual interface of the Grasshopper plugin these classes and the objects that are generated from these classes are categorized as code generation. This namespace also includes the RAPID generator class. 

**Controllers:** This namespace contains classes and methods to interact with ABB controllers in real-time. 

**Definitions:** This namespace contains all the classes to define robots and robot tools that are needed for simulation and code generation.

**Kinematics:** This namespace contains the inverse and forward kinematics class, and the path generator class. 

**Presets:** This namespace contains the mechanical unit presets.

**Utils:** This namespace mainly contains helper methods that are used in all other namespaces. Most of these methods are also used in the utility components in the visual interface of the Grasshopper plugin. 

## **Credits**

Robot Components is an open-source project that was initiated by the chair of [Experimental and Digital Design and Construction of the University of Kassel](https://www.uni-kassel.de/fb06/institute/architektur/fachgebiete/experimentelles-und-digitales-entwerfen-und-konstruieren/home). The plugin is currently further developed and maintained by [Arjen Deetman](http://www.arjendeetman.nl). All developers and contributors are listed [here](https://github.com/RobotComponents/RobotComponents/blob/master/AUTHORS.md).

RobotComponents uses the ABB PC SDK for real-time connection to ABB Robots, you can find the .dll used in this project here: [ABB developercenter](http://developercenter.robotstudio.com/landing). 

Robot Components uses the OPW kinematics solver as described in the paper ['_An Analytical Solution of the Inverse Kinematics Problem of Industrial Serial Manipulators with an Ortho-parallel Basis and a Spherical Wrist_'](https://www.researchgate.net/publication/264212870_An_Analytical_Solution_of_the_Inverse_Kinematics_Problem_of_Industrial_Serial_Manipulators_with_an_Ortho-parallel_Basis_and_a_Spherical_Wrist) by Mathias Brandstötter, Arthur Angerer, and Michael Hofbaur.

We would like to acknowledge [Jose Luis Garcia del Castillo](https://github.com/garciadelcastillo) and [Vicente Soler](https://github.com/visose) for making their Grasshopper plugins [RobotExMachina](https://github.com/RobotExMachina) and [Robots](https://github.com/visose/Robots) available. Even though our approach is different it was helpful for us to see how you implemented certain functionalities and approached certain issues. 

## **License**

Copyright (c) 2018-2020 EDEK Uni Kassel\
Copyright (c) 2020-2024 Arjen Deetman

Robot Components is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License version 3.0 as published by the Free Software Foundation. 

Robot Components is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with Robot Components; If not, see <http://www.gnu.org/licenses/>.

@license GPL-3.0 <https://www.gnu.org/licenses/gpl-3.0.html>

