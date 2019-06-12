# Look into SDK
We will look into DevIoT SDK.

## The structure of SDK
If you have installed a DevIoT SDK package in python, you can import the SDK in any working directory. Here is the structure of the SDK.
```
cisco_deviot
|-gateway
|   |-Gateway
|-thing
|   |-Thing
|   |-Action
|   |-Property
|   |-PropertyType
|-logger
```
'**Gateway**' defines a gateway(device), which contains things and communicates with a DevIoT server.
'**Thing**' defines a component in a gateway, which contains either several properties(variables) or actions
'**Action**' defines an action or a command that a Thing can do
'**Property**' defines a variable measured by a Thing or used in an Action.
'**PropertyType**' has the types of property as constants. There are four types: INT, STRING, BOOL, COLOR.

You can import these classes by the following code.
```
from cisco_deviot.gateway import Gateway
from cisco_deviot.thing import Thing, Action, Property, PropertyType
from cisco_deviot import logger
```

## The classes of SDK
This is the summary of the [SDK document](https://github.com/ailuropoda0/gateway-python-sdk). How to use methods is explained in the next page with sample classes in starter-kit.

### Gateway
```
Gateway(name, deviot_server="deviot.cisco.com", connector_server="deviot.cisco.com:18883", kind="device", account="")
```
Gateway class takes 5 variables as arguments of its constructor. Only *name* is required. However, if you don't enter *account*, your gateway is opened to everyone in DevIoT. 

### Thing
```
Thing(id, name, kind=None)
```
Thing class has three instance variables: ***id***, ***name***, ***kind***. *id* is a unique ID of a Thing instance. *name* is a display name of an instance shown on DevIoT. *kind* is the kind of an instacne, which is for its icon.

An input component is a component getting variables like a light sensor. It takes variables(properties) by using the method *add_property*. An output component is a component doing some actions like LED. Actions are registered by using *add_action* method and defining methods.

A Thing instance can have both properties and actions. But the instance can act as either an input component or an output component in each project.

### Action
```
Action(name, **kwargs)
```
Action constructor takes 2 arguments: *name*, *\*\*kwargs*. Only *name* is mandatory and the method having the same name should be defined in a Thing class or instance. *\*\*kwargs* is for adding properties to Action.

### Property
```
Property(name, type=PropertyType.INT, value=None, range=None, unit=None, description=None)
```
Property takes six instance variables. *name* is the name of property shown on DevIoT. *type* is the data type of property. You can use PropertyType class. *value* is the initial value of property. *range* is a list of two limit value. *unit* and *description* should be string.  
