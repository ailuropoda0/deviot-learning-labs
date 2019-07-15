# Build gateways using SDK
We will look into how SDK is used in sample classes of Starter-kit.

### Cpu
Cpu class measures the usage of CPU and memory from a device, and it works as an input component in DevIoT. Cpu uses 'psutil' package, so you may need to install 'psutil' package.

#### Constructor
```
class Cpu(Thing):
    def __init__(self, tid, name):
        Thing.__init__(self, tid, name, "cpu")
        self.add_property(Property(name="CPU_usage", unit="%"))
        self.add_property(Property(name="memory_usage", unit="%"))
```
You can see that *Cpu* inherits **Thing** class on the first line. So *Cpu* gets methods and variables from Thing class.

In *Cpu* constructor, it takes *tid* (the ID of thing) and *name*(display name), which are used in Thing constructor. Then it adds properties of "CPU_usage" and "memory_usage" by using *add_property* method. I recommend you to use underscore('_') instead of space(' ') when you name properties.

#### update method
```
    def update_state(self):
        cpu = psutil.cpu_percent()
        memory = psutil.virtual_memory().percent
        self.update_property(CPU_usage=cpu, memory_usage=memory)
```
*update_state* is the method to update properites of *Cpu*. The name must be *update_state* to run main.py. When updating properties, you can use *update_property* method of *Thing* class. It takes keywords arguments. The way to use keywords arguments is like the code below. And I used the functions of psutil package to get cpu usage and memory usage. 'psutil' provides APIs to get the current status of CPU and memory.
```
thing.add_property([property name]=[updated value])
or
thing.add_property(**{[property name]:[updated value]})
```

This is all we need in order to build an input component. (1) Add properties (2) define the method to update properties.


### Mock_beeper
Mock_beeper works as an output component, printing "BEEP" periodically for a specific amount of time. 

#### Constructor
```
class Mock_beeper(Thing):
    def __init__(self, tid, name):
        Thing.__init__(self, tid, name, "mock")
        self.add_action(Action("beep").
                        add_parameter(Property(name="duration", type=PropertyType.INT, value=10, range=[10, 100])).
                        add_parameter(Property(name="interval", type=PropertyType.INT, value=1, range=[1, 10])))
```
*Mock_beeper* constructor takes *tid, name* as arguments like Cpu. Then it adds Action instance called "beep". Because "beep" action needs two variables, duration and interval, *add_parameter* method is used to add properties to action.

#### method
```
    def beep(self, duration, interval):
        if not isinstance(duration, int):
            duration = int(duration)
        if not isinstance(interval, int):
            interval = int(interval)
        elapsed_time = 0
        while elapsed_time < duration:
            logger.info("[BEEP]")
            time.sleep(interval)
            elapsed_time += interval
```

After adding Action instance, the method with the same name with the added Action should be defined. Because of the limitation of method name, the name cannot include space ' ' and cannot overlap with other methods like *update_property* and *add_property*.

*beep* method is the what *Mock_beeper* will do if Action "beep" is called. Since Action has two parameters(properties), *duration* and *interval*, *beep* takes two arguments. *beep* method prints "[BEEP]" logs during *duration* at intervals of *interval*.


### Setting a JSON configuration file
As explained in Step 2, starter-kit uses 'things.json' as a config file. Each object within {} indicates an instance of Thing class (or the class derived from Thing). **type** should be the name of a Thing class file, **name** is the name displayed on DevIoT. **options** is for the additional setting when you make the custom Thing class. But 'options' is not necessary. You can omit it or left with {}. 
```
[
  {
    "type": "mock_beeper",
    "name": "Py-Mock 1",
    "options": {}
  },
  {
    "type": "cpu",
    "name": "CPU"
  }
]
```

### main.py
You don't have to modify main.py. But if you want to change the name of a JSON file or a folder, then you should change it in main.py.
```
gateway.load('things.json', 'things')
```
*gateway.load* method is the method to read a JSON config file and custom Thing classes. The first argument is the location of a config file relative to main.py. The second argument is the name of a directory containing defined Thing classes. If the second argument is '', it means classes are in the same directory with main.py.
