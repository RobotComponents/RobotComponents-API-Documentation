# **C# example**

```csharp
// System Libs
using System;
using System.Collections;
using System.Collections.Generic;
// Rhino Libs
using Rhino.Geometry;
// Robot Components Libs
using RobotComponents.ABB.Actions;
using RobotComponents.ABB.Actions.Declarations;
using RobotComponents.ABB.Actions.Instructions;
using RobotComponents.ABB.Actions.Dynamic;
using RobotComponents.ABB.Definitions;
using RobotComponents.ABB.Enumerations;
using RobotComponents.ABB.Presets;
using RobotComponents.ABB.Presets.Enumerations;
```

```csharp
// Construct a tool
Plane tcp = new Plane(new Point3d(0, 0, 250), new Vector3d(0, 0, 1));
Cone cone = new Cone(tcp, -tcp.Origin.Z, 55);
Mesh coneMesh = Mesh.CreateFromCone(cone, 100, 100);
RobotTool tool = new RobotTool("tool1", coneMesh, Plane.WorldXY, tcp);

// Construct a robot preset
Robot robot = Factory.GetRobotPreset(RobotPreset.IRB4600_40_255, Plane.WorldXY, tool);

// Fabrication settings
SpeedData speedLow = new SpeedData("speed_low", 20);
SpeedData speedHigh = new SpeedData("speed_high", 250);
ZoneData zonePrecise = ZoneData.GetPredefinedZoneData(PredefinedZoneData.fine);
ZoneData zoneFlyover = ZoneData.GetPredefinedZoneData(PredefinedZoneData.z10);
double offset = 100;
string digitalOutputName = "DO_01";

// Create actions
List<RobotComponents.ABB.Actions.IAction> actions = new List<RobotComponents.ABB.Actions.IAction>();

// Configuration control
actions.Add(new JointConfigurationControl(true));
actions.Add(new LinearConfigurationControl(false));

// Home position
RobotJointPosition robotJointPosition = new RobotJointPosition(0, 0, 0, 0, 45, 0);
JointTarget homeTarget = new JointTarget(robotJointPosition);
Movement goHome = new Movement(MovementType.MoveAbsJ, homeTarget, speedHigh, zoneFlyover);
actions.Add(goHome);

// Declare variables
Vector3d vector = new Vector3d(0, 0, 0);

// Follow curves
for (int i = 0; i != curves.Count; i++)
{
    // Reverse one another
    if (i % 2 == 0)
    {
    curves[i].Reverse();
    }

    // Curve parameters
    double[] t = curves[i].DivideByCount(10, true);

    // Comments
    actions.Add(new CodeLine(" ", CodeType.Instruction));
    actions.Add(new Comment("New curve", CodeType.Instruction));

    // Follow curves with precise linear movements
    for (int j = 0; j != t.Length; j++)
    {
    double u;
    double v;
    Point3d point = curves[i].PointAt(t[j]);
    surface.ClosestPoint(point, out u, out v);

    Vector3d xAxis = curves[i].TangentAt(t[j]);
    Vector3d zAxis = surface.NormalAt(u, v);
    Vector3d yAxis = Vector3d.CrossProduct(xAxis, zAxis);
    Plane plane = new Plane(point, xAxis, yAxis);

    // Above start
    if (j == 0)
    {
        vector = -plane.ZAxis;
        vector.Unitize();
        Plane planeStart = new Plane(plane.Origin, plane.XAxis, plane.YAxis);
        planeStart.Translate(vector * offset);

        RobotTarget targetStart = 
            new RobotTarget(String.Format("curve_{0}_start", i), planeStart);
        Movement movementStart = 
            new Movement(MovementType.MoveJ, targetStart, speedHigh, zoneFlyover);
        actions.Add(movementStart);
    }

    RobotTarget target = 
        new RobotTarget(String.Format("curve_{0}_{1}", i, j), plane);
    Movement movement = 
        new Movement(MovementType.MoveL, target, speedLow, zonePrecise);
    actions.Add(movement);

    // Enable digital output after start
    if (j == 0)
    {
        actions.Add(new SetDigitalOutput(digitalOutputName, true));
    }
        // Disable digital output after last target and offset above end
    else if (j == t.Length - 1)
    {
        // Disable digital output
        actions.Add(new SetDigitalOutput(digitalOutputName, false));

        // Above end
        vector = -plane.ZAxis;
        vector.Unitize();
        Plane planeEnd = new Plane(plane.Origin, plane.XAxis, plane.YAxis);
        planeEnd.Translate(vector * offset);

        RobotTarget targetEnd = 
            new RobotTarget(String.Format("curve_{0}_end", i), planeEnd);
        Movement movementEnd = 
            new Movement(MovementType.MoveL, targetEnd, speedLow, zonePrecise);
        actions.Add(movementEnd);
    }
    }
}

// Home position
actions.Add(goHome);

// Create the RAPID program
RAPIDGenerator rapidGenerator = new RAPIDGenerator(robot, "MainModule", "main");
List<string> program = rapidGenerator.CreateModule(actions);
```
