# **Python example**

```python
# Import Rhino.Geometry
import Rhino.Geometry as rg

# Import Robot Components
import clr
## Import Robot Components
clr.AddReference("RobotComponents.ABB.dll")
import RobotComponents.ABB as rc
from RobotComponents.ABB.Enumerations import *
## Import presets
clr.AddReference("RobotComponents.ABB.Presets.dll")
import RobotComponents.ABB.Presets as pr
from RobotComponents.ABB.Presets.Enumerations import *

# Construct a tool
tcp = rg.Plane(rg.Point3d(0, 0, 250), rg.Vector3d(0, 0, 1))
cone = rg.Cone(tcp, -tcp.Origin.Z, 55)
cone_mesh = rg.Mesh.CreateFromCone(cone, 100, 100)
tool = rc.Definitions.RobotTool("tool1", cone_mesh, rg.Plane.WorldXY, tcp)

# Construct a robot preset
robot = pr.Factory.GetRobotPreset(RobotPreset.IRB4600_40_255, rg.Plane.WorldXY, tool)

# Fabricatoin settings
speed_low = rc.Actions.Declarations.SpeedData("speed_low", 20)
speed_high = rc.Actions.Declarations.SpeedData("speed_high", 250)
zone_precise = rc.Actions.Declarations.ZoneData.GetPredefinedZoneData(PredefinedZoneData.fine)
zone_flyover = rc.Actions.Declarations.ZoneData.GetPredefinedZoneData(PredefinedZoneData.z10)
offset = 100
do_name = "DO_01"

# Create the actions
actions = []

# Configuration control
actions.append(rc.Actions.Instructions.JointConfigurationControl(True))
actions.append(rc.Actions.Instructions.LinearConfigurationControl(False))

# Home position
rob_joint = rc.Actions.Declarations.RobotJointPosition(0, 0, 0, 0, 45, 0)
home_target = rc.Actions.Declarations.JointTarget(rob_joint)
go_home = rc.Actions.Instructions.Movement(MovementType.MoveAbsJ, home_target, speed_high, zone_flyover)
actions.append(go_home)

# Follow curves
for i in range(len(curves)):
    
    ## Reverse one another
    if (i % 2): 
        curves[i].Reverse()
       
    ## Curve parameters
    t = curves[i].DivideByCount(10, True)
    
    ## Comments
    actions.append(rc.Actions.Dynamic.CodeLine(" ", CodeType.Instruction))
    actions.append(rc.Actions.Dynamic.Comment("New curve", CodeType.Instruction))
    
    ## Follow curves with precise linear movements
    for j in range(len(t)):
        point = curves[i].PointAt(t[j])
        dum, u, v = surface.ClosestPoint(point)
        
        x_axis = curves[i].TangentAt(t[j])
        z_axis = surface.NormalAt(u, v)
        y_axis = rg.Vector3d.CrossProduct(x_axis, z_axis)
        plane = rg.Plane(point, x_axis, y_axis)
        
        ## Above start
        if (j == 0):
            vector = -plane.ZAxis
            vector.Unitize()
            plane_start = rg.Plane(plane.Origin, plane.XAxis, plane.YAxis)
            plane_start.Translate(vector * offset)
            
            target = rc.Actions.Declarations.RobotTarget("curve_{}_start".format(i), plane_start)
            movement = rc.Actions.Instructions.Movement(MovementType.MoveJ, target, speed_high, zone_flyover)
            actions.append(movement)
        
        target = rc.Actions.Declarations.RobotTarget("curve_{}_{}".format(i, j), plane)
        movement = rc.Actions.Instructions.Movement(MovementType.MoveL, target, speed_low, zone_precise)
        actions.append(movement)
        
        ## Enable digital output
        if (j == 0):
            actions.append(rc.Actions.Instructions.DigitalOutput(do_name, True))
    
    # Disable digital output
    actions.append(rc.Actions.Instructions.DigitalOutput(do_name, False))
    
    ## Above end
    vector = -plane.ZAxis
    vector.Unitize()
    plane_end = rg.Plane(plane.Origin, plane.XAxis, plane.YAxis)
    plane_end.Translate(vector * offset)
            
    target = rc.Actions.Declarations.RobotTarget("curve_{}_end".format(i), plane_end)
    movement = rc.Actions.Instructions.Movement(MovementType.MoveL, target, speed_low, zone_precise)
    actions.append(movement)
    
# Home position
actions.append(go_home)

# Create the RAPID program
rapid_generator = rc.Actions.RAPIDGenerator(robot, actions, "MainModule", "main")
program = rapid_generator.CreateModule()
```